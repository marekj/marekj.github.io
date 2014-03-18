---
layout: post
title: Domain Specific PageObject with domkey gem
---

## PageObject is not a Browser Page

It could be an entire Page but to me PageObject is a Domain Specific Object on a Page; or on some visible panel on a Page.

PageObject is an encapsulation of a behavior required of the widgetry of HTML elements composed together and presented to the user.

From the outside PageObject faces the user of the application. User sets the value and queries for the value of a PageObject.

## PageObject should not leak webdriver elements

On the inside PageObject could be composed of one or more webdriver elements required to implement behavior user requires. PageObject encapsulates the webdriver elements widgetry required to talk to the browser and elements should not leak to the outside world by default. Only when needed, a test developer sends an `.element` message and exposes the webdriver element or elements that compose the PageObject.

## Domain Specific PageObject Example

Here is an animated gif example of CheckboxTextField type of PageObject which provides a domain specifc behavior for the user. ([the example is using Domkey gem](http://github.com/rubytester/domkey)). When Checkbox is checked off the corresponding TextArea is cleared and control becomes disabled. When Checkbox is set on, the TextArea accepts text input. I named the checkbox a switch: and TextArea a blurb: Notice that switch: and blurb: are themselves PageObjects that compose the CheckboxTextField PageObject. Webdriver elements are encapsulated.

![Animated Gif of git log aliases in action](/images/rubytester-domkey-pageobject-intro.gif)
(the gif is a bit heavy 1.2 meg)

This PageObject has 3 value states: `true, false, 'Some String'`. When switch: is off the value is false. When switch: is checked but there is no text entered the blurb: then PageObject's value is true, When Text is presentd (with switch: on) then the value is the value of blurb: PageObject. All those 3 value states are Domain Specific in the world of the user of this PageObject.

## PageObject is composed of PageObjects

PageObject does not 'leak' to the outside world what wigetry is receving messages inside. Both switch: and blurb: are PageObjects that responds to `set` and `value`.

We can peek inside PageObject with `.element` message and discover that indeed it is composed of widgetry called switch: which is a checkbox and blurb: wich is a text_field. That is a privilidged knowledge the tester would require to test the widgetry of the pageobject but to the outside caller we present the PageObject that responds to set and value. PageObject in turn is responsible to talk to the HTML widgetry and that part ought to be encapsulated by the PageObject.

[The above example can be found in the domkey gem specs](https://github.com/rubytester/domkey/blob/master/spec/page_object_decorators_spec.rb#L133). I hope you can find [Domkey](http://github.com/rubytester/domkey) useful. PageObject in domkey uses an awesome [watir-webdriver](http://rubygems.org/gems/watir-webdriver) gem wich implements [Watir API](http://watirwebdriver.com/) over selenium webdriver. [The ideas is Domkey were first presented at Selenium Conf Watir Day 2011](http://www.slideshare.net/testrus/domain-specific-watir-page-objects). Consider Domkey gem next time you need to model Domain Specific PageObjects for your Browser Testing and let me know if that encapsulation works for you.
