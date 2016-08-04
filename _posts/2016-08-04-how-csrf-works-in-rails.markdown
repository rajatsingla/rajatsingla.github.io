---
layout: post
title:  "What is csrf in rails and how it works."
date:   2016-08-04 21:41:55 +0530
categories: ruby
---


In the descriptions that follow, the parameter sym refers to a symbol, which is either a quoted string or a Symbol (such as :name).

{% highlight ruby %}
Module Mod
  include Math
  CONST = 1
  def meth
    #  ...
  end
end
Mod.class              #=> Module
Mod.constants          #=> [:CONST, :PI, :E]
Mod.instance_methods   #=> [:meth]
{% endhighlight %}
constants → array
constants(inherited) → array
In the first form, returns an array of the names of all constants accessible from the point of call. This list includes the names of all modules and classes defined in the global scope.

source:<a> http://ruby-doc.org/core-2.2.0/Module.html</a>
