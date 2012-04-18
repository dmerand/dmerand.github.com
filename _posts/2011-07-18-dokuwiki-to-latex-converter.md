---
layout: post
title: DokuWiki to LaTeX Converter  
category: Code
tags: dokuwiki latex cli  
---

Up until recently I was taking all of my personal notes in [dokuwiki](http://dokuwiki.org) format, because that's what we use at my job for our internal wiki. It's convenient to be able to copy notes straight into the wiki without having to convert. 

Sometimes, however, I would want to print my notes. Since I've always been a fan of the beautiful output of [LaTeX](http://www.latex-project.org/), and I wanted to work on my [sed](https://secure.wikimedia.org/wikipedia/en/wiki/Sed) and [awk](https://secure.wikimedia.org/wikipedia/en/wiki/AWK) chops, I wrote a utility to convert from DokuWiki to LaTeX based on regular expressions.

I've stopped using this code on my own computer in favor of a [pandoc](http://johnmacfarlane.net/pandoc/) solution, which I'll detail in a later post.

The script was originally one long BASH script which included calls to `cat`, `sed`, and `awk`. However, for the purposes of illustration I'm going to just show each important piece. If you want to download the complete script, you can get it [here](http://donaldmerand.com/files/1312559520/doku2tex.sh)



The first part of the script is to wrap the document in a (xe)LaTeX header and footer. I chose xelatex because it can handle Unicode fonts. That's the reason why I was using Lucida Grande as my font face, but any Unicode font would work.

{% highlight tex %}
\documentclass{article}
\pagestyle{empty}
\setlength{\parindent}{0pt}
\setlength{\parskip}{1ex plus 0.5ex minus 0.2ex}
\usepackage{ulem}
\usepackage{fontspec,xunicode}
\defaultfontfeatures{Mapping=tex-text}
\setromanfont[Mapping=tex-text]{Gill Sans}
\newfontfamily\specialfont{Lucida Grande}
\setcounter{secnumdepth}{-1}
\begin{document}
{% endhighlight %}

..then comes the code for the conversion. Then the footer:

{% highlight tex %}
\end{document}
{% endhighlight %}

The conversion code has two parts: an awk script to parse multi-line elements, such as list items, and a sed script to parse single-line conversions. The awk script is as follows:

{% highglight awk %}
/^        [\*|-] / { 
	sub(/^        [\*|-] /, "\t\t\t\\item ")
	if (list4) { print $0 } else { 
		list4 = 1
		print "\t\t\t\\begin{itemize}"
		print $0
	}
	next
}
list4 {
	list4=0
	print "\t\t\t\\end{itemize}"
}
/^      [\*|-] / { 
	sub(/^      [\*|-] /, "\t\t\\item ")
	if (list3) { print $0 } else { 
		list3 = 1
		print "\t\t\\begin{itemize}"
		print $0
	}
	next
}
list3 {
	list3=0
	print "\t\t\\end{itemize}"
}
/^    [\*|-] / { 
	sub(/^    [\*|-] /, "\t\\item ")
	if (list2) { print $0 } else { 
		list2 = 1
		print "\t\\begin{itemize}"
		print $0
	}
	next
}
list2 {
	list2=0
	print "\t\\end{itemize}"
}
/^  [\*|-] / { 
	sub(/^  [\*|-] /, "\\item ")
	if (list1) { print $0 } else { 
		list1 = 1
		print "\\begin{itemize}"
		print $0
	}
	next
}
list1 {
	list1=0
	print "\\end{itemize}"
}
#default case
{ print }
#catch itemize at the end of the file
END {	if (list1==1 || list2==2 || list3==2 || list==2) {
	print "\\end{itemize}" 
}
{% endhighlight %}

The results of the awk script get piped to a sed script which parses single-line items, such as bold, italic, etc:

{% highlight sed %}
s/======\(.*\)======/\\\section{\1}/
s/=====\(.*\)=====/\\\subsection{\1}/
s/====\(.*\)====/\\\subsubsection{\1}/
#remaining headers do not translate to latex. Just make them bold
s/==*\(.*\)==*/\\\textbf\{\1\}/g
#bold, underline, italic
s/\*\*\(.*\)\*\*/\\\textbf\{\1\}/g
s/__\(.*\)__/\\\underline\{\1\}/g
s|\/\/\(.*\)\/\/|\\\emph\{\1\}|g
#footnotes
s|((\(.*\)))|\\\footnote\{\1\}|g
#ellipses
s|\.\.\.|\\\ldots |g
#double-single quotes (remember LONGEST MATCH)
s|''\(.*\)''|\`\`\1''|g
#quote character
s|"\""\(.*\)"\""|\`\`\1''|g
#arrows
s|->|---|g
#underscores do not work unless escaped
s|\_|\\\_|g
#special cases for @done for strikethrough
s|item \(.*\)@done$|\\item \\\sout\{\1\}|g
s|\(.*\)@done$|\\\sout\{\1\}|g
#fractions, numbers
s| \([0-9][0-9]*\)/\([0-9][0-9]*\) | \$\\\frac\{\1\}\{\2\}\$ |g
s| \([0-9][0-9]*\) | \$\1\$ |g
s|\$ \$||g
#link. no special character at end. 
#matches file:///syntax, and allows spaces as a result
s?\(https*:\/\/[-a-zA-Z0-9.&_=/\\\?]*[a-zA-Z0-9]\)? \\\url\{\1\}?
s?file:\/\/\/\([-a-zA-Z0-9 .\/\\]*\)? \\\url\{\/\1\}?
{% endhighlight %}

So there you have it. It's very informative to take on a project of this size: you learn pretty quickly how big of a mess regular expressions can be. But that's the beauty of sed and awk - you can combine simple expressions until you have one big (ugly) program that does something significant. It's amazing the power of what you can do with a few basic utilities that are included by default on just about every non-windows machine on the planet (and are available for windows machines as well).

I also learned that most of what you can do with sed can be done right from inside the amazing text editor, [vim](http://www.vim.org/). An unintended consequence of learning sed is that you become a vim search/replace _master_. That alone is a great reason to learn how to use sed.
