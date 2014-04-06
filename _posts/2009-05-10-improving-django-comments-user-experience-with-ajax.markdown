---
layout: post
title:  "Welcome to Jekyll!"
date:   2009-05-10 20:20:00
---

I really like Django. It is not bloated like a lot of other frameworks, and it has a healthy balance between convention and configuration. As a developer I want to be able to use the tools that I want to use, and not be forced into a specific way of doing things. An example of this in Django is the comments framework that is part of django.contrib. The comments framework provides the infrastructure for attaching comments to any domain model in your Django project through content types. It also provides a few spam prevention features that you should consider leveraging, such as a security hash or a hidden honeypot field. However, the default comments form is rather bare, and the workflow for posting a comment requires too many clicks and redirects.

There are some solutions that try to improve the user experience, and do a good job of it, but require hacking the comments framework. I personally don’t like hacking the internals of any framework. Not because I’m scared to, but because I don’t want to inherit any unnecessary maintenance overheads.

Besides, I believe the comments framework does the right thing. It doesn’t try to do too much, and by that I mean the HTML that it generates for the default rendered form and corresponding responses can easily be mashed up as part of the submitting pages DOM. So with a bit of JQuery and AJAX knowhow you can post comments without refreshing or being redirected away from the submitting page.

First I use the utility methods from the comments framework to list all comments for a particular discussion content type, and to display the default comment form immediately after. Note that the comment form is wrapped in div tags with the id “comment_form”. The div will be used to override the block with responses from the server after an ajax post.

{% highlight html %}
<h3>Comment</h3>
    {% load comments %}
    {% get_comment_form for discussion as form %}
    <div id="comment_form">
    {% render_comment_form for discussion %}
</div>
{% endhighlight %}

In my template (discussion.html) in which I want to allow people to comment on something I add the following to a script block that will insert the JavaScript into the head element of the base template (base.html).

{% highlight js %}
<script type="text/javascript" charset="utf-8">
function bindPostCommentHandler() {
    $('#comment_form form input.submit-preview').remove();
    $('#comment_form form').submit(function() {
        $.ajax({
            type: "POST",
            data: $('#comment_form form').serialize(),
            url: "{% comment_form_target %}",
            cache: false,
            dataType: "html",
            success: function(html, textStatus) {
                $('#comment_form form').replaceWith(html);
                bindPostCommentHandler();
            },
            error: function (XMLHttpRequest, textStatus, errorThrown) {
                $('#comment_form form').replaceWith('Your comment was unable to be posted at this time.  We apologise for the inconvenience.');
            }
        });
        return false;
    });
}

$(document).ready(function() {
    bindPostCommentHandler();
});
</script>
{% endhighlight %}

In line 2 I define a method called bindPostCommentHandler() which performs the ajax call to post a comment and to handle the response when the form submit event is triggered.

In line 3 I remove the preview button from the form as I do not want this functionality. Rich text plugins generally have a preview mode, so I don’t see the need for the server to process a preview request.

In line 4 I bind the ajax post call to the submit event of the comments form. This means when I click on the Post button to submit the form I will trigger an ajax post instead of a normal post to the server.

Line 7 serialises the data from the form input fields using the JQuery serialize() function.

Line 8 specifies the URL to post to. I use the utility method from the comments framework to retrieve this for me, so I don’t need to hard code it to a specific URL. The comment_form_target should retrieve the correct URL for the comments application that you should have configured in your urls.py file. If not then add the following URL route to your urls.py file.

    (r'^comments/', include('django.contrib.comments.urls')),
Line 10 specifies that the response will be HTML. This is necessary so that JQuery knows how to parse and handle the response.

Line 11 details the success callback, which is the function that calls when the response is successful. The callback simply replaces the form in the div wrapper with the response. A successful response could either be a “thank you” message for posting a comment, or a form displaying fields with invalid fields. Line 13 is necessary to rebind the bindPostCommentHandler() function to any new form that appears in the div wrapper. If you omit this rebinding, then you will lose ajax posts on successive submits.

Line 15 defines the callback that gets called when the server responds with an http error code. I just override the div wrapper block with an apologetic message.

Finally, line 23 binds the bindPostCommentHandler() function when the page is ready.

As you can see from the above, all you need is some JavaScript to improve the user experience for posting comments using Django’s comments framework. No framework hacking necessary. Note that if you want to further improve this solution, then you can add sliders, fade ins, and fade outs for displaying the responses from the server. I left these out for brevity. I also left out appending the submitted comment to the list of comments, as the above solution will only display the submitted comment after the user performs a GET request after the submit.

