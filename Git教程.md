<h1 style="text-align:center; font-family:Times New Roman; color:blue;">
 GIT<span style="font-family:SimSun;">教程</span>
</h1>
<h2 style="text-align:center; font-family:Times New Roman; font-size:20pt;">
  CWY
</h2>

## 一、初始化设置
1. **配置用户名**
   `git config --global user.name "Cai Weiye"`
   | local(省略) | --global | --system |
    |:-:|:-:|:-:|
    | 本地配置，只对本地仓库有效 | 全局配置，所有仓库生效 | 系统配置，对所有用户生效 |
2. **配置邮箱**
   `git config --global user.email 15005526538@163.com`
  <u>设置的值如果没有空格的话，双引号可以省略</u>
3. **存储配置**
   ```shell
   git config --global credential.helper store  #保存用户名和密码
   
   git config --global --list                   #查看git配置信息
   ```

## 二、创建仓库
- **仓库**(repository)：
  >简称**repo**，相当于一个目录，这个目录里所有的文件都可以被git管理起来，每个文件的修改、删除、添加等操作git都会跟踪到，以便任何时候都可以追踪历史或者还原到之前的某一个版本。
1. **创建仓库**
   ```shell
   mkdir learn-git
   cd learn-git

   git init my-repo        #在一个目录下创建仓库
   ```
   >创建完成后会在my-repo目录下生成.git文件夹，存放了git仓库所有数据

  - **在github里远程克隆仓库**
  `git clone https://xxx`

## 三、工作区域和文件状态
1. **工作区**(Working Directory)
   > 在电脑里能实际看到的目录。
2. **暂存区**(Stage/Index)
   > 暂存区也叫索引，用来临时存放未提交的内容, ⼀般在.git目录下的index中。
3. **本地仓库**(Repository)
   > Git在本地的版本库，仓库信息存储在.git这个隐藏目录中
4. **远程仓库**(Remote Repository)
   > 托管在远程服务器上的仓库，常用的有GitHub、GitLab、Gitee。
- *修改完工作区的文件之后，需要将他们添加到暂存区，然后再将暂存区的修改提交到本地仓库中*
5. **文件状态**
   1. **已修改**(Modified)
   > 修改了但是没有保存到暂存区的文件。
   2. **已暂存**(Staged)
   > 修改后已经保存到暂存区的文件。
   3. **已提交**(Committed)
   > 把暂存区的⽂件提交到本地仓库后的状态。
   
   | main/master | origin | HEAD | HEAD^/HEAD~ | HEAD~n |
   |:-:|:-:|:-:|:-:|:-:|
   | 默认主分支 | 默认远程仓库 | 最新提交节点 | 上一个版本 | 上n个版本 |
## 四、添加和提交文件
1. **查看仓库的状态**
    ```shell
   echo "this is first file" > file1.txt    #创建并编辑一个文件
   git status                               #查看该文件的状态
   git status -s  #-s(short)，简要查看状态，命令的回显有两个问号，第一列是暂存区的状态，第二列是工作区状态
   ```
2. **添加到暂存区**
   ```shell
   git add file1.txt                        #将文件添加到暂存区
   git status                               #此时可以看到文件已经被添加到了暂存区
   git add .                                #将所有文件添加到暂存区
   git add *.txt                            #将所有txt文件添加到暂存区
   git ls-files                             #工作区和暂存区被跟踪的文件
   git rm --cached file1.txt                #取消暂存
   ```
3. **提交**
   ```shell
   git commit -m "这是第一次提交"
   git commit -am "xxx"                     #用-am一次性完成添加和提交
   ```

   | -m |
   |:-|
   |后面指定提交的信息|

   ***git只会提交暂存区的文件***
    ```shell
    git log                                   #查看提交记录，版本id
    git log --oneline                         #查看简洁的提交记录
    ```
4. **回退版本**
   `git reset --<param> <回退的版本id>`  
   | soft | 回退到某一版本，保留工作区和暂存区的所有修改内容 |
   |:-:|:-:|
   |  **hard** | **丢弃工作区和暂存区所有修改内容** |
   | **mixed**(默认参数) | **只保留工作区修改内容，丢弃暂存区修改内容** |

   ```shell
   git reset HEAD^                  #默认使用mixed参数，HEAD^表示上一个版本

   git reflog                       #查看操作的历史记录
   git reset --hard <id>            #如果误操作，查看历史记录后在回退到该版本
   ```      
