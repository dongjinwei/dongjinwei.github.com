---
layout: post
title: "使用命令行提交stash pull request"
date: 2014-12-10 10:57:27 +0800
comments: true
categories: 
---
###痛点
由于公司使用stash系统，每次写完提交代码后，都需要登录到网页上指定多个人作为reviewer，有些麻烦，所以想写个ruby脚本，在脚本中将自己的经常指定的几个人作为reviewer固定，并在本地直接发起pull request，不再需要登陆到网页中重复这些工作

###步骤
1. 下载stash命令行工具

		gem install atlassian-stash
2. 配置

		$ stash configure
		Stash Username: XXX
		Stash Password (optional): XXXX
		Stash URL: http://git.xxx.com
		Create a git alias 'git create-pull-request' ? no
3. 创建review.rb脚本

		def main()
    		branch = "develop";
    		if (ARGV[0] != nil)
        		branch = ARGV[0]
    		end
    		system "stash pull-request #{branch} @1 @2 @3"
		end
 
		if __FILE__ == $0
    		main();
		end
###提交代码
前几步没啥区别，就是在最后运行一下上面的脚本
		
		git add .
		git commit -m "XXX"
		git fetch
		git rebase
		git push origin featrue/XX
		ruby review.rb origin/develop
				