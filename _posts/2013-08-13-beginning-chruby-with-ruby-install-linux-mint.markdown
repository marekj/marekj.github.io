---
layout: post
title: Beginning chruby with ruby-install on Linux Mint 13 Maya
---

Our motivation for using `chruby`: I am using Linux Mint 13 (Maya), based on Ubuntu Precise.

## Preamble

.. a bit of a mumble: skip it if you don't care:

I upgraded the `system` ruby with

    sudo apt-get update
    sudo apt-get install ruby1.9.1

At this time ruby version is: 1.9.3p0 (2011-10-30 revision 33570) [x86_64-linux]).

> You may notice that the ruby1.9.3 package is the same as ruby1.9.1
  `sudo apt-get cache show ruby1.9.3` outputs Version: 1.9.3.0-1ubuntu1)

With this version of ruby we get rubygems 1.8.11.
If I try running `sudo gem update --system` to update rubygems I get a warning that I should not update
"because it will overwrite the content of the rubygems Debian package, and might break your Debian system in subtle ways"


Never mind what's in apt-get. Use chruby and ruby-install instead.

## Using chruby and ruby-install

So let's get some more rubies installed (at this point reader may ask: why not rbenv? rvm?
well, simple things first and let's find out what's really going on before adding additional complexity)

### install chruby

How do you install `chruby`?. Read this https://github.com/postmodern/chruby#install.
You download it, unpack it and run installer `sudo make install` (prompt will ask you for your root password)

chruby installs itself in `/etc/local/share/chruby/` and it has 2 files chruby.sh and auto.sh. We'll talk about them later.

### use chruby to switch installed rubies

Well. We need rubies installed but we don't have any. We only have `system` ruby from apt-get.
So let's install ruby 2.0 with ruby-installer first before we configure chruby to switch between them.

### install ruby-install

How do you install `ruby-install`. Read this: https://github.com/postmodern/ruby-install#install
It's the same process as installing `chruby`. you can find the installation at `/usr/local/share/ruby-install`.

After installing `ruby-install` run the follwing command `ruby-install` to see what known version of rubies you can install with it.

### install ruby with ruby-install

let's install ruby 2.0

    sudo ruby-install ruby 2.0
    # the above line will install into /opt/rubies directory
    ruby-install ruby 2.0
    # and this one into ~/.rubies/

If you are not sure which one and not sure what you should do select the second one and install to your ~/.rubies/ dir.

Now go get coffee since this will take few minutes

### Configure chruby for your shell (your login shell)

When you open your commandline (Terminal) your probably use `gnome-terminal`.
No? ok, whichever terminal you use you want to set the the login shell to make sure your ~/.profile is being read as you start your shell.
In `gnome-terminal` open Edit>Profile Preferences and access tab Title and Command.
Check the checkbox labelled 'Run Command as login shell'.
If you don't know what a login shell is we need don't worry.
The rule is that login shell will read your ~/.profile (or your ~/.bash_profile if you use it)

And ~/.profile is what we need to enable chruby in your shell.

#### enable chruby for all shells

switch to be root with `su -lp` (or just sudo -s). you will be asked for password.
Now run the following commands:

    # enables chruby globally for /etc/profile
    echo "source /usr/local/share/chruby/chruby.sh" > /etc/profile.d/chruby.sh

    # make it executable, to execute it every time you open an new shell
    chmod +x /etc/profile.d/chruby.sh

    # leave root session and return to your user session
    exit

OK, `chruby` will be availabe in your shell when you start a new terminal session (or source /etc/profile.d/chruby.sh) in your current session.

#### enable default ruby in your profile

If you want to set an default ruby with every login, add it to your ~/.profile

    echo "chruby 2.0" >> ~/.profile


## Conclusion

none.