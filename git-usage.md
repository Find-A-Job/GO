dir:go/games/ppc/ppebg/b/log/server_debug.log
echo "$(< go/games/ppc/ppebg/c/log/server_debug.log)"
echo "$(< go/games/brc/brpj/a/log/server_debug.log)"
git add .
git commit -m "二八杠-调整比牌函数compareCards(3),二八杠-调整椅子号"
git pull
git push origin master

git push origin master:master //推送本地仓库的master分支到远程仓库origin的master分支
git push origin dev:dev  //同上
git push origin :dev //删除远程仓库origin的dev分支
--
git branch //查看本地所有分支
git branch -d xxx //删除某个分支
git checkout xxx //切换到某个分支
--
git reset HEAD xxx //取消已暂存的某个文件(从本地仓库还原暂存区的某个文件，工作区代码不变

)
git checkout -- xxx //从暂存区还原工作区的某个文件
git checkout head xxx //从本地仓库还原工作区的某个文件，暂存区不变
git reset --hard HEAD^  //从本地仓库还原工作区的文件到上一版本
git reset --hard HEAD^^  //从本地仓库还原工作区的文件到上上版本
git reset --hard HEAD~10 //从本地仓库还原工作区的文件到前10个版本
git reset --hard d362816 //从本地仓库还原工作区的文件到指定版本
--
git stash save xxx //保存当前"暂存区"和"未暂存但已追踪且被修改过的文件"到栈中，并以xxx命

名
--
当我们在一个过时的分支上面开发的时候，执行 rebase 以此同步 master 分支最新变动
git rebase xxx //将xxx分支的最新变动同步到当前分支
--
git diff <file>比较工作区文件和暂存区文件差异,--即下一次add时xxx文件会被添加的内容
git diff <hash id> <hash id2> 比较两次提交之间的差异
git diff <branch> <branch1> 比较两个分支之间的差异
git diff --staged 比较暂存区和本地仓库之间的差异
git diff --cached 
git
git diff <file>              # 比较当前文件和暂存区文件差异 git diff


git diff <id1><id1><id2>     # 比较两次提交之间的差异


git diff <branch1> <branch2> # 在两个分支之间比较

git diff --staged            # 比较暂存区和版本库差异


git diff --cached            # 比较暂存区和版本库差异


git diff --stat              # 仅仅比较统计信息


git clean -f     # 删除 untracked files
git clean -fd    # 连 untracked 的目录也一起删掉
git clean -xfd   # 连 gitignore 的untrack 文件/目录也一起删掉 （慎用，一般这个是用来删掉

编译出来的 .o之类的文件用的）
# 在用上述 git clean 前，墙裂建议加上 -n 参数来先看看会删掉哪些文件，防止重要文件被误删
git clean -nxfd
git clean -nf
git clean -nfd

git rebase master

# merge
git checkout --ours word.txt    # => chocolate下
git checkout --theirs word.txt  # => boycott上

# rebase
git checkout --ours word.txt    # => boycott上
git checkout --theirs word.txt  # => chocolate下

git add .
git rebase --continue

git reflog   #查看本地记录
git reset --hard xxxxxx #将本地仓库还原到xxx版本,工作区的更改也会被撤销
git reset --soft xxxxxx #参数soft指的是：保留当前工作区，以便重新提交

git commit --amend -m "xxx"  # 修改最近一次的commit message

:set number # vim显示行号
