---
layout: post
date: 2015-10-25
tags:
- install jekyll
categories:
- jekyll
---

```bash
sudo apt-get install ruby
sudo apt-get install ruby1.9.1-dev
gem sources --remove https://rubygems.org/
gem sources -a https://ruby.taobao.org/
gem sources -l         #查看是否只有taobao镜像
gem update --system    #更新RubyGems软件
sudo gem install jekyll #可能需要多试几次
```
