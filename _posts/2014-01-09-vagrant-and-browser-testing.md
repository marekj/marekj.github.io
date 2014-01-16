---
layout: post
title: Vagrant Setup on Mac OS X for the greater glory of browser testing
---

## Field Notes: setup

`brew cask install virtualbox`
`==> Success! virtualbox installed to /opt/homebrew-cask/Caskroom/virtualbox/4.3.6-91406`

and

`brew cask install vagrant`
`==> Success! vagrant installed to /opt/homebrew-cask/Caskroom/vagrant/1.4.2`


## Field Notes: CentOS box

`vagrant add gox centos https://github.com/2creatives/vagrant-centos/releases/download/v6.5.1/centos65-x86_64-20131205.box`

`vagrant init centos`

`vagrant up`

### rvm options

`vagrant ssh`

`yum update` - well not sure if needed but why not

Install rvm

https://rvm.io/rvm/install

reload bash to pick up rvm `bash --login`

test rvm installed

        $ type rvm | head -1

list rubies you can install with rvm

        $ rvm list remote

gives you list of binaries proper names to invoke with rvm install
here they are => https://rvm.io/binaries/centos/6.4/x86_64/

        $ rvm install ruby-2.0.0-p353 --binary --max-time 40

binary switch ensures the installation of precompiled ruby so no need to build it locally. this is faster.
--max-time switch is needed because on vm machines it times out after few seconds (not sure what the default is) so le't make it at least 20 or even 40.

And finally setup default

        $ rvm use ruby-2.0.0-p353 --default

That's it. I am not using gemsets here but if I was to be so picky about the test isolation for a probject I could do have a 'testbox' gemset

        $ rvm use ruby-2.0.0-p353@testbox --create --default



My Issue: vagrantfile installs rvm into /usr/local/rvm if I run provision with shell inline or path. This is strange. I have no time to investigate who I should force vagrant to install rvm into /home/vagrant/.rvm instead.


