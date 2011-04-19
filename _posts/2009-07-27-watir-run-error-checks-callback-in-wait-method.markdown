--- 
title:  Watir run_error_checks Callback in Wait Method
layout: post
---

## Register error_checkers and let Browser run them after page loads

When `Watir::IE` runs `wait` method (most of the time implicitly to synchronize execution) it includes a call to `IE.#run_error_checks` mehtod. When `#run_error_checks` is invoked it takes each Proc stored in `@error_checkers` array and calls it with 'self' as argument (self being an IE instance). This is a great mechanism to add your own custom checkers on page loads.

The run_error_checks code looks like this:

{% highlight ruby %}

  # this method runs the predefined error checks
  def run_error_checks
    @error_checkers.each { |e| e.call(self) }
  end

{% endhighlight %}

The procs stored in @error_checkers array should be a lambda or a Proc.new. I prefer lambda for its argument checking and return from the method (this is beyond scope for now)

The pattern is something like this:

{% highlight ruby %}

  ERR_CHECKER_EXAMPLE = lambda do |browser|
    browser.blabla
    raise "some error" unless blabla == expected_value
  end

  # and in client code
  ie.add_checker ERR_CHECKER_EXAMPLE # => registers the checker
  ERR_CHECKER_EXAMPLE.call(ie) # => runs the checker right away without registering it

{% endhighlight %}

The checker code can be assigned to a constant because it's a piece of code that will not change (it's a function) and it's easy to just store it in some module. In Watir codebase there already is an example of checking for HTTP errors in 'watir/contrib/page_checker' constructed as lambda error_checker which calls check_for_http_errors method on very reload of the page. Take a look at that file.

## How does IE track the error_checkers to execute?

By default the `@error_checkers` is initialized as an empty array when new instance of IE is created, so no checkers are being ran on wait method by default. But you can add_checker or disable_checker after you initialize a new IE to be run automatically.

Here is an example of how you can create and organize a collection of error checkers and then either register them with IE instance or just run them as needed.

{% highlight ruby %}

module ErrorCheckers
  include Watir::Exception
 
  # returns all defined checkers as hash: key as name and value as proc
  def self.checkers
    @checkers ||= {}
  end
 
  # returns proc tagged by key given, raises exception if key not found
  # ErrorCheckers[:bla] # => Proc stored as key :bla
  #
  def self.[](key)
    raise "No ErrorChecker with key: #{key.inspect}" unless checkers.has_key? key
    checkers[key]
  end
 
  # assign the block as proc to key in a checkers hash
  # the block must include IE instance object
  # as argument so we can call(ie) to execute a checker explicitly
  #
  # create :example_checker do |ie|
  #   # your code here that uses ie ref
  # end
  #
  def self.create(key, &block)
    checkers[key] = block
  end

end

{% endhighlight %}


I also repackaged the code in 'watir/contrib/page_checker' in this format because I want and manage it with the collection of ErrorCheckers

{% highlight ruby %}

# repackage the code and stored it ErrorCheckers[:check_for_http_error]
ErrorCheckers.create :check_for_http_error do |ie|
  if ie.document.frames.length > 1
    1.upto(ie.document.frames.length) do |i|
      begin
        ie.frame(:index, i).check_for_http_error
      rescue UnknownFrameException
          # frame can be already destroyed
      end
    end
  else
    ie.check_for_http_error
  end
end


{% endhighlight %}

Here is another invented example of error_checker. Let's say you want to verify that you should always be on a secure page with 'https' protocol on every page load. The checker might be constructed and then called with IE instance like this.

{% highlight ruby %}

ErrorCheckers.create :not_secure do |ie|
  raise 'expected https secure page' unless ie.url[0,5] == 'https'
end

# request secure page and verify it loaded with https
ie.goto "https://mail.google.com/"
ErrorCheckers[:not_secure].call ie # => should not raise exception 

# here request secure but be redirected to unsecure example
ie.goto "https://google.com/"
ErrorCheckers[:not_secure].call ie # => should raise exception. google does not load https here

{% endhighlight %}

So now you can see how you can define your own checkers and call them on IE instance or register then with browser.

You can also create a shortcut to regiser all your defined checkers (if you need to) as a bulk approach. Here is an example of how that may work with `ErrorCheckers.listen(ie)`

{% highlight ruby %}
module ErrorCheckers

  # instruct the given browser ie to execute the checkers on IE.run_error_checks call
  def self.listen(ie)
    checkers.each_value do |v|
      ie.add_checker v
    end
  end
end

{% endhighlight %}

So in conclusion. You can define your own error checkers and you can either call the checker yourself at some defined points in your test with `ErrorCheckers[:keyword].call(browser)` or delegate the calls to browser that will just run those checkers for you on every wait method call with `browser.add_checker ERR_CHECKER` or use a bulk approach with `ErrorCheckers.listen(browser)`

I'll be rolling this code into Watirloo, helper lib for Watir

