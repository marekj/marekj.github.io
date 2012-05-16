---
title: Seconf WatirDay and PageObjects
layout: post
---

## Presentation at Seconf WatirDay


<div style="width:425px" id="__ss_7502787"> <strong style="display:block;margin:12px 0 4px"><a href="http://www.slideshare.net/testrus/domain-specific-watir-page-objects" title="Domain Specific Watir Page Objects" target="_blank">Domain Specific Watir Page Objects</a></strong> <iframe src="http://www.slideshare.net/slideshow/embed_code/7502787" width="425" height="355" frameborder="0" marginwidth="0" marginheight="0" scrolling="no"></iframe> <div style="padding:5px 0 12px"> View more <a href="http://www.slideshare.net/" target="_blank">presentations</a> from <a href="http://www.slideshare.net/testrus" target="_blank">rubytester (testrus)</a> </div> </div>


## Domain Specific Watir Page Objects 

The talk on Page Objects was mostly about code. How do you build an abstraction layer on top of the regular plain old watir elements. How do you build a semantic layer, the semantic coming from your business domain language that you care about. 

#### Page Elements and Watir objects

Let's start with a sketch of a page with some elements you need to fill: 

![Page with Elements to turn into page objects](/images/watirday-pageobjects-mock.png)

So we have a First Name, Last Name, Gender and Phone number. If this was an html page the corresponding watir code for these elements could be like this:

{% highlight ruby %}
browser.text_field(:name, 'frst_nm')
browser.text_field(:name, 'lst_nm')
browser.text_field(:name, 'sex_cd')
browser.text_field(:name, 'phone_1')
browser.text_field(:name, 'phone_2')
browser.text_field(:name, 'phone_3')
{% endhighlight %}


#### Setting values

And given I have some test case data to enter for those elements: 

	firstname: 'Franz'
	lastname: 'Ferdinand'
	gender: 'M'
	phone: '(800)555-1212'

I will write some watir code for it:

{% highlight ruby %}

browser.text_field(:name, 'frst_nm').set 'Franz'
browser.text_field(:name, 'lst_nm').set 'Ferdinand'
browser.text_field(:name, 'sex_cd').set 'M'
browser.text_field(:name, 'phone_1').set '800'
browser.text_field(:name, 'phone_2').set '555'
browser.text_field(:name, 'phone_3').set '1212'

{% endhighlight %}


#### Connecting data to objects

At this point I want to treat objects on the page like I treat data elements so for example I want to relate the "Last Name" text field on the page as 'lastname' element and I want to hide the actual watir api implementation. 


{% highlight ruby %}

    # I want to use my test vocabulary as identifiers for page elements
    firstname.set = 'Franz'
    lastname.set = 'Ferdinand'

    # so I insert watir objects in a method
    # those are my watir object providers
    def firstname
      browser.text_field(:name, 'frst_nm')
    end

    def lastname
      browser.text_field(:name, 'lst_nm')
    end

{% endhighlight %}

#### Page class mapping data to pages elements

These elements are on the page so we need a page and we need to talk to the browser

{% highlight ruby %}

    # These elements exist on a page

    # A page may look like this
    page = Page.new(browser)

    # and setting value to elments like this
    page.firstname.set 'Franz'
    page.lastname.set 'Ferdinand'
    page.gender.set 'M'


 	# in Order to accomplish this we would need a Page class

    class Page
      attr_accessor :browser

      def initialize browser
        @browser = browser
      end

      def firstname
	    browser.text_field(:name, 'frst_nm')
      end

      def lastname
        browser.text_field(:name, 'lst_nm')
      end
    end

{% endhighlight %}


#### Data element matching with page element

Let's remember that I think of my data like this: 

	firstname: 'Franz'
	lastname: 'Ferdinand'
	gender: 'M'

and in Ruby I can represent it as a hash

{% highlight ruby %}

    datamap = {:firstname => 'Franz',
               :lastname => 'Ferdinand',
               :gender => 'M'}

    # and I can set values to my element like this
    page.firstname.set datamap[:firstname]
    page.lastname.set datamap[:lastname]
    page.gender.set datamap[:gender]

{% endhighlight %}

As you can see each key in hash represents a name of a method on a page that accesses that very page element I need to set but there has to be a better mechanism than enumerating each key to each element. What we want is the page to match the key of my data to the page object I am trying to set.

Let's look at this mechanism.

{% highlight ruby %}

    # commmon page class for all pages
    class Page
      attr_accessor :browser

      def initialize(browser)
        @browser = browser
      end

      def set(hash)
        hash.each_pair do |field, value|
          browser.send(field).set value
        end
      end

    end

    #populate data on a page
    class PersonPage < Page

      def firstname
        browser.text_field(:name, 'frst_nm')
      end

      def lastname
        browser.text_field(:name, 'lst_nm')
      end

      def gender
        browser.select_list(:name, 'sex_cd')
      end
    end


    page = PersonPage.new(browser)
    page.set :firstname => 'Franz', :lastname => 'Ferdinand', :gender => 'M'

{% endhighlight %}


Now the Page class accepts the data hash map and matches the methods for page elements with the data for keys. At this point Watir has been encapsulated and slowly the semantic page object naming is layer is being used to define test cases. 
This has been our goal, how to write test cases using the business domain naming and hide the actual page access implementation. 



