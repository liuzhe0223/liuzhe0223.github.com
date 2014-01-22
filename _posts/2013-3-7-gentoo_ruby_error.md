---
layout: post
title: gentoo,ruby error  cannot load such file -- auto_gem (LoadError), resolved
tags: linux
category: note 
---

* 今天gem install 的时候发现error， irb 也会 error， 仔细看error:

```
    /home/zhe/.rvm/rubies/ruby-2.0.0-p0/lib/ruby/site_ruby/2.0.0/rubygems/core_ext/kernel_require.rb:45:in 'require': cannot load such file -- auto_gem (LoadError)
    from /home/zhe/.rvm/rubies/ruby-2.0.0-p0/lib/ruby/site_ruby/2.0.0/rubygems/core_ext/kernel_require.rb:45:in 'require'
```

解决：

```sh
If you are having issues such as "no such file to load -- auto_gem (LoadError)" with gentoo, run the following to see if it corrects your issues:
$ unset RUBYOPT;   sudo env-update
For further information on this specific issue, please see this gist containing more details.
```
