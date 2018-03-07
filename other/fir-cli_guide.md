# fir-cli安装使用说明
## 作用
fir.im-cli 可以通过指令查看, 上传, 编译 iOS/Android 应用.
## 安装配置
1. 安装fir-cli

	```
	gem install fir-cli
	```
	使用fir --version查看是否安装成功
2. 生成token，
3. fir login使用token登录
	
	```
	➜  ~ fir login
Please enter your fir.im API Token: input token here
I, [2016-08-16T09:28:36.254629 #1435]  INFO -- : Login succeed, current  user's email: zhuwenbo@zhujia360.com
I, [2016-08-16T09:28:36.254744 #1435]  INFO -- :
	```

## 发布测试包
4. 切换到项目目录

	```
cd /Users/zwb/Developer/iOS/ZhuJiaYi
	```
5. 使用fir-cli发布

	```
fir b ./ -w -S ZhuJiaYiApplication -C Debug -p -c "筑家易测试包 v1.6.1 20160816"
//-w workspace
//-S scheme
//-C configuration
//-p publish to fir.im
//-c, [--changelog=CHANGELOG]
	```

## 参考文件
1. [github fir-cli](https://github.com/FIRHQ/fir-cli)
2. fir源码路径，如：/Users/zwb/.rvm/gems/ruby-2.3.1/gems/fir-cli-1.5.0/lib/fir/cli.rb

## FAQ
1. 编译报错，添加User Header Search Paths : ${SRCROOT} 选择recusrsive