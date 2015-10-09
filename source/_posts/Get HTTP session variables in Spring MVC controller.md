title: Get HTTP session variables in Spring MVC controller
date: 2011-01-20 14:13:00
tags: [http,spring mvc]
---

It took me a while today to figure out how to get a User object from the HTTP session in Spring MVC controller, but it's actually pretty simple:  

<pre class="brush: java">RequestContextHolder.currentRequestAttributes()  
.getAttribute("user", RequestAttributes.SCOPE_SESSION)  
</pre>

And that's it, just cast it to User and you're done.