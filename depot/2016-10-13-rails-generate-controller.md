---
layout: post-show
keywords--0: 
title--before: rails generate controller
keywords--1:
title--middle: 
keywords--2:
title--after: 
---

{% highlight shell %}
rails generate controller StaticPages home help 
{% endhighlight %}

In the config/routes.rb file:

`get 'static_pages/home'`

`get 'static_pages/help'`