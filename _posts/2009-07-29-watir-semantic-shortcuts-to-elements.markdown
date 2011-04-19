--- 
layout: post
title: Watir Semantic Shortcuts to Elements
date: 2009-07-29 09:28:29.828000 -05:00
---

## Accessing Watir elements with semantic shortcuts

Let's say you have a text_field element named "last_name" you want to access on a page and the text_field is inside a div container with id 'person'. You may write something like this in Watir API:

{% highlight ruby %}
browser.div(:id, 'person').text_field(:name, 'last_name')
# or something like this if you don't care about text_field contained by the div
browser.text_field(:name, 'last_name')
# but for our purpose we'll stick with the div containing text_field scenario
{% endhighlight %}

And what if you want to decouple the GUI specific implementation and replace it with a shortcut that refers to that text_field element? After all when user is visiting a page she does not fill the text_field with a :name, 'last_name' or 'lst_nme' or 'lnm' . She enters her 'last name' on the 'page'. So you can call that page object `last_name` (in Ruby speak). Then you would need to provide some kind of translation between the page object name and the DOM name. But why even bother with that? There are at least 2 payoffs; 1) you write tests that in a language users speak which brings clarity and intention revealing behavior and 2) over time maintanance is minimized; contained to the methods that define page elements and not to our tests.

## How to build the accessors to dom elements

The simplest way to decouple GUI from your tests is to wrap the code that accesses the DOM element with a friendly domain specific name.

{% highlight ruby %}
def last_name
  browser.div(:id, 'person').text_field(:name, 'last_name')
end
# and call it with last_name method
last_name.set "Johnson"
{% endhighlight %}

But if you have 10 or 20 or 100 of those elements there has to be a better way. You can define a dynamic method that creates the wrapper methods for you. Maybe like this.

{% highlight ruby %}
field :last_name do
  browser.div(:id, 'person').text_field(:name, 'last_name')
end
{% endhighlight %}

The `field` method defines `last_name` method or any other method for you. (on that later). As you can see there is not much difference between the def wrapper method and the dynamic generator. However in a dynamic generator you can set the receiver of the block, maybe access logging and provide call back methods or some other special code. The dynamic generator is your factory of shortcuts to elements. ( I realize that I just took a long jump here)

For example one of the things you can take care of in dynamic method creation is to provide the `browser` as the receiver of the block, then you shorten your definition like this:

{% highlight ruby %}
# browser as receiver will be provided for you
# notice the browser method is not referenced here
field(:last_name) { text_field(:name, 'last_name') }
{% endhighlight %}

