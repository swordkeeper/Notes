# Git

#### 基本命令

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
     git rm --cached tracked.txt # 从待命区删除已经被缓存的文件 tracked.txt
     ```

4. 提交变动commit

   ```bash
   git commit -m "new commit" # 如果待命区staging area中的内容发生变化。则需要提交来使得变化真正被保存/序列化
   # -m 是必加参数，用于对提交标注释
   ```

5. 下载远程的repo到本地

   ```bash 
   git clone https://github.com/Gazler/cloneme #下载远程url的repo到本地，会创建cloneme文件夹（repo）
   git clone https://github.com/Gazler/cloneme my_cloned_repo #下载远程repo到本地，并存于my_cloned_repo文件夹内
   ```

   

