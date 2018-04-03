---
layout: post
title: Checklist for Making Friendly Web Pages
category: code
tags: HTML CSS RSS
---

When I wrote the code for `donaldmerand.com`, I decided to try my best to adhere to good programming practices and standards for as many elements of the site as possible - the generated HTML, the CSS, the RSS feed, and everything else I could think of. Some people might say that _all_ code projects that _everybody_ does at _all times_ should adhere to best practices and be syntactically valid. People who say that have never worked in a production environment of any sort. That, or they work in very well-supported, well-managed, and well-funded programming environments and have few deadlines.

Here's what I found about web standards: good programming practice is a somewhat moving target, having just as much to do with æsthetic preference as any hard-and-fast rules. Syntactical validity is basically concrete in programming languages, but on the web it's different. For example, a [big point](http://diveintohtml5.org/past.html) of the HTML5 standard is to slyly, quietly deal with the poor syntax that's all over the web. If we _all_ wanted pure and perfect web standards, then we'd have all jumped on the XHTML 2.0 bandwagon. How many of us actually did?

`crickets...`

Having said all of that, there are some things you can do as a web programmer that most everybody agrees are Good Things, and you should probably be doing in Most of your Projects. There are also some things that _I_ say you should do, because I'm biased just like everybody else, and you're here to listen to me, right?

To spare myself (and maybe you) from having to remember all of those things when making my next site, I made a list. So here you go...

# Checklist for Making Friendly Web Pages

1. HTML4 should be validated by the [W3C HTML Validator](http://validator.w3.org/)
	- Don't bother with a <!DOCTYPE> of anything XHTML unless you're actually going to modify your headers to send a MIME type of `application/xhtml+xml`. As it turns out, if you send a regular `text/html` MIME-type, your page [isn't being parsed as XML at all](http://diveintohtml5.org/past.html#xhtml)
	- HTML5 should be validated with a [HTML5 validator](http://html5.validator.nu/)
	- Bonus points for passing [HTML lint](http://lint.brihten.com/html/)
1. If you're going with HTML5, you might want to check for browser compatibility eg. with [modernizr](http://www.modernizr.com/).
	- Also check [this link](http://diveintohtml5.org/everything.html) for single-item compatibility if you don't need more that a couple of HTML5 functions.
	- Also [this link](http://diveintohtml5.org/peeks-pokes-and-pointers.html) is a great resource for what's new in HTML5.
1. CSS should be validated by the [W3C CSS Validator](http://jigsaw.w3.org/css-validator/)
	- Bonus points for passing [CSS lint](http://csslint.net/)
	- Bonus points for [minifying](https://secure.wikimedia.org/wikipedia/en/wiki/Minification_%28programming%29) your CSS.
	- Bonus points for using [sass](http://sass-lang.com/) for your own CSS sanity.
	- _Double-extra_ bonus points for including style elements for different [media types](http://www.w3.org/TR/CSS2/media.html). For example, you might want to include a "print" stylesheet that hides your sidebar/navigation elements, so that when articles are printed, only the content comes out.
1. If you have _any_ sort of dynamic content (blog posts, etc.), you should have an [RSS/atom](https://secure.wikimedia.org/wikipedia/en/wiki/RSS) feed.
	- Your RSS feed should be validated by a [RSS Validator](http://feedvalidator.org/)
		- Though it's technically optional, you should include a `<link rel="alternate" type="application/rss+xml" href="/rss/" />` tag in your `<head>` section, to indicate to feed readers such as Google Reader that visit your main URL where the rss feed is (in this case a RSS 2.0 feed at http://yoursite.com/rss/).
		- Your feed needs to be in the proper XML format, obviously.
		- You have to encode your feed elements. For example, you can't say `<a href>`, you have to say `<a href>`. There's something called [CDATA](http://www.w3schools.com/xml/xml_cdata.asp) that makes this relatively straightforward if your web development language of choice doesn't have escape functions, and you don't feel like writing your own.
		- Your RSS links can't be relative - `/index.html` should be replaced with `http://yoursite.com/index.html`.
1. The site should have a [favicon](https://secure.wikimedia.org/wikipedia/en/wiki/Favicon). Use the [favicon generator](http://favicon-generator.org/) to get one from a regular image.
	- You should include a `<link rel="shortcut icon" href="/favicon.ico" />` in your `<head>` section to make sure this gets used. Some browsers (like Chrome) don't seem to need it, but it's better to be safe if you want people to see your nifty icon.
1. You should be licensing your content. See [creative commons](http://creativecommons.org/) for more details and a license-link generator.
1. You should be compressing your content. See [this article](http://betterexplained.com/articles/how-to-optimize-your-site-with-gzip-compression/). 
	- If you're using PHP, I'd recommend doing both the PHP compression _and_ the Apache compression. Apache compression doesn't seem to catch PHP files that serve HTML content. I'm guessing this is the same for other web scripting languages, but I'll admit my expertise in this area is limited.
1. You should have clean URL syntax. Try to avoid URLS like `http://mysite.com?somequery=value&some_other_query=some_othervalue`. Instead, go for `http://mysite.com/somequery/value`. Search engines love it, humans love it, everybody is happy.
    - [This site](http://quoderat.megginson.com/2007/02/15/rest-the-quick-pitch/) gives a good overwiew of the RESTful URL situation, and provides good guidelines on URL management.
    - ..and unless you're using a framework like [rails](http://rubyonrails.org/), you'll probably want to brush up on [this .htaccess primer](http://corz.org/serv/tricks/htaccess.php), because you'll need to know it to make your own custom URLs by hand. You can also learn other neat tricks, like [banning specific user agents](http://corz.org/serv/tricks/htaccess2.php#section-more_banning) or [preventing hot-linking](http://corz.org/serv/tricks/htaccess2.php#section-prevent_hot-linking).
1. You should probably have a page search. You can redirect to a google `search_term site:yoursite.com` query at the very least.
	- Bonus points if your search returns with [highlighting](http://beatgates.blogspot.com/2011/07/extend-jqueryhighlight-to-highlight.html). This might not be applicable, depending on how your search page returns results.