This dynamic method generation mechanism is used by many Watir libraries
(see [Taza](http://github.com/scudco/taza) `element` and [Watircraft](http://github.com/bret/watircraft) `field` and [Watirloo](http://github.com/marekj/watirloo) `face` methods, or [Rasta](http://github.com/hmcgowan/rasta) `keyword`.
It allows you to create a semantic page object name, keyword, simply a friendly name to access an element on a page. The semantic names conform to the way a user would talk about objects on the page so their names are customer facing and are used to describe tests. The method definitions are developer facing and they wire the semantic name to an element in the DOM. This is a kind of a facade or mapping mechanism. The benefits are test clarity and minimized maintenance.

If you still wonder how this may work here is a scenario example: imagine you have many tests that use the call to `last_name` but at some point the text_field definition changes, maybe a :name attribute is changed or it's not a text_field any more, maybe a select_list of last names. Now you don't have to go and find all the instances of a `text_field(:name, "last_name")`. All you have to do is update the definition stored by the facade method rather than in all the places the text_field is used as implementation. In this case the `last_name` method is used as a 'stand in' for that text_field or select_list or whatever element. The `last_name` is a facade method, a face to interact with.

### Page Object as a shortcut to a collection of elements.

What if you have two text_fields on a page like this.

{% highlight ruby %}

text_field(:name, 'last_name0')
text_field(:name, 'last_name1')

{% endhighlight %}

Now the page is asking to fill the last name for two people. Are you going to have to define the `last_name0` and `last_name1` field method? No, we can treat the text_fields sharing similar naming pattern as a collection.

We can maybe do something like this

{% highlight ruby %}
field :last_name do |nbr|
  text_field(:name, "last_name#{nbr}" #pass the counter to alter name attribute
end

# and then you can access 
last_name(1).set "Johnson"
last_name(2).set "Kowalski"
last_name(42).set "Adams"

{% endhighlight %}

This is an example of how an `field' type method can be made to accept attributes that then alter the pattern of access in a collection.

### Watirloo with face method for making semantic page objects

In [Watirloo](http://github.com/marekj/watirloo) I call the method that creates semantic page objects the `face` method because it helps to make an `interface` to some element on the page and `face` is a sort of a facade, and invokes some semantic personalization. It's fun to write tests where you define `faces` you talk to in your tests.

Here are some examples of how you create faces in watirloo that map to some cryptic DOM definitions for address form

{% highlight ruby %}
face(:zip) { text_field(:name, 'zipCd')}
face(:street) { text_field(:id, 'adrStr1'}
face(:state) { select_list(:name, 'stCd'}

# now you can call them in your test
zip.set "75230"
street.set "Walnut Street"
state.set "TX"

{% endhighlight %}

What I haven't mentioned is how the face method defines a receiver of the block given by a facename. When you call `zip` or `street` method who receives the call? Well the current 'self' receives the call, it can be rspec group or TestCase class or any other object that contains the face definition. But who receives the call of the body of the method? Here is an example where `browser` is the receiver of the block

{% highlight ruby %}
# defines access to element based on its facename

def face(facename, *args, &block)
  define_method(facename) do |*args|
    browser.instance_exec(*args, &block)
  end
end

{% endhighlight %}

The main part is `browser.instance_exec` It's just like instance_eval but accepts arguments passed to it (as seen in the `last_name(1)` example).

In the above face definition I assume that all interfaces talk to the browser directly as the page container. `browser` is the receiver of the blocks defined by `face` method and here implicitly provided by the face method. If it wasn't then we would need to provide it somehow in the definition.

Here are two different examples: 

{% highlight ruby %}

# browser as receiver assumed implicitly and provided by face method
face(:person) { div(:id, 'person') }

# browser provided explicitly. The face method assumes the browser method exists in the receiver
face(:person) { browser.div(:id, 'person') }

{% endhighlight %}

Which is better? Not sure, I think I'll go with the first choice for Watirloo face definition treating the browser (or the root document if frames are present) as implicit receiver for facename blocks.

## Shifting receiver with chaining.

But what if you want to specify that the text_field is inside a form or a div and you want to provide a specific access to that particular location. Maybe you would like to write you tests with that containment in mind:

{% highlight ruby %}
# instead of 
browser.div(:id, 'person').text_field(:name, 'last_name0')

# you wish you could write like this shortcut
browser.person.last_name(0)

# and you imagine these definitions that would support it
face(:person) {div(:id, 'person')}
face(:last) { |nbr| text_field(:name, "last_name#{nbr}")}

# but that will NOT WORK out of the box
{% endhighlight %}

Will not work! No! No! No! Well, maybe... but it requires some manipulation of 'self' of the returning objects as you chain them.

The Watir API is beautiful in this regard. The 'div' method returns the reference to DOM where the 'div' is located and you can call the text_field on it so the locator will find that element contained in the 'div'.

The secret in Watir code is something like the factory methods used.

{% highlight ruby %}

def text_field(how, what)
  TextField.new(self, how, what)
end

{% endhighlight %}

This really is the genius of Watir API design IMHO because text_field method instantiates TextField class and passes it the context of a container. Then the TextField calls set_container to narrow the scope. Very nice indeed.

The problem with page objects chaining is that we need to manage the scope of containment. Simply doing the chaining because it looks nice and convenient `browser.person.last_name(0)` will not work unless the call to 'person' method returns the receiver that also has `last_name()` method defined. If you are lost at this point don't worry and read on.

In Watir API `browser` returns an object that responds to methods like 'div' or 'text_field' and in turn the 'div' method returns an object that responds to 'div' or 'text_field' methods as well. With each method you 'chain' you narrow the scope of the DOM tree where a particular element is contained. Watir element methods take care of establishing the receiver implicitly. That's what makes Watir API so nice. Pleasing to use and hard working behind the scenes.

> I may be exaggerating a bit but this is why I think Watir is underappreciated in Ruby community because it mainly targets Windows and Internet Explorer  but that is precisely the reason why Watir is still 'undiscovered' in the Enterprise browser testing where high cost commercial solutions win because of marketing.

Back to our topic: In building some kind of an abstraction layer that decouples implementation from page objects, known sometimes as GUI mapping, we would need to manage that scope in a `face` definition.

## Which Solution to Implement?

Solution (1) is to define faces with known receiver as `page` the root element (either a browser or a frame) and forget the method chaining. Any facename like `last_name` method would be made to call on `page` but `person.last_name` chaining would not work because `person` returns Watir 'div' object which does not :respond_to? `last_name`. So if you want fined grained control just drop to the Watir API and deal with DOM implementation that way. This is my preferred method (do not obscure Watir API, work with it, do not hide it).

Solution (2) is to allow for method chaining that always has to start with browser or frame, a `page` method that returns the root document to talk to. In this case we can have `page.person.last_name` or `page.last_name` where `page` is the receiver of defined page objects. Then if you want to drop to Watir API you can use `browser.div(:id, 'person')` instead of `page.person` (this has some potential)

Here are some code acrobatics to accomplish this.

{% highlight ruby %}

def self.face(facename, *args, &block)
  define_method(facename) do |*args|
    @container = @container.instance_exec(*args, &block)
    self #return receiver to chain to next page_object
  end
end

def page
  @container = browser
end

# now having this you can define
face(:last_name) {|nbr| text_field(:name,"last_name#{nbr}")}
face(:first_name) {|nbr| text_field(:name,"first_name#{nbr}")}
face(:person) { div(:id, "personPanel") }

# and use in your client
page.person.first_name(2).set "Kurt"
page.person.last_name(2).set "Vonnegut"

# or

page.first_name(2).set "Kurt"
page.last_name(2).set "Vonnegut"

{% endhighlight %}

Now you can access `last_name()` explicitly contained by `person` or just contained by `page` which is equivalent to `browser` unless you want to redefine it to point to a frame.

The new issue with this is that every time you call a `facename` method it returns 'self' in order for chaining to work. So if you want to call .set "value" method you either have to provide an adapter or handle it with method_missing like this;

{% highlight ruby %}
# adapter to call Watir Element set.
def set
  @container.set
end

def method_missing(method, *args)
  if @container.respond_to?(method)
    @container.send method, *args
  else
    raise "Something went wrong"
  end
end

{% endhighlight %}

Another solution (3) would be to have a factory method that returns Face.new(container, facename, *args, &block)) and encapuslate this setting of container in the similary way Watir works. (or maybe just add Face class subclassed from Watir::Element to get that management for free?)

Either solution depends on your page objects need. I can see that chaining solution can give me more flexibility in situation where there is a text_field with the same name in two or more different forms but it adds some mental acrobatics to build this. 

On the other hand the `browser.instance_exec` makes sure every element is anchored to the browser as receiver and stick with that. Well, we'll see. If you have any ideas let me know. For now Watirloo defines the Solution 1 in gem 0.0.6. In gem 0.0.5 you had to provide the receiver yourself in face definitions.

Any feedback on which solution makes sense to you? Thanks.
