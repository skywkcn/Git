Git可以不联网进行版本控制，而此时又可以进行 多分支操作；而当将本地库上传到远程库时，可以多人协作，所以要特别注意你所进行的操作是仅在本地库的操作，还是设计与远程库联系的操作
# 将代码上传到GitHub的大致流程
1. 在GitHub上创建仓库（对于本地库中已经有的项目，在创建时如果创建readme文件和.gitignore文件pull过程中可能有错误）
2. git init
3. 设置忽略文件
4. git add
5. git commit
6. 本地和远程库建立联系：git remote add origin SSH  （这里的origin是名字，每个仓库的名字不一定都是origin，可以使用git remote -v查看仓库名字，下同)
7. Pull一下：git pull origin master
8. 上传代码到github远程库中：git push <远程库名称> <分支名称>；例如：git push origin master
	- 除了上述命令还可以使用：git push -u origin master
# 本地库操作
## 用户信息
- 添加
	- Git config --global user.name "yourname"
	- Git config --global user.email "your@email.com"
- 修改
	- 覆盖的形式：
		- Git config --global user.name "yourname"
		- Git config --global user.email "your@email.com"
	- 替换的形式：
		- Git config --global --replace-all user.name "yourname"
		- Git config --global --replace-all user.email "your@email.com"
- 删除
	- Git config --global --unset user.name "yourname"
	- Git config --global --unset user.email "your@email.com"
- 查看
	- 查看所有：Git config --list
	- 查看指定信息：
		- Git config user.name
		- Git config user.email
## Git基础
1. 初始化（初始化需要在对应文件夹目录下进行）：**Git init**；该命令将创建一个名为.git的子目录，这个子目录含有你初始化的Git仓库中所有的必须文件，这些文件是Git仓库的骨干。
2. 查看工作区、暂存区的状态：**Git status**
3. 设置忽略文件
	- github官方提供的忽略文件：[官方gitignore](https://github.com/github/gitignore);（GitHub中Python的忽略原文件中没有忽略.idea文件夹）
	-  [Git入门篇之忽略文件](https://zhuanlan.zhihu.com/p/60752662);
	- 在git最初开始，如果.gitignore和需要忽略的文件一期add和commit，可能导致忽略失败。则可以采用以下任意两种方法：
		1. 先add和commit  .gitignore文件，在git add .所有文件
		2. 将需要提交的文件和.gitignore文件（不包括需要忽略的文件）一起add和commit
	- 利用vim创建".gitignore"
		1. 命令：vim .gitignore（在.gitignore存在的情况下，该命令为打开该文件）
		2. 注意gitignore前面有个点号，不能少
		3. 也可以用touch .gitignore创建该文件
	- 将需要忽略的文件添加到.gitignore中
	- 提交.gitignore
4. 将工作区的修改添加到暂存区中：**Git add**
	- 添加所有文件"git add ."（注意后面的点）
	- 提交特定格式的文件"git add *.py"
	- 提交文件夹："git add 文件夹/"（注意文件夹名后面的斜杠）
5. 将暂存区的文件提交到本地仓库
	- 记述一行提交信息：**git commit -m "First commit"**（-m参数后的"First commit"称作提交信息，是对这个提交的概述）
	- 记述详细提交信息：**git commit**（之后编辑器会启动，以下以VIM作为编辑器进行说明）
		1. 提交git commit命令后进入页面
		2. 按Esc进入正常页面
		3. 按i进入插入页面，进行编辑信息
		4. 如果上一步中进行了汉字输入一定要将输入法换为英文模式，按Esc进入正常页面
		5. 输入":wq"保存并退出
		- 在编辑器中记述提交信息的格式：
			- 第一行：用一行文字简述提交的更改内容
			- 第二行：空行
			- 第三行：记述更改的原因和详细内容
	- 跳过暂存区进行提交：**git commit -a**
		- 例如：git commit -a -m "First commit"
		- 该命令会自动把所有已经跟踪过的文件暂存起来一并提交，从而跳过 git add 步骤；但是要小心，有时这个选项会将不需要的文件添加到提交中
6. 修改已经提交过的commit（分两种情况）
	- 修改上一次的commit：git commit --amend（之后进入vim中，正常编辑保存即可）
		- 修改历史commit
			1. 查看提交历史(git log或者git log --oneline)
			2. 编辑提交历史git rebase -i <父级hash>（如果需要修改多个commit，那么此处的head就应该是时间最久远的commit之前一个的hash）；此步也可以使用命令git rebase -i HEAD~n（此处的n表示倒数第几次）
			3. 将需要修改的commit前的pick修改为reword（简写为r），然后:wq保存
				- 如果有多个commit需要修改，那么此处应该将所有需要修改的commit前的pick都修改为reword（或r）
				- 此处的修改是在vim中修改的需要用到vim命令
			4. 修改commit，然后:wq保存，之后进入下一条需要编辑的commit进行修改；全部修改完成后，保存后会出现修改信息，表示修改成功；可以使用git log --oneline查看
			5. 如果需要修改的commit已经提交到远程库中，那么最后需要push到远程库，此时需要在push时，加上--force表示强制覆盖远程库上的提交信息：git push --force origin master
	- [Git修改已经提交的commit内容](https://cloud.tencent.com/developer/article/1335804)
	- [Git修改已提交的commit注释](https://www.jianshu.com/p/098d85a58bf1)
7. 查看日志：**git log**
	- 只显示提交信息的第一行：git log --pretty=short
	- 只显示指定目录、文件的日志：git log命令后加上目录名(git log readme.md)
	- 显示文件的改动：git log -p (git log -p readme.md)
	- 查看更改前后的差别：git diff命令可以查看工作数、暂存区、最新提交之间的差别
	- 显示精简的日志信息：git log --oneline
8. 显示工作区和暂存区文件：git ls-files
9. 删除文件：git rm <文件> （**此操作本地文件也会被删除**）
10. 删除文件：git rm --cached <文件> （**此操作只删除远程库中的文件，不会删除本地文件**）
	- 删除文件夹：git rm -r --cached <文件夹>
	- 根据文件在暂存区、工作区等状态不同，会有不同的操作
11. 删除本地库：删除仓库文件夹下隐藏的.git文件夹即可
	- [Git如何删除本地仓库](https://blog.csdn.net/bloombud/article/details/80431557);
	- 判断是否删除成功，在本地仓文件下面，右键选择Git bash here，如果命令行末尾没有(master)，即删除成功
12. 移动或重命名文件、目录或符号链接：**git-mv**
	- git mv [options]  [source]  [destination]：将soucre重命名为destination（source必须存在）;
	- git mv [options]  [source]  [destination directory]：destination directory必须为现有目录，将sourec移动到destination directory中;
		注意：使用git mv命令修改完文件后，必须使用git commit进行提价
		[Git - git-mv Documentation](https://git-scm.com/docs/git-mv/en);
# Github
star、watch、fork的区别
- star 的作用是收藏，目的是方便以后查找
- watch 的作用是关注，目的是等作者更新的时候，你可以收到通知
- fork 的作用是参与，目的是你增加新的内容，然后 Pull Request，把你的修改和主仓库原来的内容合并