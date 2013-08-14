---
layout: post
title: Beginning chruby with ruby-install on Linux Mint 13 Maya
---

## Motivation for writing this article

I wanted to have an easy post whose audience are testers working with ruby, watir-webdriver and who may not understand too much about what a `user` acccount is versus `root` account -- and why do I have to use this `sudo` thing, and why is apt-get always sudo apt-get and not just apt-get,and why don't I just change permission of the enitre system to myself so I don't have to use sudo, and what are those login shells and cli and gnome-terminal and oh my why can't I just double click stuff to make it run??? so...

So my motivation is to reach those testers who are novice linux users. - On the other hand I may completely miss my audience here and basically write this for my imaginary audience which is not found in reality. YMMV!

## Motivation for using chruby

My motivation for using `chruby`? 'Cause as a tester I need to run my ruby tests on ruby 1.9.3 or maybe on ruby 2.0, or on ruby 1.9.3 with rubygems 1.8.25 or some other configuration, and ruby switchers are all the rage with rvm and rbenv - but I needed something simple, the simplest thing that can possibly work and the simplest thing I can possibly understand and not much more. So `chruby` it is. Thanks to [@postmodern_mod3](https://twitter.com/postmodern_mod3)

Some of the smarties in the room might say: why don't you use `sudo update-alternatives` to designate the ruby interpreter. Yeah - but I still would have to
install ruby. With chruby and ruby-install I can accomplish all of this faster

And for those of you on OS X can use `brew install chruby`, it's even easier.

## Technical Preamble

.. a bit of a mumble: skip it if you don't care but you should if you want to understand what we'll refer to as ruby system.

As usual we start with test failure.

I am using Linux Mint 13 (Maya), based on Ubuntu Precise. The machine does not have ruby installed. So system has no ruby. we need to tell system to get ruby. Our system uses the magic of `apt-get` to install it so open a terminal and run this:

    sudo apt-get update
    sudo apt-get install ruby1.9.1

Aha, this may fail for you becuase your system you don't have other tools needed to build ruby. (enough with system jokes)...

OK, run this to get the tools for building ruby (or you could use `ruby-install` which handles those dependencies for you! Yeah for `ruby-install` talking to apt-get):

    sudo apt-get install build-essential zlib1g-dev libyaml-dev libssl-dev libgdbm-dev libreadline-dev libncurses5-dev libffi-dev
    # and build that ruby
    sudo apt-get install ruby1.9.1

It should take few minutes and we are done. You should have ruby version: 1.9.3p0 (2011-10-30 revision 33570) [x86_64-linux]).

> Sidenote: why not use sudo apt-get install ruby1.9.3 - yes, you can use that it's the same thing - ruby1.9.3 package is the same as ruby1.9.1 - `sudo apt-get cache show ruby1.9.1` outputs Version: 1.9.3.0-1ubuntu1)


