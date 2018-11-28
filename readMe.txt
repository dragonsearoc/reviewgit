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



