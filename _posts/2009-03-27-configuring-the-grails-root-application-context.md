---
layout: post
title:  "Configuring the Grails Root Application Context"
date:   2009-03-27 23:24:00
---

By default if you create a Grails application called funkysite (i.e. you did grails create-app funkysite on the command line), then when you run your application the root context is set to /funkysite. This means that your controllers such as a UserController with a show action will be available at http://localhost:8080/funkysite/user/show. Having “funkysite” as the root context is redundant, especially if you want to host your application at http://funkysite.com/funkysite/user/show.

My preference is to just have “/” as the root application context. Fortunately, you can easily configure the root application context in Grails. All you need to do is edit conf/Config.groovy and add grails.app.context = “/”. This will set your root application context to “/”, so that the above UserController and show action will be available at http://localhost:8080/user/show.
