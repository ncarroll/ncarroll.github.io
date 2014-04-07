---
layout:     post
title:      "Using StringTemplate as the view engine for your Spring MVC application"
date:       2009-06-18 18:03:00
comments:   true
---

I wish I never discovered Django templates because I shudder every time I have to use Freemarker or Velocity for my Spring MVC applications. Fortunately there is an alternative. You can use StringTemplate to generate your views in Spring MVC quite easily. StringTemplate enforces a strict separation of concerns, which therefore minimises the amount of logic that is allowed in the view, thus forcing you to do more in your controllers. Therefore your view remains purely for presentation purposes.

First things first is that you need to download StringTemplate and Antlr, and make those libraries available on your classpath. Next, extend Spring’s InternalResourceView.

{% highlight java %}
import org.springframework.web.servlet.view.InternalResourceView;
import org.springframework.core.io.Resource;
import org.antlr.stringtemplate.StringTemplateGroup;
import org.antlr.stringtemplate.StringTemplate;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.util.Map;
import java.io.PrintWriter;

public class StringTemplateView extends InternalResourceView {

    @Override
    protected void renderMergedOutputModel(Map model, HttpServletRequest request,
                HttpServletResponse response) throws Exception {

        Resource templateFile = getApplicationContext().getResource(getUrl());

        StringTemplateGroup group = new StringTemplateGroup("webpages", templateFile.getFile().getParent());
        StringTemplate template = group.getInstanceOf(getBeanName());
        template.setAttributes(model);

        PrintWriter writer = response.getWriter();
        writer.print(template);
        writer.flush();
        writer.close();
    }
}
{% endhighlight %}

Finally, you need to configure your Spring application context to use StringTemplate to render your views.

{% highlight xml %}
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans

http://www.springframework.org/schema/beans/spring-beans-2.5.xsd>

    <bean id="viewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <property name="viewClass" value="com.agilecognition.bookshelf.web.view.StringTemplateView"/>
        <property name="prefix" value="/WEB-INF/templates/"/>
        <property name="suffix" value=".st"/>
        <property name="requestContextAttribute" value="rc"/>
    </bean>
</beans>
{% endhighlight %}

Now you can organise your StringTemplate files that have the “.st” suffix in /WEB-INF/templates. For example, you might want to organise your templates as follows.

WEB-INF
    + templates
        + layout
            layout.st
        + partials
            header.st
            footer.st
            feedback_form.st
            contact_us.st

The layout.st template might look like the following template.

{% highlight html%}
<html>
  <body>
    <div class="header">$partials/header()$</div>
    <div class="content">$body$</div>
    <div class="footer">$partials/footer()$</div>
  </body>
</html>
{% endhighlight %}

The above layout.st contains the basic structure for an HTML page on your site. You can use this one template to keep a consistent look and feel across your entire site. The layout.st template depends on the header.st and footer.st templates that exist in the partials directory.

To insert the feedback form in feedback_form.st into the $body$ placeholder attribute for your contacts page you simply create a contact_us.st template with the following line.

$layout/layout(body=partials/feedback_form())$ 
When your controller handles the request to display the contact us page, the controller will return a ModelAndView with the view name set to “contact_us”. The view name maps to the contact_us.st file in your templates directory. StringTemplate will render the contact_us page using the layout template with the header, footer and feedback form templates.

