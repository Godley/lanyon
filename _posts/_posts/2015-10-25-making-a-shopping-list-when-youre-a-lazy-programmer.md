---
title: Making a shopping list when you're a lazy programmer
layout: post
---
I'm a big fan of cooking. For me, it's super relaxing to just spend some time prepping, cooking, splitting the cooking wine between your pan and your glass (heheh), listening to some music and finally, eating what you've created.
<blockquote class="twitter-tweet" lang="en"><p lang="en" dir="ltr">I&#39;m mainly with <a href="https://twitter.com/charwarz">@charwarz</a> for her cooking ability. <a href="https://t.co/V0FUOIJm8T">pic.twitter.com/V0FUOIJm8T</a></p>&mdash; Daniel Brown (@DanTonyBrown) <a href="https://twitter.com/DanTonyBrown/status/658012687671119872">October 24, 2015</a></blockquote> <script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

As a programmer, though, there's a lot about preparing to cook that's tedious and which I was sure could be automated. It takes time to figure out what you want to make, then work out how much that's going to cost based on what's in your cupboard, and get out and buy the stuff, especially if you're doing something with meat that you will need to go to the butcher to get. I seriously don't get why our retail economy is based on the 9-5 pattern, when our office jobs are also based on this pattern - when the hell am I supposed to get the time to get midweek meat if the butcher closes before I've even got out of the science park?! I want to support local economies and in the Mill Road area this is particularly easy, but c'mon now. You're not making this easy for me.

Anyway. I use BBC Good food quite a lot because usually when I google an ingredient I want to use, that'll be the first thing that pops up. So I thought I'd try to make my own site or app which would allow me to automate how I browse BBC Good food, so that the app would know what I have in my kitchen, and give me the list of recipes that best match what I have, then produce a shopping list for my way out of work (ALDI is conveniently located near work).
I'd seen a working couple on reddit had made something similar, but with text files storing recipes and current stock instead of any kind of database. 
I'd really rather skip a lot of user entry and for that instance the couple had certain recipes they knew well and liked doing, which isn't the same in my case. I've never actually written a screen scraper before, so I thought I'd give it a try.

I started with [Django](http://djangoproject.com) as my framework because I've used it sparingly at work and I can see what the hype is about. I like being able to model my databases from in python without having to dive into SQL, and changes late on are really easy. I'll be explaining how everything works here, but it's assumed you already know a bit about setting up django and setting up scrapy.

My models are relatively simple. The atom, if you like, is a stock item. All this needs is a name, a quantity and a weight - this gives you options as to what you're using. E.g if it's lemons, you'd say how many as opposed to weight, but if it's flour etc. You get the point.
```
class Stock(models.Model):
    title = models.CharField(max_length=200)
    quantity = models.IntegerField(default=0)
    weight = models.IntegerField(default=0)
    recipes = models.ManyToManyField(Recipe)
```
This has a many to many relation with recipe, which has a title and instructions. This is likely to change when I've fixed other problems, because in addition to linking many to many to stock, we need to know *how much* of that stock the recipe should use, and how it'll be prepared, so it's likely I'll add another model to cover that.
```
class Recipe(models.Model):
    title = models.CharField(max_length=200)
    description = models.CharField(max_length=200)
```
Finally we have Menu, which is how I'm going to plan each week. This will have a many to many link to recipe.
```
class Menu(models.Model):
    recipes = models.ManyToManyField(Recipe)
    pub_date = models.DateTimeField('date published')
```
Next up I looked at screen scraping. There's really 2 favoured frameworks - [Beautiful Soup](http://www.crummy.com/software/BeautifulSoup/) and [Scrapy](http://scrapy.org). Scrapy feels very similar to Django and there's a wealth of tutorials on integrating it, probably because the creators were heavily inspired by it, so I went with Scrapy.

My first foure into scraping BBC Good Food was getting the ingredients. I figured this would be the easiest way to get a list of stock or expected stock for my cupboard without physically entering it myself, so that it's all in the db and I just have to update how much of each thing I have.
![]({{ site.url }}/images/2015/10/Screen-Shot-2015-10-25-at-11-22-30.png)
 Then when it comes to scraping each recipe's page, all I'd need to do was query the db for the collected ingredients and link them together. I also wasn't sure on exactly how things work together, so I wanted to do a simple scrape before delving into relational databases.

First off, I ran the scrapy command for generating a scraper. `scrapy genspider goodfood bbcgoodfood.com`.
This spits out:
```
# -*- coding: utf-8 -*-
import scrapy


class GoodFoodSpider(scrapy.Spider):
    name = "goodfood"
    allowed_domains = ["bbcgoodfood.com"]
    start_urls = (
        'http://www.bbcgoodfood.com/',
    )

    def parse(self, response):
        pass
```

I needed to change the following:

- put in a rule so the spider knows which pages to trawl into

- set up a callback for when that happens.

- Change the URL to be the ingredients page

- write the parse callback

And here we are:
```
# -*- coding: utf-8 -*-
from scrapy.contrib.spiders import CrawlSpider, Rule
from scrapy.contrib.linkextractors.sgml import SgmlLinkExtractor
from scrapy.selector import HtmlXPathSelector
from items import StockItem, RecipeItem
import sys
print sys.path
from meals.models import Stock
from scrapy.http import Request
import django




class GoodfoodSpider(CrawlSpider):
    name = "goodfood"
    allowed_domains = ["bbcgoodfood.com"]
    start_urls = (
        'http://www.bbcgoodfood.com/recipes/category/ingredients',
    )

    rules = (
        Rule(SgmlLinkExtractor(allow=(), restrict_xpaths=('//li[@class="views-row-odd views-row-first"]','//li[@class="views-row-even"]','//li[@class="views-row-odd"]', '//li[@class="views-row-odd views-row-last"]',)), callback="parse_items", follow= True),
    )
    def parse_items(self, response):
        hxs = HtmlXPathSelector(response)
        titles = hxs.xpath('//span[@class="field-content"]/text()')
        items = []
        for title in titles:
            item = StockItem()
            item["title"] = title.extract()
            items.append(item)
        return(items)


```
Basically what I did here was find the classes used by each link. This is where "restrict_paths" comes from. Every time scrap finds a link inside li, it will follow it and use the parse_items method to parse it.
Alongside the scraper, we need to store everything in Django. This is where we setup an item inside items.py:
```
import scrapy
from meals.models import Stock, Recipe
from scrapy_djangoitem import DjangoItem


class StockItem(DjangoItem):
    # define the fields for your item here like:
    # name = scrapy.Field()
    django_model=Stock
```
And a pipeline in pipelines.py so that scrap will know what to do with the item:
```
class GoodFoodStockPipeline(object):

    def process_item(self, item, spider):
        item.save()
        return item
```
The django project also needs to be in our python path, so that we can import from it. Do this in settings.py:
```
sys.path.append('/Users/charlottegodley/PycharmProjects/MealPlanner/DjangoMeals')
os.environ['DJANGO_SETTINGS_MODULE'] = 'DjangoMeals.settings'
```
Also add your pipeline somewhere in settings with:
```
ITEM_PIPELINES = {
   'pipelines.GoodFoodStockPipeline': 300
}
```
And run it from command line: `scrapy crawl goodfood`.
This blog's getting pretty long, so I'll be splitting this into several blogs which I'll publish every few days. In the meantime, the repo is [here](https://github.com/Godley/MealPlanner.git). Happy hacking!