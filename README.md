"# JackyTech.github.io" 

---
title: "搭建博客配置过程中的问题及解决方法"
date: 2022-06-27T23:40:00+08:00
draft: false
---
搭建博客前提：

一、电脑安装并配置好Git，尤其是username和email

二、注册GitHub账号

三、在GitHub配置ssh验证：

    解决办法：

    1、生成密钥

    首先进行本地SSH公钥的生成，打开git bash终端或cmd命令行（本文使用cmd命令行进行演示），输入：ssh-keygen -t rsa -C "邮箱地址"，一路回车即可。

    2、验证密钥

    成功会在用户/.ssh文件夹下生成两个密钥文件。

    3、GitHubSSH认证

    打开浏览器登陆Github，点击自己的头像，在下拉列表中选择Settings，然后选择SSH and GPG keys这一栏，如果已经认证过，打开后会看到目前该账户下已进行过SSH认证的机器。

    4、在GitHub添加SSH Key

    点击右上角New SSH key，复制步骤二中后缀名为.pub文件中的所有内容。粘贴至Key中，同时需要编辑一个Title来区分此Key认证的是哪一台机器，通常用计算机的名字，然后点击Add SSH key按钮添加。
    (此时遇到的问题：无法打开后缀名为.pub文件
    解决办法：.ssh中有一个文件【id_rsa.pub】,创建的密钥就储存在这里，但是不能双击打开，用windows的命令行，进入.ssh 中，用命令【more id_rsa.pub】获取内容，就是密钥，可以复制下来。)

    5、验证认证结果

    保存后，回到git bash中，输入ssh git@github.com进行认证验证。

四、掌握一定cmd命令操作：
    cmd的打开方式，win10可以通过win键+R打开运行。开始-运行-输入cmd即可打开。

五、掌握配置各种环境变量

具体步骤：

一、安装Hugo：下载好后将其解压到Hugo空文件夹，在此文件夹下，打开cmd控制台，键入hugo version。

    问题：没有出现相关内容说明环境变量没有配置合适
    解决办法：在系统变量（s)中先新建一个hugo文件夹，然后直接在path中新建“%hugo%”，再用hugo version测试，只要出现版本号即可。

二、创建个人博客文件夹

仍然在此文件夹下打开cmd命令行，键入hugo new site myblog(自定义的博客名)；键入hugo new site myblog 后会创建一个myblog文件夹。

三、下载并设置博客主题

进入Hugo博客主题下载库挑选自己喜欢的博客模板（这里以m10c模板为例）点击Download，在D:\Program Files\Github\Hugo\myblog目录下打开cmd命令，键入上面获取到的git clone代码（git clone https://github.com/vaga/hugo-theme-m10c.git themes/m10c）。

    问题：在此之前应先使用git将博客模板从网上克隆下来
    解决办法：
    1)首先要在本地新建一个文件夹，作为本地仓库。例如：新建文件夹mydata
    2)进入mydata文件件下，右击-git bush here
    3)进入下面的界面，输入git init，将本地仓库初始化
    （图像链接：https://img-blog.csdn.net/20180531110500796?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2V2YV9sdQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70）
    4)将你需要的项目从github或者服务器上克隆下来
    命令：git clone url
    （url为为项目服务器地址或github地址）
    命令：git clone https://github.com/vaga/hugo-theme-m10c.git theme/m10c
    5)完成（打开文件夹，项目已经被clone到本地了）

四、在本地启动个人博客

在D:\Program Files\Github\Hugo\myblog目录打开cmd命令行，键入hugo server -t m10c --buildDrafts
复制末尾的http://localhost:1313到自己的浏览器中(注意此时cmd命令行保持打开)

五、实际写一篇文章来测试

切换到myblog目录下，cmd命令行中键入hugo new post/blog.md
这个操作会在myblog\content\post路径下生成一个blog.md文件。
可以用vsCode或者其他能编写markdown文件的编译器打开blog.md文件，写一些正文(注意将draft属性改为false,否则无法显示)

切换到myblog目录下，cmd命令行中键入命令hugo server -t m10c --buildDrafts
cmd命令窗不要关闭，复制http://localhost:1313，在浏览器中打开

六、将个人博客部署到远端服务器(可以使用github部署到github仓库)

在github创建一个远端仓库

    注意：这个仓库的名称必须是Github的username+.github.io

在myblog目录下，cmd命令行中键入命令hugo --theme=m10c --baseUrl="https://github.com/JackyTech/JackyTech.github.io/" --buildDrafts

    注意:这里的m10c主题和仓库路径要填写自己的

代码执行后就会在myblog文件夹下生成一个public文件夹。
接下来把public文件推送到github上：
切换到public文件夹下，代开命令行窗口，依次键入：(因为此前已经建立了repository，所以直接从命令行推送现有存储库)

git remote add origin https://github.com/JackyTech/JackyTech.github.io.git (绑定了.git配置文件夹对应的远端服务器的已经发布了)

git branch -M main(将master分支转换为main)

git push -u origin main (推送到githubu)

    问题：github把master默认分支改为了main, 为了适应它这种变化, 我们也要做相应调整, 把本地git的master改成main。
    解决方法：修改已创建项目的主分支为main，切换到主分支master，使用git branch -M main命令, 把当前master分支改名为main, 其中-M的意思是移动或者重命名当前分支。

