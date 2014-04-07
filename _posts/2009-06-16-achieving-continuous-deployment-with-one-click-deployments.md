---
layout:     post
title:      "Achieving continuous deployment with one click deployments"
date:       2009-06-16 14:52:00
comments:   true
---

It makes me very happy to see others [championing continuous deployment](http://startuplessonslearned.blogspot.com/2009/06/why-continuous-deployment.html). Apparently the developers at [IMVU](http://www.imvu.com/) release into production a number of times a day, as often as 50 releases in a day. Smells like a lot of bug fixing than new features, but at the very least they have a lean process for releasing into production. I believe having the capability to release into production as often as possible is more important than having a process that draws out the length of time that it takes to release into production.

I’ve always been a fan of getting code into production as quickly as possible. Releasing into production early and often adds business value and keeps users happy. Adding new functionality that makes it easier to add items to a shopping cart for example means the business can encourage more fluid sales through a better online experience. It also means certain bugs won’t provide a detrimental user experience that would drive customers away. You will drive more customers away the longer a bug stays in production. Or if you are lucky, have one of your users get so ticked off with your application that they [provide their own patch and release it for you](http://forum.skype.com/index.php?s=1e6bd318faff0a16bc9bc1987efc79d3&showtopic=310121&st=20&p=1633781&#entry1633781).

My preferred approach to continuous deployment is to integrate it as part of your continuous integration (CI) build box. With Cruise or Bamboo create a build that runs an ant script that creates a branch, compiles source, runs tests, and deploys on a successful build. You might want to keep this build separate from your regular trunk builds, as you may not want to release every time someone checks code into the source code repository, especially when your team gets into a healthy habit of doing atomic commits.

Utilising your CI box in your deployment process effectively means you can release into production with one click of a button. Imagine that, one click deployment, one click that automates your quality assurance process (assuming you have a quality suite of unit, integration, and functional tests), and production artifacts such as WAR files and configuration properties for archiving. Spending time improving your deployment process and making it leaner means you can realise business value earlier and keep your customers happier.

