---
layout: post
title: Domain Specific PageObject with domkey gem
---

PageObject is not a Browser Page, it could be but to me PageObject is a Domain Specific Object on a Page or on some visible panel on a Page. PageObject is an encapsulation of a behavior required of the widgetry of HTML elements composed togehter and presented to the user.

From the outside PageObject faces the user of the application. ser sets the value and queries for the value of a PageObject.

On the inside PageObject encapsulates the html widgetry required to talk to the Browser and should not be leaked to the outside world. Only when needed a test developer can talk to widgetry with `.element` message and expose the widgetry for unit tests.

Here is an animated gif example of how PageObject is constructed and how it behaves. This is a CheckboxTextField type of pageobject. It has specific behavior for the user. When Checkbox is checked off the Text is cleared and text area control becomes disabled. When The Checkbox is set on, the text field accepts text input.

This PageObject has 3 value states: true, false, 'Some String'. When checkbox is uncheched the PageObject's value is false. When Checkbox is checked but there is no text entered the PageObject's value is true, When Text is entered (with checkbox set to true) then the value is the value of textarea. All those 3 value states are Domain Specific in the world of the user of this PageObject.

This is how a client code would interact with this pageobject to set its value.

```ruby
pageobject = PageObject.new(widgetry, container)

pageobject.set true
pageobject.set false
pageobject.set 'Hello, I am string to set by this pageobject'
```

Notice in the animation that PageObject does not 'leak' to the outside world what wigetry is receving messages inside.

You can examine the PageObject is composed of two elements:

- switch: representing a checkbox
- blurb: representing a textarea.

The keys switch: and blurb: are arbibrary names but signal a system metaphor

Notice that each one of widgets is itself a PageObject. Again we don't leak the fact that switch: is a checkbox. It's a PageObject that responds to `set` and `value`. We can peek inside PageObject with `element` message and discover that indeed it is composed of widgetry called switch: which is a checkbox and blurb: wich is a textfield. That is a privilidged knowledge the tester would require to test the widgetry of the pageobject but to the outside caller we present the PageObject that responds to set and value. PageObject in turn is responsible to talk to the HTML widgetry and that part ought to be encapsulated by the PageObject.


![Animated Gif of git log aliases in action](/images/rubytester-domkey-pageobject-intro.gif)

Hope you can find Domkey gem useful. Here it is http://github.com/rubytester/domkey Consider it next time you need to model Domain Specific PageObjects for your Browser Testing.
