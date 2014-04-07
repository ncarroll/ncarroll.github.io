---
layout:     post
title:      "Gradle Android Plugin"
date:       2010-04-26 17:30:00
comments:   true
---

I have recently joined a newly formed team developing Android applications at a large telco, and I am pleased to announce that we are using Gradle for our builds. We are using Gradle with the [Android plugin](http://github.com/jvoegele/gradle-android-plugin), and instantly we managed to build a simple application, run tests, and have it installed on a device. Our build script simply looks like the following, which is all that is necessary to use the Android plugin.

{% highlight groovy %}
buildscript {
  repositories {
    mavenRepo(urls: 'http://jvoegele.com/maven2/')
  }
  dependencies {
    classpath 'com.jvoegele.gradle.plugins:android-plugin:0.8'
  }
}
usePlugin com.jvoegele.gradle.plugins.android.AndroidPlugin
{% endhighlight %}

Of course this is a rather simplistic script, but it does everything I need it to do right out of the box. The Android plugin provides a number of tasks that allow you to build, test, package and sign your application. You can even install the packaged application on a device or emulator by running gradle androidInstall. Make sure to set the property “adb.device.arg” to “-e” for a running emulator or “-d” for a connected device.

There is also support in Hudson to trigger a Gradle script. Hudson has a Gradle plugin that can be installed from the Admin console, and allows you to directly trigger a Gradle script in your project. Otherwise you can create a simple shell script to call the Gradle tool from the command line.

It is also worth noting that both IntelliJ and Eclipse provide support for Gradle and the Groovy syntax. That is if you don’t like using the command line to trigger your builds.

Gradle has allowed us to spend less time setting up our build and continuous integration environment, and more time on actual Android development. Our team has benefited greatly from this boost in productivity.
