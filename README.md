一、配置

git config --global user.name "账户名称"

git config --global user.email "邮箱"

tips：检查当前文件夹是否归git管理：

在当前文件夹打开终端输入git status指令 返回 fatal：not a repository （or any of the parent directories）： .git 则表示这不是一个git仓库 所有不归git管理 

二、使用git

    git status
        - 查看当前仓库的状态
    git init
        - 初始化仓库

    刚刚添加到项目中的文件处于未跟踪状态：
        未跟踪  ---> 暂存
            git add <filename> 将文件切换到暂存的状态
            git add * 把当前项目中所有新增或修改的文件变为暂存状态（包括未跟踪状态的文件）
            git add . 把当前项目中所有新增或修改的文件变为暂存状态（包括未跟踪状态的文件）
        暂存 ---> 未修改
            git commit -m "提交信息" 将暂存的文件存储到仓库中(本地仓库中)
            提示信息一般在公司项目中以下面三种形式居多
            feat:新增文件
            bugfix:修改bug
            style:添加样式
            git commit -a -m "xxxx" 提交所有已修改的文件(未跟踪的文件不会提交)
        未修改 ---> 修改
            修改代码后，文件会变为修改状态
    git log 
        - 查看提交的commit记录(查看日志)

三、常用的命令

    1.重置文件（移除暂存区）（回退修改文件到之前版本）
    git restore <filename> 恢复文件
    git restore --staged <filename> 取消暂存状态

    2.删除文件
    git rm <filename> 删除
    git rm <filename> -f 强制删除

    3.移动文件
    git mv from to 移动文件 重命名文件
    案例：git mv 1.txt 2.txt 文件1名称变为2

四、分支

    git在存储文件时，每一次代码的提交都会创建一个与之对应的节点，git就是通过一个一个的节点来记录代码的状态的。
    节点会构成一个树状结构，树状结构就意味着这个数会存在分支，默认情况下仓库只有一个分支，命名为master。
    在使用git时可以创建多个分支，分支与分支之间相互独立，在一个分支上修改代码不会影响到其他的分支。

    git branch # 查看当前分支

    git branch <branch name> # 创建新的分支

    git branch -d <branch name> # 删除分支

    git switch <branch name> # 切换分支

    git switch -c <branch name> # 创建并切换分支

    git checkout <branch name> # 切换分支 （老版本git功能 功能相对于switch更多更全一点）

    git checkout -b <branch name> # 创建并切换分支

    在开发中，我们都是在自己的分支上编写代码，代码编写完成后，再将自己的分支合并到主分支中

    git merge <branch name> # 合并代码 把当前merge的代码合并到当前所在分支

    git remote add origin url # 连接远程仓库 origin 库名（服务器） url 仓库地址

    git branch -M main # 修改当前分支名称为main

    git push -u origin main # 将本地代码推送到指定的远程仓库 origin 库名 mian对应分支名

    git clone https://gitee.com/zhaomenghao/git-demo.git # 将远程仓库代码下载到本地

五、变基(rebase)

    在开发中除了通过merge来合并分支外，还可以通过变基来完成分支的合并

    我们通过merge合并分支时，在提交记录中会将所有的分支创建和分支合并的过程全部都显示出来，
    这样当项目比较复杂，开发过程比较波折时，我们必须要反复创建、合并、删除分支。这样一来将会使得我们代码的提交记录变得极为混乱。

    原理（变基时发生了什么）：
        
        1.当我们发起变基时，git会首先找到两条分支的最近的共同祖先

        2.对比当前分支相对于祖先的历史提交，找到它们的不同并且将它们提取出来存储到一个临时文件中

        3.将当前部分执行目标的基底（目标基底就是目标分支）

        4.以当前基底开始，重新执行历史操作
    
    变基和merge对于合并分支来说最终的结果是一样的！但是变基会使得代码的提交记录更整洁更清晰！
    注意！大部分情况下合并和编辑是可以互换的，但是如果分支已经提交给了远程仓库，那么这时尽量不要使用变基。

六、远程仓库(remote)

    目前我们对于git的所有操作都是在本地进行的。在开发中显然不能这样的，这是我们需要一个远程的git仓库。
    远程的git仓库和本地的本质没有什么区别，不同点在于远程的仓库可以被多人同时访问使用，方便我们协同开发。
    在实际工作中，git的服务器通常由公司搭建内部使用或是购买一些公共的私有git服务器。学习阶段，直接使用一些
    开放的公共git仓库。目前我们常用的库有两个：GitHub和Gitee(码云)

    将本地库上传git：
    
    # git remote add <remote name>(远程库的名字) <url>(远程库的地址url)
    git remote add origin https://github.com/lilichao/git-demo.git

    # 修改分支名字为main
    git branch -M main

    # git push 将我们的代码上传到服务器上 origin 远程库的名字 main 本地分支的名字
    git push -u origin main

    将本地库上传gitee:
    
    创建git仓库：
    mkdir git-demo
    cd git-demo
    git init 
    touch README.md
    git add README.md
    git commit -m "first commit"
    git remote add origin https://gitee.com/zhaomenghao/git-demo.git
    git push -u origin "master"

    已有git仓库
    git remote add origin https://gitee.com/zhaomenghao/git-demo.git
    git push -u origin "master"

    如果需要同时关联两个仓库需要配置一下两个仓库的库名(origin)，
    同时最好实现两个仓库的分支为同一个名称建议（mian）为分支名称

    第一次使用git push -u origin master/main 时会让输入账号密码
    如果使用的是GitHub就输入GitHub的账号密码如果使用的是gitee就输入gitee的账号密码
    输入后点击continue就会把本地仓库推到远程仓库没有Error等报错信息就是已经成功

    如何删除仓库：

    github 在setting里面管理 左侧tab找到删除仓库点击会弹出弹窗，按照提示填写要删除的仓库路径，输入密码确认即可

    gitee 找到管理按钮点击后 左侧tab找到删除仓库点击会弹出弹窗，按照提示填写要删除的仓库路径，输入密码确认即可

