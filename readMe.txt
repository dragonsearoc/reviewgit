这是复习git知识的项目

2018/11/23

git是分布式版本控制工具
git是免费软件

需要版本回退

.git是git版本库
.git有暂存区stage、master分支、HEAD指针

Git跟踪修改，而不是文件
Git跟踪文件的修改。

撤销修改
	a)改乱工作区的内容，未git add到暂存区时（1.只修改工作区未add；2.修改工作区add到暂存区后未commit，再修改工作区）
		使用命令git checkout -- 文件名，文件将回到最近一次git add或git commit后的状态
	b)工作区的更改已git add到暂存区时
		使用命令git reset HEAD 文件名，暂存区的修改将丢弃，回到a)，继续a)中操作
	c)工作区的修改已git commit时（未pull到远程仓库）
		使用命令git reset --hard <HEAD^|版本标识>,进行版本回退
	（注：checkout --和checkout是两个命令，checkout“切换到另一个分支”）

删除文件
	步骤：
	1）rm 文件名
	2）确定要删掉版本库中文件，先git rm 文件名，这样删除改文件这一改动提交到了暂存区，然后git commit，版本库中该文件就被删除了
	3）误删、不想删了，使用命令git checkout -- 文件名，git会将版本库最新的该文件下到工作区中。

Git远程仓库（使用GitHub)
	1)在GitHub上创建一个账号，假定邮箱dotayeats@163.com
	2)使用ssh协议生产私钥、公钥，命令：ssh-keygen -t rsa -C "邮箱名"，默认在user目录下生成.ssh文件夹，有id_rsa.pub（公钥）、id_rsa（私钥）
	3)在GitHub的个人设置setting的ssh-keys配置项中增加公钥的配置（将id_rsa.pub内容拷贝到配置项即可）
	关联远程库：git remote add 远程库名字（一般origin） 远程库地址（如：https://github.com/dragonsearoc/reviewgit.git）
	关联后，使用命令git push -u origin master第一次推送master分支的所有内容
	此后，每次本地提交后，只要有必要，使用命令git push origin master推送最新修改

克隆远程仓库项目
	git clone 远程仓库项目地址（如：git@github.com:dragonsearoc/gitskills.git）
	注：Git支持多种协议,如http（例https://github.com/dragonsearoc/reviewgit.git），ssh（git@github.com:dragonsearoc/gitskills.git）
	ssh协议比http传输速率更快，而且http每次推送都需要口令，ssh不用。

Git分支管理
	Git通过HEAD指针指向当前分支指针（master或dev等分支指针），当前分支指针再指向当前分支的当前提交点，这样，HEAD指针就能确定当前的分支，已经当前分支的当前提交点。
	当git checkout -b 分支名（dev）创建dev分支时，dev指针与master分支指向同一提交点，HEAD指针原本指向master变为指向dev，此时工作区的修改都是针对dev分支的，工作区修改提交后，dev指针移到，但master指针不变，在dev分支的工作完成后，合并到master时，最简单的方式就是让master指针指向dev的当前提交（Git的Fast-forward合并模式就是如此）。
	合并完成后，dev分支就可以删除了，删除dev分支实际就是删除dev指针。
	相关命令小结
		git branch	//查看分支 有“*”标记的是当前工作分支
		git branch 分支名称 //创建分支
		git checkout 分支名称 //切换分支
		git checkout -b 分支名称 //创建+切换分支
		git merge 分支名称 //合并指定分支（分支名称）到当前分支
		git branch -d 分支名称 //删除分支
Git解决冲突
	当前分支与合并分支存在冲突时（如都对同一文件做了不同的修改），git merge 分支名称将不能正确合并分支，需要解决冲突后再将冲突文件提交。
	打开冲突文件，看到Git用<<<<<<<，=======，>>>>>>>标记出不同分支的内容
	查看分支合并的情况
		git log --graph --pretty=oneline --abbrev-commit。

Git分支管理策略
	Git合并分支时通常会优先尝试使用fast-forward模式，但这种模式下，删除分支后，git会丢失分支信息。
	强制禁用fast-forward模式，git在merge时会生成一个新的commit，这样就可以通过分支历史看出分支信息。
	禁用fast-forward模式的参数--no-ff

分支策略：
	master分支为主干，稳定版本，不作为开发分支使用
	dev分支为开发分支，团队成员有各自的dev副本，各自在副本上开发，提交，工作完成后，，将各自分支都合并到dev分支，确认无误后合并到master，发布

Git的Bug分支
	工作中需要解决bug（如代号101的bug）时，在目标分支（如master)上创建bug分支（如git checkout -b isure101），解决后git add，git commit，最后git checkout 目标分支（master），git merge bug分支（如isure101）。
	实际情况很可能这样，在dev分支开发某需求到中途时，突然需要紧急解决bug（如代号101），由于当前工作区的内容由于某些原因（如代码不完善，代码不能工作）不能提交到dev分支，
	这时如果不保存当前工作区的内容，直接创建bug分支，开发到中途的代码就将丢失掉，严重影响工作，因此git提供gitstash命令来保存当前的工作区，使我们解决完bug后可以轻松继
	续之前未完成的工作。
	命令git stash list将罗列保存的工作区，还原方式有两个：
		1.git stash apply 工作区标识名，还原工作区，但stash不会被删除，需要使用git stash drop 工作区标识名 删除
		2.git stash pop还原工作区并删除

