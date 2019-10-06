# Git

### 基本命令

1. 初始化空git仓库

   ```bash
   mkdir mygit #创建一个空目录
   cd mygit
   git init  # 将mygit创建成一个仓库，里面会出现.git文件夹
   ```

2. 设置仓库的基本信息

   ```bash
   git config --global user.name "RUI"  #全局设置用户名和email  
   git config --global user.email "cmi.sss@gmail.com"   # 该方法会改变～/.gitconfig中的全局配置
   git config --local user.name "CHEN"  #只改变本repo中的信息，即.git/config 文件
   
   git config -e --global  # -e 参数可以直接打开编辑器来编辑对应的文件
   ```

   你也可以通过直接修改这两个文件``~/.gitconfig``和``./.git/config``达到同样效果

   ```bash
     1 [user]
     2     name = lalala
     3     email = lalala@gmail.com
   ```

3. 添加/删除文件到待命区(staging area)

   - ```bash
     git status # 来查看当前repo的文件状态
     # 通过命令git add 将所有未追踪untracked file的文件添加到待命区
     git add untracked.txt. # 添加文件 untracked.txt 到待命区
     git add . # 添加所有未追踪文件到待命区
     ```

     ```bash
     # 撤销某次add
     git rm --cached tracked.txt # 从待命区删除已经被缓存的文件 tracked.txt
     # 或者
     git reset tracked.txt
     ```

4. 提交变动commit

   ```bash
   git commit -m "new commit" -a # 如果待命区staging area中的内容发生变化。则需要提交来使得变化真正被保存/序列化
   # -m 是必加参数，用于对提交标注释
   # -a 参数可以省略git add 步骤，直接添加并提交
   
   git add forgetfile.txt # 添加遗忘的文件
   git commit --amend # 将遗忘的文件重新提交到上次的commit中。不产生新的commit。
   
   git commit --date 21.10.2019 # 指定提交日期，可以指定明天（未来），也可以修改时期为过去，但是该commit仍排栈顶
   ```

5.  回复文件checkout

   ```bash
   git checkout filename # 如果filename被修改了，用checkout就把文件恢复到最近一次commit的状态
   ```
   
6. 下载远程的repo到本地

   ```bash 
   git clone https://github.com/Gazler/cloneme #下载远程url的repo到本地，会创建cloneme文件夹（repo）
   git clone https://github.com/Gazler/cloneme my_cloned_repo #下载远程repo到本地，并存于my_cloned_repo文件夹内
   ```


6. 忽略文件

   ```bash
   # 在～/.gitignore(全局) 或 ./.gitignore(local)中添加要被忽略的文件
   .gitignore
   *.swp    # 每行一条pattern，忽略所有.swp结尾的文件变动
   !a.swp   # 不忽略a.swp文件
   ```

7. 暂存变化stash

   ```bash
   # 如果你想临时退出当前的变更，但又不想将现有未完成的变更提交，可以用stash命令
   git stash # 将当前的 untracked 和 tracked 但是未commit的文件压栈。这样你就可以切换分支了
   git stash list # 列出所有暂存的保存变更
   git stash pop/push # 压栈或者弹栈
   git stash apply # 应用某次记录点
   ```

8. git 移动文件

   ```bash
   # 重命名或移动
   git mv a.txt ./src/b.txt
   ```

9. 查看git 日志

   ```bash
   git log
   ```

10. 对提交添加标签

    ```bash
    git tag new_tag # 对最新提交添加标签 new_tag
    git push --tags # 将新标签提交到远程repo
    ```

11. 远程remote repo

    ```bash
    git remote show # 显示远程关联的repo名，可能有多个。 通常远端repo都叫 origin
    git remote -v # 查看远端repo详细信息
    
    git pull origin master # 将远端 origin repo 拉到本地 master 分支上
    
    git remote add origin https://github.com/githug/githug # 添加远程repo origin 和其URL
    ```

    

### Git原理

Git 底层用``SHA-1``来进行校验。若是文件发生变动或者传输发生错误，文件系统的``SHA-1``值就会发生变动。``Git正是通过侦测SHA-1值的变动来检测 repo 的变动``

#### 远程Git逻辑图
<div>
  <img src="./images/90e14b750d27677866b4e34e4f52fd4a.jpg" height=200>
</div>

