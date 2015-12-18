#Git协同开发

##1.Fork项目
	进入要参与项目的地址：https://github.com/xudabin/test.git，点击Fork,先fork这个项目到自己的项目中。

##2.clone项目到本地
	2.1 把fork过来的项目clone到本地 <br/>
		git clone https://github.com/hujewelz/test.git
		
	2.2 可以新建并切换到开发(dev)分支
		git checkout -b dev
	然后可以在该分支下做开发。
##3. Pull request
	当实现了一个功能，就可以开始提交这个commit了。
	eg: git add test.md
		git commit -m 'add test.md'
		git push -u origin dev
	
	这时候，你进入github网站，进入compare & pull request，填写一些信息crete pull request即可。当项目发起者看到这个pull request时，就会合并到他的项目中了。

##4. 建立与主项目服务器的连接
	4.1 当主项目有更新了怎么办？那么，这个时候我们就需要将主项目的更新合并到本地。 
	git remote add upstream https://github.com/xudabin/test.git
	
	4.2 将更新合并到本地
	    git fetch upstream 
		git merge upstream/master
	或
		git pull upstream master:dev
	将远程仓库的master分支的更新合并到本地的dev分支（合并到哪个分支自己决定，不一定非得是dev分支哦，个人习惯而已）
	这个时候就将主项目的更新合并到本地了。然后就可以继续开发新功能了。
