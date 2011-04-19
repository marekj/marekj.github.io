--- 
title: Using Local Watir Code from Git With Local Gem
layout: post
---

## Turning a path/to/some/dir into a gem with 'local_gem' gem

"The other day while actively developing a gem, I got tired of rake reinstalling it to test its effect in irb with some other gems. I wanted to use the edge version of my gem, version now." - this is how
[Tagaholic](http://tagaholic.me/2009/02/05/local-gem-loads-your-current-code-now.html)
aka [@cldwalker](http://twitter.com/cldwalker) introduces his `local_gem`

I usually work with some stuff I have checked out from git like Watir or Watirloo but when I change some code and I want to use it in my client code I would normally need to create a gem and install it. (what I mean by client code is some code that uses the library and references it as a gem)

This takes time. It's faster to treat the source code for a gem as a gem itsefl without modifing the client code that uses it. This is what 'local_gem' gem provides. Here is a short writeup on how I use it with watir family libraries when I work on my client code and need to make some custom changes to the 'watir' code. 

## Working with 'watir' family gems.

I work with with `watir` gem source mostly with the IE version and modify it with some custom extensions. I also work with 'watirloo' gem for my development. These libs are my infrastructure for my client's test code base. 

Here is how my client code uses the gems (I hope this is similar to how you use it)

{% highlight ruby %}
require 'watir'
require 'watir/ie' # for some IE specific code
{% endhighlight %}

Pretty straighforward. This begs a question: What do you actually require? The 'watir' project source code creates 3 separate gems 'commonwatir', 'watir' and 'firewatir'. The 'commonwatir' gem orchestrates which other gem lib to load depending on your browser choice. So require 'watir' actually requires `GEMPATH/commonwatir/lib/watir.rb` file of 'commonwatir' gem - and when you say you want to work with the browser='ie' then commonwatir requires `GEMPATH/watir/lib/watir/ie.rb` file of the 'watir' gem. 

To illustrate this [I have made a crude drawing of require 'watir' and 'watir/ie' on flickr](http://www.flickr.com/photos/marekj/3615299778)
so if you want to make changes to some 'watir' gem or 'firewatir' gem you can go to the GEMPATH place and mess with it there which is not recommended or you can work on the source code from git but then you would need to create gem and install it again to test in our client code the changes you may want or..... you can use 'local_gem' gem


## Working with 'watir' code locally using 'local_gem'

I use 'local_gem' in its override mode to bypass the cycle of "change code>make gem>install gem>try your client code". I shorten it to "change code>try your client code" like this

{% highlight ruby %}

require 'local_gem'
require 'local_gem/override'

LocalGem.setup_config(nil) do |conf|
  conf.gems = {
    'watir' => [
      "C:/code/github/watir/commonwatir/lib",
      "C:/code/github/watir/watir/lib"
    ]
  }
end

require 'watir' # loads the C:/code/github/watir/commonwatir/lib/watir.rb
require 'watir/ie' # loads C:/code/github/watir/watir/lib/watir/ie.rb 
{% endhighlight %}

Mystery solved. Now in my client code I don't have to make any changes to how I require 'watir' and I can hack on the git repo of watir files checked out from github. (Try it yourself. Checkout 'watir' and make some changes and see them reflected immediately in your client code that uses 'watir' code)

As a helper now we can invent an env var switch `ENV['LOCAL_GEM']` to turn on or off the overrides so I can run one test using the officially installed 'gem' or my 'local_gem' version.

For good measure I will throw in my watirloo gem there too and some other stuff.

Here is how my setup looks like

{% highlight ruby %}
if ENV['LOCAL_GEM']
  require 'local_gem'
  require 'local_gem/override'

  LocalGem.setup_config(nil) do |conf|
    conf.gems = {
      'watir' => [
        "C:/code/github/watir/commonwatir/lib",
        "C:/code/github/watir/watir/lib"
      ],
      'firewatir' => 'c:/code/github/watir/firewatir/lib',
      'watirloo' => "C:/code/github/watirloo/lib",
      'watircraft' => [
        "C:/code/github/watircraft/lib",
        "C:/code/github/watircraft/bin"
      ]
    }
  end
end

# and in my client code I need to provide the env var
ENV['LOCAL_GEM'] = 'override'

{% endhighlight %}


That's about it. Hope you find it useful and thanks to [@cldwalker](http://twitter.com/cldwalker) for hacking the 'local_gem'

