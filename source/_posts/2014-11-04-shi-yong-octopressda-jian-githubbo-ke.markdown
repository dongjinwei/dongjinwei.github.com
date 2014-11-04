---
layout: post
title: "使用Octopress搭建github博客"
date: 2014-11-04 14:57:57 +0800
comments: true
categories: 
---
这篇文章介绍在Mac上使用octopress+ruby，利用github pages功能，搭建一个github风格的blog

##安装command line tool
加入你的电脑上已经安装了xcode，则在其preference里可以直接安装这个命令行工具。假设你的mac里没装xcode，则去<https://developer.apple.com/downloads>直接下载dmg包

##安装homebrew
	ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"

##安装gcc
假如已经有了，则跳过此步骤
	
	brew tap homebrew/dupes
	brew install apple-gcc42
	
##安装1.9.3以上版本的ruby
	brew install rbenv
	brew install ruby-build
	rbenv install 1.9.3-p0
	rbenv rehash
使用如下命令，确认默认使用的ruby版本为1.9.3
	
	ruby --version
如果是低于1.9.3的版本，下载rvm进行版本控制
	
	$ curl -L get.rvm.io | bash -s stable
	$ source ~/.bashrc
	$ source ~/.bash_profile
	rvm install 1.9.3
	rvm use 1.9.3  --default
然后再检查一遍版本号，一定要确保ruby版本要大于等于1.9.3

##安装Octopress
	git clone git://github.com/imathis/octopress.git octopress
	cd octopress
	gem install bundler
	rbenv rehash
	bundle install
	rake install
假如没装xcode，要先做特殊处理,不然执行上面的命令会发生错误
	
	sudo xcode-select --switch /usr/bin
	sudo mv /usr/bin/xcrun /usr/bin/xcrun-orig
	sudo vim /usr/bin/xcrun
	sudo chmod 755 /usr/bin/xcrun
	
##配置Octopress
根据自己的需要更改_config.yml中的url、title、subtitle、author字段

##将博客部署到github
1. 在自己的github账号中新建名称类似yourname.github.com的仓库
2. 自动配置上面创建的仓库，需要输入仓库地址

		$ rake setup_github_pages

3. 将博客真正部署到仓库中

		rake generate
		rake deploy
现在就可以访问username.github.com了。注意有可能要等几分钟才能打得开

##开始写博客
使用Octopress提供的task创建一个博文

	rake new_post["title"]
创建出来的文件默认为markdown格式，在`source/_posts`目录下，可以使用 [Mou](http://25.io/mou/)进行编辑保存。写完之后进行提交

		$ rake generate
		$ git add .
		$ git commit -am "Some comment here." 
		$ git push origin source
		$ rake deploy
到此结束！










