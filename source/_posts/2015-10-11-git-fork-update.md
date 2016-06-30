title: 【转载】Git 怎样保证fork出来的project和原project（上游项目）同步更新
date: 2015-10-11 13:43:10
categories: 编程
tags: [git]
---

## 问题描述： 
当我们  在github上fork出一个项目后，如果原有的项目更新了，怎样保持我们fork出来的项目和原有项目保持同步呢并提交我们的代码更新呢？即怎样保持fork出的项目和上游项目保持更新，怎样创建pull request？关键步骤是使用git 的 rebase 命令。  
<!-- more -->
## 步骤： 

1.  在 Fork 的代码库中添加上游代码库的 remote 源，该操作只需操作一次即可。 
如: 其中# upstream 表示上游代码库名， 可以任意。
```
git remote add upstream https://github.scm.corp.ebay.com/montage/frontend-ui-workspace 
```
2. 将本地的修改提交 commit

3. 在每次 Pull Request 前做如下操作，即可实现和上游版本库的同步。 
	```
	git remote update upstream 
	git rebase upstream/{branch name} 
	```
	需要注意的是在操作3.2之前，一定要将checkout到{branch name}所指定的branch, 如: 
	```
	git checkout develop
	```

4. Push 代码到 Github
```
git push
```