5. **查看状态或差异**      
   ```shell    
   git diff                         #不加参数，默认比较工作区和暂存区的差异
   git diff HEAD                    #比较工作区和版本库的差异
   git diff --cached                #暂存区和版本库的差异
   git diff <id_1> <id_2>           #比较两个版本的差异
   ```
6. **删除文件**
   ```shell
   git rm file.txt                  #从工作区和暂存区中删除文件
   git commit -m "删除了file.txt"    #删除之后提交，用于删除仓库中的文件

   git rm --cached file             #如果不想删除本地文件，加上--cached         
   ```
7. **忽略文件**
   | 特殊文件后缀名 | |
   |-|-|
   |.git| Git仓库的元数据和对象数据库 |
   |.gitignore|忽略文件，不需要提交到仓库的文件|
   |.gitatrributes|指向当前分支的指针|
   |.gitkeep|使空目录被提交到仓库|
   |.gitmodules|记录子模块的信息|
   |.gitconfig |记录仓库的配置信息|
   
   **?应该忽略哪些文件**
   - 系统或软件自动生成的文件
   - 编译产生的中间文件和结果文件
   - 运行时生成的日志文件、缓存文件、临时文件
   - 密码、身份等密码信息
  
   ```shell
   echo "ignore" > ignore.log       #创建日志文件
   echo ignore.log > .gitignore     #将日志文件加入到忽略文件

   vim .gitignore                   #修改.gitignore文件
   |—— *.log                        #添加该行，忽略所有日志文件
   |—— temp/                        #忽略temp下所有文件
   |—— ...
   ```
8. **撤销和删除文件**
   ```shell
   git mv <file> <new-file>         #移动⼀个文件到新的位置
   git checkout <file> <commit-id>  #恢复⼀个文件到之前的版本
   git revert <commit-id>           #创建⼀个新的提交，用来撤销指定的提交셈 后者的所有变化将被前者抵消，并且应用到当前分支。
   git restore --staged <file>      #撤销暂存区的文件，重新放回工作区(git add的反向操作)
   ```
