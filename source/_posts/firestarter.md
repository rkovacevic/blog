---
layout: post
title: Playing with NodeJS
description: "Tales of making a Twitter clone with Node."
date: 2014/10/08
category: articles
tags: [nodejs, angular, firestarter, mongo,javascript]
image:
  feature: texture-feature-05.jpg
---

I used a lot of AngularJS in recent years, and some NodeJS, but never really had an opportunity to work on an all-javascript app, both on frontend and backend. It seems to be all the rage these days, so I decided to give it a whirl, and see how it moves. I made a small Twitter clone app, the "Hello world" of our time. 

You can see the app running here: [http://rkovacevic-firestarter.herokuapp.com/](http://rkovacevic-firestarter.herokuapp.com/)

And check out the source here: [https://github.com/rkovacevic/firestarter](https://github.com/rkovacevic/firestarter)

I was quite happy with Node and the expirience was fun. Here are some takeaways I ended up with:

## It's all been written before, several times

The amount of npm modules is huge. I recently read it surpassed maven. Whatever you could possibly need has been written already, in several ways, so don't just take the first one you find, shop around and you'll probably find another module that does the same thing, but better. 

http://npmsearch.com/ is a good resource for this.

## ...except form validation

As great as AngularJS is, form validation just sucks. If user is typing in an email, I'm sure he doesn't need an error message after just one typed in letter. Also, the notion that server might also exist and that it might also return some validation errors is completely absent. 

There are some libraries that attempt to solve this, and there are some improvements in Angular 1.3, but it seems like nobody cracked the problem in an elegant way yet. It's not a trivial thing to do right, I get the sense nobody has really taken it seriously enough, despite it being an essential part of many apps. 

In the end, I ended up taking angular-validator library (https://github.com/kelp404/angular-validator) and further modifying it for my needs. 

## Beware of bower

I used bower package manager for frontend libraries. For the most part, it works fine, but I get the feeling it's not quite ready for production yet. For example, it's not possible to exclude package's dependencies. I used bootswatch for bootstrap themes, but there is no way to tell the packages that depend on bootstrap, to not download it, since I'm already using bootswatch. 

At this point, I think there's no shame in just copying frontend libraries manually. Dependency hells are a nightmare, and I've had my fare share of dealing with package managers in their early days, from rpm to maven. I find it's best to avoid them until version 2.0, at least.

## Use Q

I used mongoose for talking to MongoDB on the server, and mongoose uses the old school callbacks. While this is a trivial app, it was enough to give me a hint how it would feel like to get lost in a forrest of callbacks. I would definitely use Q in the future. There is a mongoose library for this: https://github.com/iolo/mongoose-q

## Good patterns are good

And finally, a message from Captain Obvius: use good patterns. Javascript gives you a lot of freedom, which also means freedom to hurt yourself - badly. Fight those project managers to leave you alone for a second, so you can study and find the best way to do something, because the wrong way can be really wrong. The node ecosystem is still young, there's a lot of dark alleys to get lost in, but if you take some time, none so bad to make you want to run back to J2EE. 