---
layout: post
title:  "Protect your Play! application with the CSRF filter"
date:   2013-02-11 19:21:00
comments: true
---

Play! 2.1 introduces a new filters API and [Cross Site Request Forgery (CSRF) protection](http://www.playframework.com/documentation/2.1.0/Highlights).  The CSRF filter is not well documented and it took a while to work out how to enable it in my Play! application.  It took a bit of digging through the Play! source code and looking through the [CSRF tests](https://github.com/playframework/Play20/tree/master/framework/test/csrftest-scala) to work out how to use the CSRF filter.  Below is a summary of what is required to protect your Play! 2.1 application with the new CSRF filter.

First of all add the filters module to your app dependencies in appname/project/Build.scala.

{% highlight scala %}
import sbt._
import play.Project._

object ApplicationBuild extends Build {

  val appName         = "appname"
  val appVersion      = "1.0-SNAPSHOT"

  val appDependencies = Seq(
    // Add your project dependencies here,
    jdbc,
    anorm,
    <strong>filters</strong>
  )
}
{% endhighlight %}

Next create appname/app/Global.scala and extend the Global settings so that it uses the CSRF filter.

{% highlight scala %}
import play.api.mvc._
import play.api._
import play.filters.csrf._

object Global extends WithFilters(CSRFFilter()) with GlobalSettings
{% endhighlight %}

Now the CSRF tests will convey that you need to do the following in all your controllers that return a view with a form post.

{% highlight scala %}
def index = Action { implicit request =>
  import play.filters.csrf._
  Ok(views.html.index(CSRF.getToken(request).map(_.value).getOrElse("")))
}
{% endhighlight %}

and use the CSRF token in your view template as follows.

{% highlight scala %}
@(csrf: String)

@main("Welcome to Play 2.0") {</pre>
<style type="text/css" media="screen"><!--
      label{display: block}

--></style>
<div>
<h2>With token</h2>
<form accept-charset="utf-8" action="@routes.Application.save()?csrfToken=@csrf" method="post">
<label for="name">Name:</label><input id="name" type="text" name="name" value="" />

<label for="age">Age:</label><input id="age" type="text" name="age" value="" />

<input type="submit" value="Continue →" /></form></div>
<pre>
}
{% endhighlight %}

Relax you don’t have to call CSRF.getToken() in all of your controllers that present a form.  Fortunately there is a CSRF view helper that takes an implicit token so that you won’t need to set up the token in your controller and pass it to the view.  Instead you just have to declare the implicit token in the view and wrap your post action with the CSRF helper function as follows.

{% highlight scala %}
@(signInForm: Form[(String,String)])(implicit token: play.filters.csrf.CSRF.Token)
@import helper._

@implicitFieldConstructor() = @{
    FieldConstructor(bootstrapInput.render)
}

@main(Messages("shopanoptic")) {
    <div class="offset3 span6">
        <div class="well">
            <h3>@Messages("signin")</h3>

            @form(CSRF(routes.Session.authenticate()), 'class -> "form-vertical") {

                @signInForm.globalError.map { error =>
                    <p class="error">
                        <span class="label important">@error.message</span>
                    </p>
                }

                <fieldset>
                    @inputText(
                        signInForm("email"),
                        'placeholder -> Messages("email"),
                        '_label -> null,
                        '_help -> Messages("signin.your.email")
                    )
                    @inputPassword(
                        signInForm("password"),
                        '_label -> null,
                        'placeholder -> Messages("password"),
                        '_help -> Messages("signin.your.password")
                    )
                </fieldset>

                <div class="form-actions">
                    <input type="submit" class="btn btn-primary" value="@Messages("signin")">
                </div>
            }
        </div>
    </div>
}
{% endhighlight %}

Now you can protect your web application from one of the [OWASP Top 10 security vulnerabilities](https://www.owasp.org/index.php/Top_10_2010-A5).
