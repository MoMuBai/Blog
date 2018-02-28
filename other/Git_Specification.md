# Git使用规范

## 分支
1. 发布用 master 分支，开发在 develop 分支，线上问题修复在 hotfix 分支；
2. 开发不允许在 master 直接提交代码；
3. 分支合并由项目 owner 进行，有冲突找对应开发人员一起合并；
4. 多人开发不同功能可以在一个分支，如 develop；
5. 多人开发同一个功能可以在不同分支，一起合并代码；
6. 多个 develop 分支同时需要提测时，多个 develop 分支合并到test分支提测，test 分支测试通过后合并到 master 分支发布；
7. 不用的分支及时删除，分支越少越好；

## 代码提交
1. Push 前先 Pull；
2. 没有错误（如 crash, 接口500错误）再 Commit / Push 代码；
3. commit message 尽量详细，常用关键字；

	```
	feature 新功能简单描述；
	bugfix 禅道ID bug标题 原因 解决方法（也可以详细记录在禅道）
	Add / Update / Merge / 	Refactor / Deprecating / Remove
	```

## 冲突
1. Conflict找冲突人一起合并代码，确保合并后代码正确运行；

## Tag
1. 发布完成提交 version 和 build 后打tag；
2. Tag 应该和发布的版本一致，如1.6.0；

## 版本回退
1. 不能丢失历史提交记录
2. 不建议使用，会使得历史提交记录丢失

	```
	git reset --hard <target_commit_id>
	git push -f
	```
3. 建议使用，不会使得历史提交记录丢失

	```
	git reset --hard <target_commit_id>
	git reset --soft origin/master
	git add .
	git commit -m "message here"
	```

## GitFlow
1. Production Branch: master
2. Development Branch: develop
3. Feature branch prefix: feature/
4. Release branch prefix: release/
5. Hotfix branch prefix: hotfix/
6. Version tag prefix: empty