## 五、SSH配置和远程仓库
   **创建仓库**：参考[bilibili教程](https://www.bilibili.com/video/BV1HM411377j?spm_id_from=333.788.videopod.sections&vd_source=7bbecb990f454aac6e3d6b28711d7595&p=11)
   
   **1. 配置SSH密钥**
   ```shell
   cd ~/.ssh                        #进入用户目录中的.ssh
   #如果没有.ssh目录，先创建
   mkdir -p ~/.ssh
   ssh-keygen -t rsa -b 4096        #生成SSH密钥，-t指定协议RSA，-b指定生成的大小为4096
   |——>回车，会在.ssh目录下生成id_rsa的密钥文件 .pub结尾的就是公钥文件
   gedit id_rsa.pub                 #打开公钥文件，复制里面的内容，添加到GitHub中(参考教程)
   ```
   **如果不是默认id_rsa文件，需要创建配置文件**
   ```shell
   ssh-keygen -t rsa -b 4096        #回车后输入新的文件名(比如test)
   |——>生成test和test.pub文件
   gedit test.pub                   #打开公钥文件，复制里面的内容，添加到GitHub中(参考教程)
   touch config                     #创建config文件
   vim config                       #添加内容如下五行内容
   # github
   Host github.com
   HostName github.com
   PreferredAuthentications publickey
   IdentityFile ~/.ssh/test
   #访问github时，指定使用SSH下的test密钥
   ```
   **2. 克隆仓库**
   ```shell
   git clone git@github.com:1271258153/Cwy.git
   #输入yes后，回车
   ```
   **3. 同步本地仓库和远程仓库**
   ```shell
   git push <远程仓库名> <远程分支名>:<本地分支名>   #将本地仓库的内容推送给远程仓库，相同可省略冒号后面部分
   git pull <远程仓库名> <远程分支名>:<本地分支名>   #将远程仓库的内容推送给本地仓库
   ```
   **4. 关联本地仓库和远程仓库**
   把现有本地仓库的内容传给新建的远程仓库
   ```shell
   git remote add origin https://github.com/1271258153/remote-repo.git  #本地仓库对应远程仓库的别名为origin
   git remote -v                    #查看当前仓库对应远程仓库的别名和地址
   git branch -M main               #指定本地分支的名称为main
   git push -u origin main          #把本地的main分支和远程仓库的origin分支关联起来
   |——>输入账号密码，密码需要在GitHub中选择
   |——>Settings → Developer settings → Personal access tokens
   |——>点击 "Generate new token"
   |——>设置令牌名称、有效期
   |——>选择权限（至少勾选 repo）
   |——>点击 "Generate token" 并立即复制令牌
      用户名：你的GitHub用户名（1271258153）
      密码：粘贴刚才复制的令牌
   ```

## 六、在VsCode中使用Git
   ```shell
   cd learn-git/my-repo
   code .                        #用vscode打开当前仓库
   ```
   **安装扩展**
   ```
   |——GitLens
   |——Git Graph
   |——Git Extension Pack
   ```
## 七、分支
   **1. 分支**(Branch)：
   >可以让多个开发人员在主分支上建立自己的分支，进行开发工作，最后再合并到主线代码库中
   ```shell
   mkdir branch-demo
   cd branch-demo
   git init
   
   echo main1 > main1.txt              #main分支的第一个文件
   echo main2 > main2.txt
   echo main3 > main3.txt
   git add .
   git commit -m "main:3"
   
   git branch                          #查看当前仓库的所有分支(默认main分支)
   git branch dev                      #创建一个dev分支
   git switch/checkout dev             #切换到dev分支(最好用switch)
   //git checkout -b <branch-name>     #创建并切换分支
   
   echo dev1 > dev1.txt                #在dev分支创建dev1.txt
   echo dev2 > dev2.txt
   git add .
   git commit -m "dev:2"

   git switch main                     #切换到main分支
   git merge dev                       #将dev分支合并到main分支
   git log --graph --oneline --decorate --all   #查看分支图

   git branch -d dev                   #手动删除不需要的分支(此分支未合并)
   git branch -D dev                   #强制删除分支
   ```
   **2. 解决合并冲突**
   >如果两个文件修改了同一个文件的同一行代码，就会产生冲突
   ```shell
   git branch feat                     #创建分支
   git switch feat
   echo "这是feat分支中添加的内容" >> main1.txt #向main1.txt文件添加内容
   git commit -a -m "feat:1"           #添加并提交

   git switch main                     #切换回main分支
   echo "这是main分支中添加的内容" >> main1.txt #向main1.txt文件添加内容
   git commit -am "main:1"             #添加并提交
   x git merge feat                    #产生冲突
   
   cat main1.txt                       #查看冲突的文件
   |——main1
      <<<<<<< HEAD
      这是main分支中添加的内容
      =======
      这是feat分支中添加的内容
      >>>>>>> feat

   vim main1.txt                       #手动编辑,留下想要的内容之后重新提交
   |——main1
      这是main分支中添加的内容,这是feat分支中添加的内容
      已合并

   git commit -am "merge conflict"     #保存后重新提交,自动合并
   #在提交前想中断本次合并
   git merge --abort
   ```
   **3. Rebase(变基)**
   >可以在任意分支上执行Rebase操作，合并分支
   ```shell
   git branch -d feat                  #删除feat分支
   git log --oneline                   #找到dev:2提交的id(4779450)
   git checkout -b dev 4779450         #恢复到dev:2这一时间点的状态

   git switch main                     #切换为main分支
   git reset --hard 278e358            #回退到合并之前提交的状态(main:4)

   cd ..
   cp -rf branch-demo rebase1
   cp -rf branch-demo rebase2

   cd rebase1           #第一个rebase命令需要切换到dev分支，变基到main
   git switch dev
   git rebase main      #合并之后，分支图变成一条直线，dev分支的提交记录都已经变基到main分支的最后面

   cd ../rebase2        #第二个rebase命令需要切换到main分支，变基到dev
   git switch main
   git rebase dev       #合并之后，分支图变成一条直线，dev分支的提交记录在main:3和main:4之间，因为dev提交时间在main3之后，在mian4之前
   ```
   **4. merge与rebase的区别**
   >**merge**
   >- 优点：不会破坏原分支的提交历史，方便回溯和查看
   >- 缺点：会产生额外的提交节点，分支图比较复杂

   >**rebase**
   >- 优点：不会新增额外的提交记录，形成直线分支图
   >- 缺点：会改变提交历史，改变了当前分支branch out的节点，避免在共享分支使用
