# Cocoapods安装说明

## 安装homebrew
* 因为安装rvm的时候会用homebrew来安装一些依赖，如：autoconf, automake, libtool, pkg-config, libyaml, readline, libksba, openssl等，所以要先安装homebrew
* 官网地址：[http://brew.sh/](http://brew.sh/)
* 安装命令： 

	```
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
	```

## 安装rvm
* cocoapods1.0.1需要ruby2.3以上版本，OSX 10.11自带的ruby版本为2.0，所以要安装新版的ruby，推荐使用rvm来管理不同版本的ruby环境。查看系统ruby版本号：

	```
ruby --version
ruby 2.3.1p112 (2016-04-26 revision 54768) [x86_64-darwin15]
	```
* 官网地址：[http://rvm.io/](http://rvm.io/)
* 安装命令：

	```
\curl -sSL https://get.rvm.io | bash -s stable --ruby
	```

* 安装完成后使用

	```
~ rvm list
rvm rubies
=* ruby-2.3.1 [ x86_64 ]
# => - current
# =* - current && default
#  * - default
	```
查看当前有的ruby版本，如果rvm命令找不到的话关闭终端重新打开即可。
* 如果没有ruby版本的话，使用

	```
rvm install 2.3.1
	```
安装ruby2.3.1版本
* 设置使用和默认的ruby版本

	```
rvm use 2.3.1 --default
```

## 安装cocoapods
* 官网：[https://cocoapods.org/](https://cocoapods.org/)
* 安装命令：

	```
sudo gem install cocoapods --version=1.0.1
	```
* 查看pod是否安装成功

	```
pod --version
1.0.1
	```