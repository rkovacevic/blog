title: Splitting strings with regex
date: 2015-10-26 11:42:09
tags: [string,regex]
---

I needed to split a string by ":" delimiter, but ignore the delimiter, if it's followed by "!". For example, given a string:

> "Don't :! split this : but split this : and this"

it should return:

> ["Don't :! split this ", " but split this ", " and this"]

I generally try my hardest to avoid regex, due to it's unreadability and unmaintainability, but in this case I lapsed. I was able to find the appropriate regex expression with this tool:

[http://www.pagecolumn.com/tool/regtest.htm](http://www.pagecolumn.com/tool/regtest.htm)
 
** x(?=y) ** - matches x only if x is followed by y
** x(?!y) ** - matches x only if x is not followed by y

So, for my problem, I needed this regex:

> :(?!!)

Or in javascript: 

{% codeblock lang:javascript %}
var array = string.split(/:(?!!)/g)
{% endcodeblock %}