With this version of ruby we get rubygems 1.8.11 and If I try running `sudo gem update --system` to update rubygems I get a warning that I should not update "because it will overwrite the content of the rubygems Debian package, and might break your Debian system in subtle ways" (yes, there is a workaround if you must `sudo gem install rubygems-update -v x.y.z` and later run `sudo update_rubygems` but that's not the point right now.)

At this point we can leave the magic of apt-get and enter the magic of chruby and ruby-install instead.

## Using chruby and ruby-install

So let's get some more rubies installed (at this point reader may ask again: why not rbenv? rvm? - well, simple things first and let's find out what's really going on before adding additional complexity)

### install chruby

What is `chruby`? It's a tool for switching ruby interpreters installed on your machine. How do you install `chruby`?. [Read this](https://github.com/postmodern/chruby#install).

- You download it,
- unpack it
- and run installer `sudo make install` (prompt will ask you for your password)

chruby installs itself in `/usr/local/share/chruby/` and it has 2 files `chruby.sh` and `auto.sh`. We'll talk about them later.

> A word about `sudo` and `su` if you find yourself a bit confused. Basically you operate on your `system` as two actors in a system, as `user` and as `root`. Your login to your machine with your `username` and and your home directory is referred to as `~/` located at `/home/username/` on filesytem. You never login to your `system` as `root` user. (ok, enough with system. let's start calling it machine or comptuter - hello computer). You must use `sudo` to gain extra permissions to modify files that belong to superuser so when using `sudo` you are asked for your password. Or you can switch who you are by by doing `sudo` or `su`. - I think I sufficiently confused you now.


### use chruby to switch installed rubies

Well. We need rubies installed but we don't have any. We only have `system` ruby from apt-get, or maybe you didn't even bother with apt-get so you don't have any. So let's install ruby 2.0 with ruby-installer first before we configure chruby to switch between them.

### install ruby-install

What is `ruby-install`? It's a tool for installing rubies on your system. How do you install `ruby-install`? [Read this](https://github.com/postmodern/ruby-install#install) - It's the same process as installing `chruby`. It installs itself in `/usr/local/share/ruby-install`.

After installing `ruby-install` run the following command `ruby-install` to see what known version of rubies you can install with it.

### install ruby with ruby-install

let's install ruby 2.0

    sudo ruby-install ruby 2.0
    # the above line will install into /opt/rubies directory owned by root
    ruby-install ruby 2.0
    # and this one into ~/.rubies/ owned by your

If you are not sure which one and not sure what you should do select the second one and install to your ~/.rubies/ dir. I install as root to /opt/rubies becuase I split responsibilities between root and user - ruby and rubygems are owned by root - all installed gems are owned by user (you may disagree. not a problem, your conceptual model diffs from mine)

Now go get coffee since this will take few minutes

> Kudos to `ruby-install` for ensuring you have all the libs needed to for compiling ruby on your machine. That's a great feature. So if you didn't bother installing ruby with apt-get and let's say don't have build-essential package then `ruby-install` will handle that for you. Aha, that's what I'm talking about.

### Configure chruby for your shell (your login shell)

When you open your commandline (Terminal) your probably use `gnome-terminal`. No? ok, whichever terminal you use you want to set the the login shell to make sure your ~/.profile is being read as you start your shell. In `gnome-terminal` open Edit>Profile Preferences and access tab Title and Command. Check the checkbox labelled 'Run Command as login shell'. If you don't know what a login shell is we need don't worry. The rule is that login shell will read your ~/.profile (or your ~/.bash_profile if you use it)

And in your ~/.profile is what we need to enable chruby in your shell. by default non login shell you will not have access to `chruby`.

#### enable chruby for all login shells

I told you that `chruby` installs two files `chruby.sh` and `auto.sh`. We now want to deal with the first one. `auto.sh` will be mentioned later.

Switch to be root with `su -lp` (or just sudo -s). you will be asked for password.
Now run the following commands:

    # enables chruby globally for /etc/profile
    echo "source /usr/local/share/chruby/chruby.sh" > /etc/profile.d/chruby.sh

    # make it executable, to execute it every time you open an new login shell
    chmod +x /etc/profile.d/chruby.sh

    # leave root session and return to your user session
    exit

OK, `chruby` will be availabe in your shell when you start a new terminal session (or source /etc/profile.d/chruby.sh) in your current session.

#### enable default ruby in your profile

If you want to set an default ruby with every login, add it to your ~/.profile

    echo "chruby 2.0" >> ~/.profile

Everytime you open a terminal and start a new session you will have ruby 2.0 as your ruby interpreter.

## enable auto switching per project

Now we can talk about that second file `auto.sh`. With this file `chruby` reads `.ruby-version` file located in current directory and automatically invokes the correct ruby interpreter. You don't have to manually `chruby` nothing at this point. `auto.sh` using `.ruby-version` does the work for you.

So, again switch to be root with `su -lp` (or just sudo -s). you will be asked for password.
Now run the following commands:

      # append auto.sh sourcing to previously created chruby
      echo "source /usr/local/share/chruby/auto.sh" >> /etc/profile.d/chruby.sh

      # leave root session and return to your user session
      exit

To test it it's best to have 3 rubies installed.
- `system` ruby 1.9.3p0,
- and two rubies installed with `ruby-installer`
-- 1.9.3p448
-- and 2.0.0p247

Open your terminal and run some tests:

    ruby -v
    # you should see 2.0.0p247 if you ~/.profile has the line `chruby 2.0` as we set earlier

    chruby
    # this should output 2 entries

      ruby-1.9.3-p448
    * ruby-2.0.0-p247

    # notice the * indicates the current ruby interpreter selected as was shown by ruby -v

    ruby system
    # we now disengage chruby (warp engines offline at this point)

    ruby -v
    # you shoudl see 1.9.3p0 comign from your system (you know the one installed with apt-get)

    chruby
      ruby-1.9.3-p448
      ruby-2.0.0-p247

    # notice there is no * by any entry here (chruby is offline)

    # now switch to 1.9
    chruby 1.9

    chruby
    * ruby-1.9.3-p448
      ruby-2.0.0-p247

    # the * should be by 1.9.3

    ruby -v
    # you should see 1.9.3p448 now as your ruby

So now create a test directory where we can demonstrate auto switching behavior.

    mkdir ~/chrubytester
    cd chrubytester
    echo "ruby-2.0.0-p247" > .ruby-version

And let's demonstrate shall we?

    ruby -v
    # surprise. now now you see 2.0.0p247. chruby used .ruby-version to switch itself


A word of WARNING at this time if you switch back to your home dir you will end up with system ruby and now with what you started previously.

    cd
    ruby -v
    # should report now a system ruby 1.9.3p0
    # run this:
    chruby
    # and you should see the entries but no * by any of them
      ruby-1.9.3-p448
      ruby-2.0.0-p247

What happened? The .ruby-version entry ensures the ruby version for that dir only. Outside of it chruby engines are offline. You need to manually engage them back with chruby 2.0 for example.

It all makes sense now, yes? (if not I can't help you)

## Conclusion

none.