七、远程库的操作命令

    git remote # 列出当前关联的所有远程库

    git remote add <远程库名> <url> # 管理远程仓库 

    git remote remove <远程库名> # 删除远程库

    git remote -v # 查看远程库的上传和下载地址 fetch 下载 push 上传
    origin  https://gitee.com/zhaomenghao/git-demo.git (fetch)
    origin  https://gitee.com/zhaomenghao/git-demo.git (push)

    git push -u <远程库名> <分支名> # 向远程库推送代码，并和当前分支关联 -u 表示和当前分支关联

    git clone <url> # 从远程仓库下载代码

    git clone <url> <name> # 从远程仓库下载代码并设置当前代码的文件名

    git push # 如果本地的版本低于远程库，push默认是推步上去 （版本就是代码）

    git push <远程库> <本地分支>:<远程分支> # 把本地分支推送到远程指定分支 如果远程分支不存在会自动创建该远程分支

    git fetch # 要想推送成功，必须先确保本地库和远程库的版本一致，fetch它会从远程仓库下载所以代码，但是它不会将代码和我们当前分支自动合并
              # 使用fetch拉去代码后，必须要手动对代码进行合并

    git pull # 从服务器上拉去代码并自动合并 想当于 git fetch + git merge

    注意：推送代码之前，一定要从远程库中拉去最新代码

八、标签(Tag)
    - 如何出现分离头指针
        - git switch commitId # 会以当前commitId创建一个分支 但同时出现分离头指针的问题
    - 当头指针(HEAD)没有指向某个分支的头部时，这种状态我们称为分离头指针（HEAD detached）,
    在分离头指针的状态下也可以操作代码，但是这些操作不会出现在任何分支上，所以注意不要在分离头指针的状态下来操作仓库。
        - 解决分离头指针状态的方法就是切换到正常分支
    - 如果非得要回到后边的节点对代码进行操作，则可以选择创建分支后再操作
        - git switch -c <分支名> <commitId>
    - 可以为提交记录设置标签，设置标签以后，可以通过标签快速的识别出不同的开发节点：
        - git tag  # 输出标签
        - git tag 版本号 # 设置标签 版本就是类似commit的描述方便查找 建议以版本号形式设置
        - git tag 版本号 commitId # 给指定commit打上标签
        - 有了标签以后，我们想以之前的commitId节点创建新的分支就可以直接通过标签快速定位创建新的分支
            - git switch -c <分支名> <标签名>
        - 把标签推送到远程服务器
            - git push <远程库> <标签名> # 把指定标签推到远程库
            - git push <远程库> --tags # 把所有标签推送到远程仓库
            - git tag -d <标签名> # 删除本地标签
            - git push <远程库> --delete <标签名> # 删除远程仓库标签

九、gitignore(忽略)

    - 默认情况下，git会监视项目中所有内容，但是有些内容比如node_modules目录中的内容，我们不希望它被git所管理，
        我们可以在项目目录中添加一个 .gitignore 文件，来设置那些需要git忽略的文件。
    - 在gitignore文件中一行就代表一个文件或一个文件夹，所以不能多个文件写在一行，如果填写的是文件名则忽略文件内的所有文件(内容),
        如果填写的是指定文件则只忽略该文件，如果填写的是*.xxx则忽略以xxx结尾的所有文件
    - 在 .gitignore 文件中注释要以 # 形式 #后面的所有内容则会被注释
        - node_modules # 忽略node_modules文件下所有文件
        - yarn.lock # 忽略yarn.lock文件
        - *.log # 忽略以.log结尾的所有文件

十、github的静态页面

    - 在github中，可以将我们自己的静态页面直接部署到github中，它会给我们提供一个地址使得我们的页面变成一个真正的网站，可以供用户访问。
    - 要求：
        - 静态页面的分支必须叫做：gh-pages
        - 该网站默认访问地址是xxx.github.io/仓库名
        - 如果希望页面可以通过xxx(用户名).github.io访问则需要将库的名字配置成xxx.github.io 

十一、docusaurus

    - facebook推出的开源的静态的内容管理系统，通过它可以快速的部署一个静态网站。【比较契合github部署静态网站的特性】
    - 使用：
        - 网址:
            - https://docusaurus.io/
        - 安装
            - npx create-docusaurus@latest my-website classic
        - 启动项目
            - npm start 或 yarn start
        - 构建项目
            - npm run build 或 yarn build
        - 配置项目
            - docusaurus.config.js 项目的整体配置文件
        - 部署项目
            - yarn deploy 
            - 注意：如果出现分支报错提示可能是配置文件中没有配置要部署的分支名，在配置文件中配置 deploymentBranch 属性即可
                - deploymentBranch:"gh-pages" 
            - 上传代码时，两个注意事项
                - deploymentBranch来指定分支！
                - 还需要配置一个环境变量：GIT_USER（变量名）: github的用户名
        - 添加页面：
            - 在docusaurus框架中，页面分为三种：1.page 2.blog 3.doc