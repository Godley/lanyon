---
title: Meal planner, revisited
layout: post
---
![]({{ site.url }}/images/2017/04/Good-Food-Guide-editor-calls-for-more-veggie-options-on-pub-menus.jpg)
About a year ago I wrote about [screen scraping the BBC Good Food website](http://charlottegodley.co.uk/making-a-shopping-list-when-youre-a-lazy-programmer/).
That turned out to be a lot harder than I initially thought and needless to say there's so much ambiguity in the data people are able to enter on there that I gave up.

I decided to try again recently because I now work on similar things at work and I've changed tact to use a manual approach for several reasons:

1. Screen scraping didn't work
1. humans are pretty picky about what they eat, so having a catalog of several hundred recipes is probably going to yield multiple "I don't like this selection" moments. This happens to me a lot when I use this [not-child-friendly website](http://www.whatthefuckshouldimakefordinner.com/index.php)
1. A lot of websites have really complicated recipes which either I'm too lazy to cook or Danny doesn't feel capable of doing.

With these three user requirements in mind, I started off by making a markdown curated git repo of the recipes Danny and I are both happy cooking/would like to eat. Yes, we are massive nerds. 
I moved on from there to dig out my old Django code and write an app that would store all of our data for both recipes and stock. 

In theory:

- A stock item is the smallest atomic model.
- An ingredient is directly mapped onto 1 stock item, but it's specific to each recipe as it contains the unit and quantity needed.
- Stock also maps onto stock item in a similar way. Doesn't need an instruction applied though and may have different units to recipes.

## Pain points
### Humans don't like standard units of measurements
A problem I'm now finding is that converting between units sucks, but I do a lot of recipes that are American and a few that are English. Since my scales kicked it a few weeks ago for some unknown reason, American recipes are now slightly less irritating. 
For reference, the difference is Americans use units which are volume, not weight based - i.e cups and tablespoons. When you first start off with this, it seems really alien and stupid, but it means:

- you don't need to own scales
- you measure by putting the cup or the spoon into the ingredient not by pouring it everywhere

**Note that by cup/spoon there are specific measurements for both. You can't just use any cup you have in your house**

![]({{ site.url }}/images/2017/04/cooking-measurement-set.jpg)

Further to US-UK issues, a lot of recipes call for 1 onion or 1 garlic clove, but the listings on various ordering sites tend to be in grams rather than number of items, but then if you need limes or lemons they tend to be in number of items?? Humans are weird.

TL;DR on that point - I like reading recipes in cups/tablespoons but not all of my recipes are in that unit, and when writing a system that's trying to work out how much you have left automatically and also tell you how much more you need to order it becomes difficult.

### None of you will give me an API
Tesco, Ocado, Sainsbury's, Morrisons don't give me an accessible API so I have to copy and paste the list of ingredients into their website. I can't be the only nerd using your website which wants this.

### I can't get small quantities of lots of items
In general online ordering allows me to search by a list of items, but I can't be specific and ask for a small portion of onion or 100g of chicken. This isn't such a big deal because generally I'll place an order which is for 2 weeks so I probably *will* order 1.5kg chicken to save putting that in my basket the following order, but as a couple not a family of 5 do I really need an entire bag of shallots? I don't even like them all that much...

The problem adds an extra layer of complexity in that my app only knows that in order to do the 2 weeks of recipes I've ordered, I need x amount more of this or x amount more of that. It doesn't know that I've just ordered 1.5kg of chicken so I can't really do a button on the page which gives me my shopping list which says "order" without having to manually enter how much I've ordered.

## TL;DR
I'm working on it, and so far it does take a lot of the stress out of planning my food deliveries, but I can't automate the whole process and that annoys me. Also it's [open source](https://github.com/Godley/MealPlanner) but don't judge me for having no tests, I'm picky enough about testing at work that writing it TDD style would take the fun out of doing it.