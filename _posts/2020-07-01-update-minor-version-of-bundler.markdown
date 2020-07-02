---
layout: post
title:  "Update Minor Version of Bundler"
date:   2020-07-01 12:00:00 -0400
categories: ruby
---

The ruby docker image I use for development currently comes with `bundler 1.17.2` installed as its default version. Attempting to update the minor version of the gem via traditional methods has lead to either version 2 being installed, or the `1.17.3` binary not being found.

If you're looking to upgrade from `1.17.2` to `1.17.3` the following should do it:
```ruby
gem install bundler -v 1.17.2
gem uninstall bundler -v 1.17.2
# You will want to find this on your machine - try the `gem env` command
# to check possible places. You just want to delete the gemspec for 1.17.2
rm /usr/local/lib/ruby/gems/2.6.0/specifications/default/bundler-1.17.2.gemspec
gem install bundler -v 1.17.3
```
