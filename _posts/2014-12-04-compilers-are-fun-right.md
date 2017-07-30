---
title: Compilers are fun, right?
layout: post
---
![]({{ site.url }}/images/2017/7/digital-1010101-310x168.jpg)
<font style="font-size:8pt">I couldn't find a suitable image, so I decided to take a leaf out of the BBC's book</font>

This week I handed in one of the most challenging, but most interesting pieces of Assessed Coursework I've done during my university career - an SPL Compiler, SPL being a "Simple Programming Language" made up by my lecturer.

This formed 50% of my grade for the module, Languages and their Compilers, and involved taking some simple roadmaps for the grammar of a language, SPL, converting them to BNF, taking the BNF and converting it to a lexical analyser, taking this and attatching rules to each lexeme, creating a parse tree from the rules, and finally, generating C code from the rules and optimising the C code outputted.

A few of those terms might have gone a bit over your head: 

* BNF stands for Backus-Naur Form: this is a language for describing a language in a recursive manner. So for example, a number:
 	* `<number> : <digit>|<digit><number>
    	<digit> : 0|1|2|3|4|5|6|7|8|9` 
* A lexical analyser takes in a line of code, and spits out the lexemes: these can be terminals or non-terminals. Terminals indicate a stopping point, so in the example above, digit is a terminal as there's no further steps required to figure out that "0" is a digit, but number would be a rule. The lexical analyser handles terminals. Note though that they can handle regular expressions, meaning rules such as:
 	`<identifier> : '<letters>'
    <letters> : <character> | <character><letters>
    <character> : a|b|c|d|etc`
reside firmly in the lexical analyser, because we don't need to know anything more about identifier, and anything beneath that can be figured out by applying a regular expression.

* A parser takes in a line of code, sends it through a lexical analyser, takes from that the tokens and figures out from how they fit together what rules to apply.

* A parse tree takes the rules generated and puts them into a tree data structure, so that you can go through each step looking for different tokens and rules.

Whilst I hit several brick walls along the way, particularly in the parser stage, thinking about language in this way has been interesting, and debugging code, particularly with optimisations in it, has proven challenging. I'm fairly pleased with my efforts and can't wait to see my score :)