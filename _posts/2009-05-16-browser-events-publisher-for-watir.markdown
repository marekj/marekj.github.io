--- 
title:  Browser Events Publisher for Watir
layout: post
---

## Catpure Events from IE Browser for Watir tests

I thought of saving an html of a browser, not the regular html you get in response but the actual DOM html. 
So the best way would be to access it with `Watir::IE#html` method. (note to self: provide example of javascript generated content in dom to prove point)

But I want to automatically save html based on browser events. Microsoft points me to two events generators I might use. One is a Browser generator
[DWebBrowserEvents2](http://msdn.microsoft.com/en-us/library/aa768283(VS.85).aspx) (I am using WinXP and IE6 for this coding solution,
I think there is a DWebBrowserEvents3 that adds something in IE8 but we can deal with this later). -- ,and another is
[HTMLDocumentEvents2](http://msdn.microsoft.com/en-us/library/aa769764.aspx) that generates events on `document`

So here is some code. I got this class as an observable object so I can register listeners to do stuff when some event I care about occurs. 

{% highlight ruby %}

require 'observer'
 
# InternetExplorer Object DWebBrowserEvents2 is an Interface for Events.
# We can hook into the events and intercept the events we care to catch.
# Every time an event we care to watch for occurs the Publisher notifies observers.
# Extra: you can also build a publisher that listenes to 'HTMLDocumentEvents2' of ie.ie.document object
# and notify listeners to onclick events if you need to
class BrowserEventsPublisher
  include Observable
  
  def initialize( ie )
    puts "BrowserEventsPublisher started for #{ie.hwnd}"
    @events_to_publish = %w[BeforeNavigate2 DocumentComplete NavigateError NewWindow3 OnQuit]
    @ie, @ie_object = ie, ie.ie
    @event_sink = WIN32OLE_EVENT.new( @ie_object, 'DWebBrowserEvents2' )
  end
  
  def browser
    return @ie
  end

  def run
    return unless browser.exists?
    @alive = true
    @events_to_publish.each do |event_name|
      @event_sink.on_event(event_name) do |*args|
        changed
        notify_observers( event_name )
        @alive = false if event_name == "OnQuit" 
      end
    end
    puts "start publishing"
    #loop { WIN32OLE_EVENT.message_loop }
    begin 
      WIN32OLE_EVENT.message_loop
    end while @alive
    puts "stopped liestening. browser no longer available for listening"  
  end
end

{% endhighlight %}

That's just the beginning. Now you also need to require 'watir' which automatically requires win32ole. 
I can now use this for sending messages to the listeners with the event name and I can capture a screenshot or I can save html of  a browser on event sent. 

Next a listener. Here is an example of listener that triggers HtmlSaver on BeforeNavigate2.

{% highlight ruby %}
# Generic Observer of BrowserEventsPublisher.
# implements update method of an observer to be notified by publisher of events
class BrowserEventsListener
  
  def initialize( events_publisher )
    puts "BrowserEventsListener started for #{events_publisher.browser.hwnd}"
    events_publisher.add_observer self
    @saver = HtmlSaver.new events_publisher.browser
  end

  def update event_name
    puts event_name
    @saver.save(:request) if event_name == "BeforeNavigate2" #save the html page as is right before submit
  end
  
end
{% endhighlight %}

the work continues

