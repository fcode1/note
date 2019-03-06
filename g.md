git init   初始化本地仓库
git config --list  查看当前配置信息
git config --global user.name '****'
git config --global user.email '****'


git remote add origin '地址'
$ git clone ‘地址’

git branch name 创建新分支
git checkout name 进入分支
git branch -d name 删除分支

git tag v1.0 打标签版本

----------------------------------------------------------

git remote 查看源
mkdir name 创建文件夹

notepad name.txt 调用记事本创建文件 


git status 查看状态
git add .全部添加
git commit  -m 

git push origin master 将文件给推到服务器上 
------------------------------------------------------------------
版本回退：
当提交了几个commit，假设我们现在有3个版本(1,2,3)，现在是版本3，发现刚刚的提交错误了，想撤回回到版本2

$ git reset --hard //重置暂存区与工作区，与上一次commit保持一致

然后发现刚刚的提交是正确的,又想回到版本3，再输入下面这个命令，相当于你那个回退没有做

$ git reset --hard [commitid] //重置当前分支的HEAD为指定commit，同时重置暂存区和工作区，与指定commit一致
//commitid 使用git log --stat查看


场景1：当改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令

$ git checkout -- file

场景2：当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步，第一步用命令

$ git reset HEAD file

，就回到了场景1，第二步按场景1操作。

场景3：已经提交了不合适的修改到版本库时，想要撤销本次提交，git reset --hard,不过前提是没有推送到远程仓库。

详细内容:https://www.toutiao.com/a6636198873578603016/?tt_from=mobile_qq&utm_campaign=client_share&timestamp=1545269506&app=news_article&utm_source=mobile_qq&iid=52671471192&utm_medium=toutiao_ios&group_id=6636198873578603016