Git的feature分支
	用于开发新功能、实验性质的分支，为了不搞乱已有的代码，建议使用feature分支开发，完成后，合并，删除该feature分支。
	强制Git删除未合并过的分支，使用命令 git branch -D 分支名称。

Git多人协作
	工作模式：
	1.首先，试图git push origin 分支名称 推送自己的修改
	2.若推送成功，结束；反之，可能由于远程仓库已有了他人的推送内容，推送失败，此时应该git pull拉取远程仓库最新的改动
	3.a）由于本地分支未与远程分支关联而拉取失败，使用命令【git branch --set-upstream-to 分支名称 origin/远程分支名称】关联，之后再【git pull】
	  b）拉取后与本地改动冲突，解决冲突，然后本地提交
	4.冲突解决或没有冲突，使用命令【git push origin 分支名称】推送即可。
	补充：
	1.查看远程库信息，命令【git remote -v】
	2.本地新建的分支，如果不推送到远程，其他人是看不到的。
	3.在本地创建和远程对应的分支，命令【git checkout -b 分支名称 origin/远程分支名称】，本地和远程分支名称最好一致。

rebase
	1.rebase操作可以把本地为push的分叉提交历史整理成直线
	2.rebase操作的目的是使我们查看历史提交的变化时更加容易，因为分叉的提交需要三分比对。

Git标签管理
	标签是版本库的快照（发布版本前通常会先打个标签），实际上就是一个指向某个commit的指针，很像分支，区别是分支可以移动，标签不能。因此创建和删除标签都是瞬间完成的操作。
	标签tag相当于commit id的别名，更方便使用。
	创建标签
		1.切换到需要打标签的分支，使用命令【git tag 标签名字（如v1.0）】即可创建标签，
		2.默认标签是打在最新的commit上,使用命令【git tag 标签名字（如v0.9）提交id】，可在指定的commit id上创建标签。
		3.创建带说明的标签，使用命令【git tag -a 标签名字（如v1.1） -m "说明"】
	查看标签列表
		git tag 排序是按标签名字的字母顺序升序的，不按时间顺序
	查看标签信息
		git show 标签名字
	注意：标签总是和某个commit挂钩。如果这个commit id在master分支和dev分支都出现，那这个标签在这两个分支都能看到

Git操作标签
	标签可以删除。创建在本地的标签不会自动推送到本地，可以轻松删除，推送到远程仓库的标签删除则麻烦下
	1.推送标签到远程，使用命令【git push origin 标签名称（如v1.0）】，或一次推送所有未推送的标签【git push origin --tags】
	2.删除本地标签，使用命令【git tag -d 标签名称（如v1.0）】
	3.删除已推送的标签，首先删除使用命令【git tag -d 标签名称（如v0.9）】删除本地标签，再使用命令【git push origin :refs/tags/标签名称（如v0.9）】
	登录github查看远程标签是否删除。

使用GitHub
	小结：
	1.在GitHub上，可以任意Fork开源仓库
	2.自己拥有对Fork下来的仓库的读写权限
	3.可以推送pull request给官方仓库来贡献代码。

Git自定义配置
	1.命令【git config --global color.ui true】，让Git显示颜色，使命令输出更舒服。
	2.忽略特殊文件
		在git工作区下创建一个.gitignore文件，然后把要忽略的文件名填进去，Git提交时就会忽略那些文件。
		在https://github.com/github/gitignore上有许多预设好的定义，按需组合下加到.gitignore即可。
		忽略文件的原则；
			a)忽略操作系统自动生成的文件，如缩略图等；
			b)忽略编译生成的中间文件、可执行文件等，也就是通过另一个文件自动生成的，如Java编译生成的类文件.class。
			c)忽略带有敏感信息的配置文件，如存放口令的配置文件。
		强制Git提交忽略的文件，使用命令【git add -f 文件名】
		查看.gitignore中那条规则忽略了文件，使用命令【git check-ignore -v 文件名】。
		.gitignore文件要放到版本库里才能有效，并且可以对.gitignore做版本管理。
	3.配置别名
		a)给命令配置别名，方便偷懒，git config --global alias.别名（如st） 原命令名（如status）
			常用的命令别名，co->checkout， ci->commit， br->branch，
			撤销修改中，git reset HEAD 文件名将暂存区的修改撤销掉，重新回到工作区，通过git config --global alias.unstage reset HEAD，git unstage达到同样效果
			配置git last命令，让git显示最后一次的提交信息，git config --global alias.last 'log -1'。
			配置git lg按格式显示日志，git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"
	4.配置文件
	 --global是对当前用户的，不加则只对当前仓库有效
	 当前仓库的配置文件放在.git/config文件中，cat .git/config即可查看。
	 别名在[alias]后，要删除某个别名，删除对应行即可。
	 当前用户的git配置文件放在用户主目录下的一个隐藏文件.gitconfig文件中