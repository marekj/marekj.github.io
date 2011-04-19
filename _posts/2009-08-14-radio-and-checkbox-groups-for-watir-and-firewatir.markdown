--- 
title:  Radio and Checkbox Groups for Watir and Firewatir
date: 2009-08-14 15:18:31.109000 -05:00
layout: post
---

## Accessing Radios and Checkboxes in Watir

Radio and Checkbox access is sometimes difficult because to access a particular control you need to know its 'value' attribute. 
What if you could access a group of radios or checkboxes by the 'name' attribute only no matter what the values are for 
each individual radio or checkbox. So I wrote a radio_group and checkbox_group methods to simplify that access.

Let's start with the typical 'watir' code for accessing individual radios and checkboxes:

{% highlight ruby %}
# 3 radios in a group. 
browser.radio(:name, 'rname', 'rvalue1')
browser.radio(:name, 'rname', 'rvalue2')
browser.radio(:name, 'rname', 'rvalue3')

# 3 checkboxes in a group
browser.checkbox(:name, 'cbname', 'cbvalue1')
browser.checkbox(:name, 'cbname', 'cbvalue2')
browser.checkbox(:name, 'cbname', 'cbvalue2')
{% endhighlight %}

Each one of the radios or checkboxes you have to access individually. However if you look at radios as a group you can see that 
it behaves like a single select_list control; you select one and only one radio. Selection of one unselects the others. That makes sense to me.

Similar with checkboxes; as a group you can see that it behaves like multi select_list control; you select none, one or more items.

## Watir radio_group and checkbox_group methods

Radios and Checkboxes form a control set, a group when they share the value of a 'name' attribute (contained by a form) so instead of accessing them individually like items in a select_list we can access them as a 'radio_group' using the 'name' attribute and bypassing the 'value' attribute.
[peruse the HTML401 spec for more info on form elements and name attribute usage for identification](http://www.w3.org/TR/html401/interact/forms.html)

Here is an example:

{% highlight ruby %}
# access all radios as a group
browser.radio_group('rname')

#access all checkboxes as a group
browser.checkbox_group('cbname')

{% endhighlight %}

The client code is simplified. You access radios and checkboxes without knowing their individual 'values'. 

Examples: 

{% highlight ruby %}

browser.radio_group('rname').size # => 3
browser.radio_group('rname').values # => ['rvalue1', 'rvalue2', 'rvalue3']


browser.checkbox_group('cbname').size # => 3
browser.checkbox_group('cbname').values # => ['cbvalue1', 'cbvalue2', 'cbvalue3']

{% endhighlight %}

To accompany the radio_group method I've also added radio_groups method that returns collection of groups on a page; similar with checkbox_group and checkbox_groups. 

Word of caution: If you have two forms on a page and each form has a group of radios and each group shares the same value for 'name' attributes we'll run into a problem. Do you see which one? Yes, the radio_group will report both sets of radios in two separate forms as one group. First of all you shoud not do that in HTML but if you do you have to delimit containement and address radio_group from each form and not from the root document.
Anyway, have a look at all the code at in [watirloo gem](http://github.com/marekj/watirloo/tree/master) Take a look at specs, they run for both IE and Firefox.

This is a very helpful extension to 'watir'. It has saved me many hours of work.

