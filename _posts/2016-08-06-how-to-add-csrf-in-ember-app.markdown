---
layout: post
title:  "How to add csrf in Ember app"
author: "Rajat Singla"
date:   2016-08-06 21:41:58 +0550
categories: ruby,csrf
comments: true
---

## What if i'm using rails as api, so my frontend is seperated from rails can i still prevent csrf and how?

So lets take an example where we use ember for frontend and rails as api.
In this case apart from adding protect_from_forgery in application controller you have to do additional three things.
One you have to add following code in application controller:
<!--more-->
{% highlight ruby %}
after_action :set_csrf_token
  protected
  def set_csrf_token
    if request.xhr?
      # Add the newly created csrf token to the page headers
      # These values are sent on 1 request only
      response.headers['X-CSRF-Token'] = "#{form_authenticity_token}"
    end
  end
{% endhighlight %}

Here we are sending newly generated form_authenticity_token in response of xhr request(ajax- XMLHttpRequest)
"request.xhr?" is to ensure that it is ajax request, we will not want to send token to any other request right.
<br>Two you have to set this new token in meta tag by extracting it from response.
<br>Third you will send csrf token in request header by extracting it from meta tag.
you can achieve two and three by adding following code in app.js of ember
{% highlight ruby %}
$ ->
  $.ajaxPrefilter (options, originalOptions, xhr) ->
    token = $('meta[name="csrf-token"]').attr('content')
    xhr.setRequestHeader('X-CSRF-Token', token)

  $(document).ajaxComplete (event, xhr, settings) ->
    csrf_token = xhr.getResponseHeader('X-CSRF-Token')
    if csrf_token
      $('meta[name="csrf-token"]').attr 'content', csrf_token
{% endhighlight %}

<br>
<b>
Why change csrf token in header on every post request and not only when session changes?
</b>
If it were to change token only when session changes there would be an extra overhead to trigger session change and then generate csrf token and set in header.
It is a lot easier to randomally generate csrf token using session[:_csrf_token] on every post request.
