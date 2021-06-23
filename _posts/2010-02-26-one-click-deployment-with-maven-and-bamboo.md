---
layout:     post
title:      "One click deployment with Maven and Bamboo"
date:       2010-02-26 13:00:00
---

A while back I wrote about [achieving continuous deployment with one-click deployments]({{ site.url }}/2009/06/16/achieving-continuous-deployment-with-one-click-deployments/). I didn’t provide an example for that post as I mostly wrote about why you need to achieve continuous deployment. Here I will follow up with a simple example of how you can achieve continuous deployment.

Continuous deployment is quite easy to setup if you are using a typical Maven project structure and Bamboo as your continuous integration tool. Also I am assuming that you want to deploy your application to a tomcat server.

In your pom.xml file add the following configuration so that you use the Tomcat plugin for deploying your application to http://hostname.com/app. Change the path, url, and server configurations to suit your needs.

{% highlight xml %}
<project>
  <build>
    <plugins>
      <plugin>
        <groupId>org.codehaus.mojo</groupId>
        <artifactId>tomcat-maven-plugin</artifactId>
        <configuration>
          <path>/app</path>
          <url>http://hostname.com/manager</url>
          <server>deployment.server</server>
        </configuration>
      </plugin>
    </plugins>
  </reporting>
</project>
{% endhighlight %}

Also make sure that your .m2/settings.xml file contains the following for authenticating with the Tomcat manager.

{% highlight xml %}
<settings xmlns="http://maven.apache.org/SETTINGS/1.0.0"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.0.0

http://maven.apache.org/xsd/settings-1.0.0.xsd">

  <servers>
    <server>
      <id>deployment.server</id>
      <username>tomcat</username>
      <password>password</password>
    </server>
  </servers>
</settings>
{% endhighlight %}

In Bamboo create a new pan for your project. I tend to give this plan the name “Promote to Production”. Configure the Builder goal for Maven to run clean tomcat:redeploy. I also only allow a specific user to trigger this plan so that not everyone has permission to deploy into production. Finally, configure the build strategy to run manually, so an authorised person can click on the Build plan button in Bamboo to deploy the application.

Once set up the above instructions will allow an authorised person in Bamboo to click on a single button to deploy into production. Leveraging your continuous integration tool for deployment allows you to archive deployment artifacts such as your WAR files in the case where you have to revert to a previous version.

