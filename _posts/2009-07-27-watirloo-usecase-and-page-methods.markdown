--- 
title:  Watirloo UseCase and Page methods
layout: post
---

## Examples according to a Model, Re Model according to examples

I think this is how I write my tests (Regression tests on Legacy Web apps). I first talk to the users and try to get some 'concrete' examples of usage. In the examples of usage I listen for 'Scenario' language and for 'Domain' language. From that I can start constructing some TestSkeleton, a type of Model of Usage, I usually call this UseCase. I can call it TestScenario, or UsageScenario, Workflow or whatever. The point is that that model is a Test Object and I treat it as 'Receiver' of methods, actions taken that TestScenario,

In Cucumber Features the receiver is the Instance of World and each Given, Then And can access the helper methods included in World. In Rspec the Receiver is ExampleGroup object and Examples have access to the methods.

For a while I have been using Page class as a receiver of actions on a Page but with Watirloo 0.0.5 I just chucked it. I now use Page module which I will use to happily 'pollute' the object space of my receiver whichever it happens to be. This may not be a good move but we'll see in time. So in Rspec I just include Watirloo::Page to 'pollute' the space with helper methods and as long as they are intention revealing and don't clash with some other methods I can use them as ExampleGroup methods in my example implementation.

Why this move? For months I noticed that a lot of my infrastructure code was managing the Page adapters abstractions on top of a browser and when I wanted to drop down to just the browser it was strange low level stuff. But I think I was doing myself a disservice hiding Watir library in my Page classes. Watir is a great library and I should take advantage of it and not obscure it. So I am getting rid of Page class because of that and I want to rely on Watir Helper methods that ease and speed up my testing. It is more natural that way for me.

Back to model. I treated Page class as Object that models Webpage or parts of a webpage. It was useful becuase I could treat it as a container of functionality for a particular Feature in software. It was also not useful because when I needed some behaviour in another place I had to jump from object to object to get it. 

Overall as I work more with Ruby I am getting more comfortable with the receiver (the 'self') in code so that in test scenarios I don't jump between objects but jump between 'contexts' of self. The behavior I am test modeling occurs in 'context' and not in object so I should model behaviour in code. 

Example code.

{% highlight ruby %}

# scenario code
UseCase.run do |uc|
  uc.data = {:firstname => "Kurt", :lastname => 'Vonnegut'}
  uc.populate_page
end

# example 1
class UseCase
  include Watirloo::Page
  def populate_page
    populate @data
  end
end

# example 2
class UseCase
  def populate_page
    @page.populate @data
  end
end

{% endhighlight %}

In the second example my overhead is my @page object. I have to do extra work to reference it. I think that by pollutiong the UseCase receiver with 'view' methods I can have more flexibility with examples of usage without being constrainted by modeling a Page. I can just use modules as namespaces here to make sure there are no collisions by mixin method.

The similar can be done with rspec

{% highlight ruby %}
describe "entering data on a page" do
  include Watirloo::Page
  it "enters first and last name"
    populate :firstname => "Kurt", :lastname => 'Vonnegut'
  end
end

#versus

describe "entering data on a page" do
  @page = PageWithLastAndFirstName.new
  it "enters first and last name"
    @page.populate :firstname => "Kurt", :lastname => 'Vonnegut'
  end
end

{% endhighlight %}

Well, both are fine but mixin is faster and cleaner way of writing.
I don't have to reference a page and concentrate on behavior in browser no matter what page is loaded.

This helps me get rid of an abstract adapter layer and go from UseCase model of thinking to the example of model in Rspec. we'll see where this goes.
