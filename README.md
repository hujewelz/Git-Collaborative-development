#Git协同开发
##前言
 很多项目开发都会采用git这一优秀的分布式版本管理工具进行项目版本管理，使用github开源平台作为代码仓库托管平台。由于git的使用非常灵活，在实践当中衍生了很多种不同的工作流程，不同的项目、不同的团队会有不同的协作方式。

本文将介绍一种前人已经在各种大小项目中经过千锤百炼总结出来的一种比较成功的git工作流，这种工作流已经被成功用于许多团队开发当中。掌握git，掌握这种工作流，对大家以后的学习、开发工作大有好处。先来开一张图：<br/>

![image](https://github.com/hujewelz/Git-Collaborative-development/blob/master/iamge/1.png)

上面一张图展示了一种使用git进行项目协同开发的模式，接下来会进行详细介绍。	

##仓库（Repository）

在项目的开始到结束，我们会有两种仓库。一种是源仓库（origin），一种是开发者仓库。上图中的每个矩形都表示一个仓库，正中间的是我们的源仓库，而其他围绕着源仓库的则是开发者仓库。

###源仓库
在项目的开始，项目的发起者构建起一个项目的最原始的仓库，我们把它称为origin。源仓库的有两个作用：

* 汇总参与该项目的各个开发者的代码
* 存放趋于稳定和可发布的代码
源仓库应该是受保护的，开发者不应该直接对其进行开发工作。只有项目管理者（通常是项目发起人）能对其进行较高权限的操作。

###开发者仓库

上面说过，任何开发者都不会对源仓库进行直接的操作，源仓库建立以后，每个开发者需要做的事情就是把源仓库的“复制”一份，作为自己日常开发的仓库。这个复制，也就是github上面的fork。

每个开发者所fork的仓库是完全独立的，互不干扰，甚至与源仓库都无关。每个开发者仓库相当于一个源仓库实体的影像，开发者在这个影像中进行编码，提交到自己的仓库中，这样就可以轻易地实现团队成员之间的并行开发工作。而开发工作完成以后，开发者可以向源仓库发送pull request，请求管理员把自己的代码合并到源仓库中，这样就实现了分布式开发工作，和最后的集中式的管理。

###分支（Branch）

分支是git中非常重要的一个概念，也是git这一个工具中的大杀器，必杀技。在其他集中式版本管理工具（SVN/CVS）把分支定位为高级技巧，而在git中，分支操作则是每个开发人员日常工作流。利用git的分支，可以非常方便地进行开发和测试，如果使用git没有让你感到轻松和愉悦，那是因为你还没有学会使用分支。

在文章开头的那张图中，每一个矩形内部纷繁的枝蔓便是git的分支模型。可以看出，每个开发者的仓库都有自己的分支路线，而这些分支路线会通过代码汇总映射到源仓库中去。

我们为git定下一种分支模型，在这种模型中，分支有两类，五种

* 永久性分支
	* `master branch`: 主分支
	* `develop branch`：开发分支
	
* 临时性分支
 	* `feature branch`：功能分支
	* `release branch`：预发布分支
	* `hotfix branch`：bug修复分支
	
####永久性分支

永久性分支是寿命无限的分支，存在于整个项目的开始、开发、迭代、终止过程中。永久性分支只有两个`master`和`develop`。

**master**：主分支从项目一开始便存在，它用于存放经过测试，已经完全稳定代码；在项目开发以后的任何时刻当中，`master`存放的代码应该是可作为产品供用户使用的代码。所以，应该随时保持`master`仓库代码的清洁和稳定，确保入库之前是通过完全测试和代码reivew的。`master`分支是所有分支中最不活跃的，大概每个月或每两个月更新一次，每一次`master`更新的时候都应该用git打上`tag`，说明你的产品有新版本发布了。

**develop**：开发分支，一开始从`master`分支中分离出来，用于开发者存放基本稳定代码。之前说过，每个开发者的仓库相当于源仓库的一个镜像，每个开发者自己的仓库上也有`master`和`develop`。开发者把功能做好以后，是存放到自己的`develop`中，当测试完以后，可以向管理者发起一个`pull request`，请求把自己仓库的`develop`分支合并到源仓库的`develop`中。

所有开发者开发好的功能会在源仓库的`develop`分支中进行汇总，当`develop`中的代码经过不断的测试，已经逐渐趋于稳定了，接近产品目标了。这时候，我们就可以把`develop`分支合并到`master`分支中，发布一个新版本。所以，一个产品不断完善和发布过程就正如下图：<br/>

![image](https://github.com/hujewelz/Git-Collaborative-development/blob/master/iamge/2.png) <br/>

注意，任何人不应该向`master`直接进行无意义的合并、提交操作。正常情况下，`master`只应该接受`develop`的合并，也就是说，`master`所有代码更新应该源于合并`develop`的代码。

####暂时性分支

暂时性分支和永久性分支不同，暂时性分支在开发过程中是一定会被删除的。所有暂时性分支，一般源于`develop`，最终也一定会回归合并到`develop`。

**feature**：功能性分支，是用于开发项目的功能的分支，是开发者主要战斗阵地。开发者在本地仓库从`develop`分支分出功能分支，在该分支上进行功能的开发，开发完成以后再合并到`develop`分支上，这时候功能性分支已经完成任务，可以删除。功能性分支的命名一般为`feature-*`，*为需要开发的功能的名称。 <br/>

![image](https://github.com/hujewelz/Git-Collaborative-development/blob/master/iamge/3.png) <br/>

假设我参与了一个项目的开发，已经把源仓库fork了，并且clone到了本地。现在要开发“讨论”功能。我在本地仓库中可以这样做：

step 1: 切换到`develop`分支 

	>> git checkout develop
	
step 2: 分出一个功能性分支 

	>> git checkout -b feature-discuss

step 3: 在功能性分支上进行开发工作，多次commit，测试以后...

step 4: 把做好的功能合并到`develop`中 

	>> git checkout develop <br/>
	# 回到develop分支
	
	>> git merge --no-ff feature-discuss 
	# 把做好的功能合并到develop中
	
	>> git branch -d feature-discuss 
	# 删除功能性分支
	
	>> git push origin develop 
	# 把develop提交到自己的远程仓库中
	
这样，就完成一次功能的开发和提交。

**release**：预发布分支，当产品即将发布的时候，要进行最后的调整和测试，这时候就可以分出一个预发布分支，进行最后的bug fix。测试完全以后，发布新版本，就可以把预发布分支删除。预发布分支一般命名为`release-*`。

**hotfix**：修复bug分支，当产品已经发布了，突然出现了重大的bug。这时候就要新建一个`hotfix`分支，继续紧急的bug修复工作，当bug修复完以后，把该分支合并到`master`和`develop`以后，就可以把该分支删除。修复bug分支命名一般为`hotfix-*`

`release`和`hotfix`分支离我们还比较遥远,我也没怎么用，就不详述，有兴趣的同学可以[点击这儿](http://www.ruanyifeng.com/blog/2012/07/git.html)进行学习。

##工作流（Workflow）

啰嗦讲了这么多，那么怎样进行协同开发呢？<br/>

###1.Fork项目
	进入要参与项目的地址：https://github.com/xudabin/test.git，点击Fork,先fork这个项目到自己的项目中。

###2.clone项目到本地
	2.1 把fork过来的项目clone到本地 <br/>
		git clone https://github.com/hujewelz/test.git
		
	2.2 可以新建并切换到开发(dev)分支
		git checkout -b dev
	然后可以在该分支下做开发。
###3. Pull request
	当实现了一个功能，就可以开始提交这个commit了。
	eg: git add test.md
		git commit -m 'add test.md'
		git push -u origin dev
	
	这时候，你进入github网站，进入compare & pull request，填写一些信息crete pull request即可。当项目发起者看到这个pull request时，就会合并到他的项目中了。

###4. 建立与主项目服务器的连接
	4.1 当主项目有更新了怎么办？那么，这个时候我们就需要将主项目的更新合并到本地。 
	git remote add upstream https://github.com/xudabin/test.git
	
	4.2 将更新合并到本地
	    git fetch upstream 
		git merge upstream/master
	或
		git pull upstream master:dev
	将远程仓库的master分支的更新合并到本地的dev分支（合并到哪个分支自己决定，不一定非得是dev分支哦，个人习惯而已）
	这个时候就将主项目的更新合并到本地了。然后就可以继续开发新功能了。



###感谢[戴嘉华](http://segmentfault.com/a/1190000002413519)

