---
layout: post
title: rails生成新项目出错-ubuntu 
tags: rails,ubuntu
category: 遇到的error
---

rails 生成新项目时 bundle install 出错:

	Gem files will remain installed in /home/mlzboy/.rvm/gems/ruby-1.9.3-p0/gems/sqlite3-1.3.5 for inspection.
	Results logged to /home/mlzboy/.rvm/gems/ruby-1.9.3-p0/gems/sqlite3-1.3.5/ext/sqlite3/gem_make.out
	An error occured while installing sqlite3 (1.3.5), and Bundler cannot continue.
	Make sure that `gem install sqlite3 -v '1.3.5'` succeeds before bundling.

应该是缺少需要的sqlite的库文件

	sudo apt-get install libsqlite3-dev

解决
