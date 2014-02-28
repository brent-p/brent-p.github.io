---
layout: post
title:  "Find and Replace Strings in Ruby using Regular Expressions"
date:   2014-02-28 17:00:00
categories: blog
---

I have been working hard recently to get the next phase of Muchly(a complete rewrite from Java to Ruby as well as lots of new functionality) ready to be released on our much.ly domain and one of the things I have been putting off is parsing posts for hashtags and urls and turning them into links. For example, in a tweet if "#food" appears I want to turn this into `<a href="\search\food">food</a>` but if a url appears e.g. http://t.co I want to turn this into `<a href="http://t.co"> http://t.co</a>`. e.g.<br/>
<img src="/assets/post.png" />

So today I finally decided to tackle this task, last time in Java I looped through the string manually finding and replacing using substrings where this time in Ruby I want to try and see if I can accomplish the same thing using regular expressions. As it turns out this is actually really easy in Ruby however I wasn't able to find a full example in so I decided I would post one of my own.

	#Replace all Hashtags in text with links to /search/hashtag
	new_text = old_text.gsub(/#[a-zA-Z0-9]*/) do |match|
    	"<a href='\/search\/#{match[1..-1]}'>#{match}</a>"
    end

For input:

	old_text = "#fun some more words #photo#day"

The output would be:

	new_text = "<a href='search/fun'>#fun</a> some more words <a href='search/photo'>#photo</a><a href='search/day'>#day</a>"


The gsub method on string replaces all occurrences that match the regular expression, while the sub method will only replace the first occurence.


      #Regular expression definitions:
      # - anything between / and / is treated as a regular expression
      # - # match a single # character
      # - [a-zA-Z0-9] match any character that is either a-z, A-Z, or 0 - 9 i.e any letter or number
      # - * match 0 or more times of the previous expression in this case [a-zA-Z0-9]
      # - \ is an escape character e.g. \/ = /
      # - match[1..-1] means return string from position 1(positions start at 0) through to the end of
      # the string so "#hashtag"[1..-1] would return "hashtag" i.e without the # symbol

To parse for urls I did the following:

	 #Replace all links that start with http: or www. with link tags
     new_text = new_text.gsub(/(http:|www.)[a-zA-Z0-9\/:\.\?]*/) do |match|
     "<a href='#{match}'>#{match}</a>"
     end

For input:

	 old_text = "Apple to discuss misleading free-to-play apps http://t.co/ktovN7NlEa"

The output would be:

    new_text = "Apple to discuss misleading free-to-play apps <a href='http://t.co/ktovN7NlEa'>http://t.co/ktovN7NlEa'</a>"   

Now I am sure you can probably get a bit fancier and catch some other cases but for first pass this will be sufficient for now. I hope this helps, if you have any questions you can contact me via the details below.      

