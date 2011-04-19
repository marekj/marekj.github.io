--- 
layout: post
title: Deconstructing Rspec Runner
date: 2009-04-24
published: true
categories:
  - rspec
  - watir
--- 
## Moving from TestUnit runner to Rspec Runner for Watir Functional Tests

For my Watir 'usecase' tests I have been running test/unit test cases with the `UI::Console::TestRunner.run` . 
The reason is that I need to decide 'when' to run some test and when to run some 'setup usecase' routines. 
In a 'functional test' steps of execution sequence and timeline matter while in classic unit tests you assert 'state and behaviour' of a class in isolation. 

In Functional tests I have to setup preconditions: load the browser and login. These 'steps' are tests as well in their own right but mostly they play a 'role' of 'flow setup' 
for later tests hence they introduce dependency in 'functional testing'. This dependency rarely exist in Test Unit style testing, it is built to not care about 
this kind of 'dependency setup' - as a matter of fact it rejects it, it does not run `test_xxx` methods sequentially by default but alphabetically 
(this fact causes constant questions on [watir-general mailing list](http://groups.google.com/group/watir-general). 
The workaround is to use [Watir::TestCase](http://wtr.rubyforge.org/rdoc/classes/Watir/TestCase.html) that sorts them sequentially. 

## How Test Unit runs tests

By default if you run a file with `ruby testfile.rb` that containes Test Unit TestCase the code will be loaded in ObjectSpace and messaged, 
counted, sorted and executed one by one. Each TestCase class is independent of another. 
Each `test_xxx` in independent of another `test_xxx`. TestCase is usually a collection of tests using the same test fixture. 
The fixture is setup and teardown for each test. 

This simply will not do for my 'browser functional tests' because I need to be able to specify 'when' I want the tests to start and 'when' to finish. 
The question of 'WHEN' is important in functional sequential tesing. Timeline is introduced as dependency that must be managed. 
I test by moving from context to context and each tescase has a role that introduces dependency for the next testcase. 
The entire model of TestUnit fails for me when it comes to flow based customer facing tests since I am using TestUnit in the way not intended.

My cruch so far has bee to disallow the automatic TestCase runs. Instead I invoke them at a particular time with the UI::Console::TestRunner.run. 

Here is how I weave them into my test infrustructure code.

{% highlight ruby %}
# get testrunner 
require 'test/unit/ui/console/testrunner'

# run some code for context setup
# get the testcase code and run it with TestRunner.run
require 'my_testcase'
Test::Unit::UI::Console::TestRunner.run MyTestCase

# and clean up context and setup for the next test
# do some code for next context setup
# get the testcase2 code and run it with TestRunner.run
require 'my_testcase2'
Test::Unit::UI::Console::TestRunner.run MyTestCase2

{% endhighlight %}

or when I run a 'usecase scenario' I feed tests into 'timeline' like so

{% highlight ruby %}
@test_list = ["MyTestCase", "MyTestCase2", "MyTestCase3"]
def run_test(test_number)
  TestRunner.run @test_list[test_number].constantize
end
# setup context for test 1
run_test 0
# setup context for test 2
run_test 1

{% endhighlight %}

This code organization ensures the 'when' Test::Unit will run and 'when' it finishes. 
Now I can control the flow of test setups and I can use testcases as 'roles' to play some part in moving context forward in timeline of step sequence to run yet more tests.

## Replacing TestUnit TestCase with Rspec ExampleGroup

Now I want to move to rspec and duplicate the same runner functionality. The Rspec style BDD block looks like this

{% highlight ruby %}
describe "Something" do
  it 'should be named someting' do
    "something".should == "something"
  end
  it 'should not be something else' do
    "something".should_not == "something else"
  end
end

{% endhighlight %}

The above 'syntax' of describe blocks can be also written as class objects (which will perhaps take away some of the magic from it)

{% highlight ruby %}
class SomethingExamples < Spec::Example::ExampleGroup
  def should_be_named someting
    "something".should == "something"
  end
  def should_not_be_something_else
    "something".should_not == "something else"
  end
end

{% endhighlight %}

and you can exeucte it just like you execute describe block. 

## How rspec executes tests.

There are usually 3 ways you would execute rspec examples. 

- With ruby 
- With spec executable
- With Rake::SpecTask

Usually when you have a file.br that containes code that is wrapped in `describe do end` block all you need to do it is add a line `requrire 'spec/autorun'` and run the file. 
The autorun will take care of invoking rspec. Similar with the spec executable you provide one or more files to run and they execute (in sequence). 

The drawback is that any code that is not called from the describe blocks will be executed first an then at_exit all the examples will run. 
Hm... that's not good for me. I either have to go with rspec runner or I am out of luck or I need to change my conceptual model I am dragging over from TestUnit.

With rspec is it any reason to even have code outside of it? Maybe I need to rethink my strategy and come up with new conceptual model.

I started writing this post thinking that I need to find an equivalent to TestRunner.run TestCase mechanism 
but now I am thinking maybe rspec itself is just it and there is no need to run outside of it.

