[toc]
# 前言 #
最近开了一个Minecraft1.12.2的模组服务器，踩了许多坑，特此记录一下。

## 工具介绍 ##
- 阿里云服务器
- FinalShell	--SSH连接
- WinSCP	--FTP文件传输

## 软件介绍 ##
- Minecraft 1.12.2	--官服
- Forge		--对应版本的forge
- Sponge 1.12.2		--海绵服：Mod和插件共存
- Nucleus		--命令插件
- LuckPerms		--权限插件

## 模组介绍 ##
- 拔刀剑
- 地幔		--匠魂前置mod
- 暮色森林
- 工业时代2
- 匠魂2

工具自行下载

1. 官服：[https://www.minecraft.net/zh-hans/download/server/](https://www.minecraft.net/zh-hans/download/server/)
2. Forge：[https://files.minecraftforge.net/](https://files.minecraftforge.net/)
3. Sponge：[https://www.spongepowered.org/](https://www.spongepowered.org/)
4. Mod：[https://www.mcmod.cn/](https://www.mcmod.cn/)

注：以上均为1.12.2版本，插件在Sponge官网下载。

# 阿里云服务器 #

阿里云近期有一个高校学生计划，可以免费领取6个月2核4g的突发性实例，到期前一个月进行测试可以免费续费6个月，也就是白给一年的服务器。
**需要注意的是需要经过实名认证，学生认证，然后答题（10题对6题就行，百度可以搜到）**

## 领取过程 ##
首先进入官网：[https://developer.aliyun.com/adc/student/](https://developer.aliyun.com/adc/student/)
答题后就可以领取了：
![领取页面](https://cdn.jsdelivr.net/gh/yihansix6/PersonalBed/minecraft/blog-imgblog5-1.png "领取页面")

**地域离哪里进选哪里，
系统选择Ubuntu1804**

![系统选择](https://cdn.jsdelivr.net/gh/yihansix6/PersonalBed/minecraft/blog-imgblog5-2.png "系统选择")

**立即购买，确认，再确认。**

![确认购买1](https://cdn.jsdelivr.net/gh/yihansix6/PersonalBed/minecraft/blog-imgblog5-3.png "确认购买1")

![确认购买2](https://cdn.jsdelivr.net/gh/yihansix6/PersonalBed/minecraft/blog-imgblog5-4.png "确认购买2")

*领取成功！*

![领取成功](https://cdn.jsdelivr.net/gh/yihansix6/PersonalBed/minecraft/blog-imgblog5-5.png "领取成功")

## 安全组配置 ##

*领取成功之后，首先要重置密码，首先来到实例详情页面，点击重置实例密码，之后输入密码，重启：*

![实例详情](https://cdn.jsdelivr.net/gh/yihansix6/PersonalBed/minecraft/blog-imgblog5-6.png "实例详情")

*密码修改完成，配置安全组的放行：*

![配置安全组](https://cdn.jsdelivr.net/gh/yihansix6/PersonalBed/minecraft/blog-imgblog5-7.png "配置安全组")

**点击快速创建实例，自定义端口选TCP，在后方输入"25565/25565"(Minecraft默认端口)，授权对象为"0.0.0.0/0"(所有对象均可使用)，其他的默认不变，点击确认即可。**

![创建规则](https://cdn.jsdelivr.net/gh/yihansix6/PersonalBed/minecraft/blog-imgblog5-8.png "创建规则")

# 安装JDK #
Minecraft是运行在JAVA环境上的，所以需要安装jdk，版本不需要太高，我这里使用的是jdk1.8
jdk下载链接：[https://pan.baidu.com/s/18AjCNFeJ160CgAu_zegKFQ](https://pan.baidu.com/s/18AjCNFeJ160CgAu_zegKFQ)
提取码：v7jc

## 连接服务器 ##

*接下来就是连接服务器了，可以使用Xshell、Putty、SecureCRT、FinalShell，我这里使用的是FinalShell。*

**打开FinalShell，点击文件夹图标=>新建连接=>SSH连接,填写公网ip，用户名，密码，点击确认即可连接成功。**
![连接服务器](https://cdn.jsdelivr.net/gh/yihansix6/PersonalBed/minecraft/blog-imgblog5-10.png "连接服务器")

## FTP上传文件 ##
*使用winscp连接服务器，与上方步骤基本一致*

![winscp连接服务器](https://cdn.jsdelivr.net/gh/yihansix6/PersonalBed/minecraft/blog-imgblog5-11.png "winscp连接服务器")

**将上方下载的jdk文件移动至服务器的root文件夹下**

![移动文件](https://cdn.jsdelivr.net/gh/yihansix6/PersonalBed/minecraft/blog-imgblog5-12.png "移动文件")

## jdk安装步骤 ##
回到FinalShell，输入"ll"查看当前目录下所有文件的详细资料,
"cd /root"进入root文件夹,"pwd"查看当前目录路径。
找到刚刚上传的jdk文件，输入"tar -zxvf jdk..."("tab"可以补全命令)进行解压。

`ll`
`cd /root`
`pwd`
`tar -zxvf jdk-8u251-linux-x64.tar.gz`

![查看文件详情](https://cdn.jsdelivr.net/gh/yihansix6/PersonalBed/minecraft/blog-imgblog5-13.png "查看文件详情")

![解压](https://cdn.jsdelivr.net/gh/yihansix6/PersonalBed/minecraft/blog-imgblog5-14.png "解压")

*解压完成之后，需要配置环境变量*

**输入"vim /etc/profile"，之后在最下按字母"i"键或"o"键插入（vim指令详情自行查找），粘贴变量配置语句，完成后按esc键，然后按冒号键(shifit+:),输入wq（保存并退出）**
**输入"source /etc/profile"使环境变量生效，输入"java -version"查看是否配置成功，出现版本号即表示安装成功**

`vim /etc/profile`
`source /etc/profile`
`java -version`


![修改环境变量](https://cdn.jsdelivr.net/gh/yihansix6/PersonalBed/minecraft/blog-imgblog5-16.png "修改环境变量")

```
export JAVA_HOME=/root/jdk1.8.0_251
export CLASSPATH=.:$JAVA_HOME/jre/lib/rt.jar:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
export PATH=$PATH:$JAVA_HOME/bin
```

![配置环境变量](https://cdn.jsdelivr.net/gh/yihansix6/PersonalBed/minecraft/blog-imgblog5-15.png "配置环境变量")

# Minecraft服务器 #
**接下来即使安装服务器了，首先"cd /home"到根home目录下，"mkdir liao"（liao是文件夹名字，随意取），然后输入"cd liao"进入liao文件夹,"mkdir sponge1.12.2"创建最终配置服务器文件夹**

`cd /home`
`mkdir liao`
`mkdir sponge1.12.2`

![创建文件夹](https://cdn.jsdelivr.net/gh/yihansix6/PersonalBed/minecraft/blog-imgblog5-17.png "创建文件夹")

在winscp中把服务器核心以及mod之类的文件放入文件夹中
文件链接：
[https://pan.baidu.com/s/1kXogKfWc0SOw9HStou4JEA](https://pan.baidu.com/s/1kXogKfWc0SOw9HStou4JEA)
提取码：eutk

![mod上传](https://cdn.jsdelivr.net/gh/yihansix6/PersonalBed/minecraft/blog-imgblog5-18.png "mod上传")

## 纯净服 ##
**"cd sponge1.12.2"来到目标文件夹，"ll"查看刚刚上传的文件，都没问题的话，可以创建一个启动文件**
**"vim start"，粘贴"java -Xmx4000M -Xms3000M -jar minecraft_server.1.12.2.jar nogui"（Xmx表示最大内存，Xms表示最小内存，根据服务器内存大小自行调节），esc后":wq"退出**

`cd sponge1.12.2`
`vim start`
```
java -Xmx4000M -Xms3000M -jar minecraft_server.1.12.2.jar nogui
```

![创建启动文件](https://cdn.jsdelivr.net/gh/yihansix6/PersonalBed/minecraft/blog-imgblog5-19.png "创建启动文件")

**然后要给刚刚创建的启动文件赋权，输入"chmod 777 start"，这样start启动文件就可以运行了，输入"./start"运行**

**接下来它会提示你："You need to agree to the EULA in order to run the server."意思是：需要先同意他的协议才能运行服务器。输入"vim eula.text"，将"eula=false"改为"eula=true"，保存退出**

`chmod 777 start`
`./start`
`vim eula.text`
`eula=true`

![运行启动文件](https://cdn.jsdelivr.net/gh/yihansix6/PersonalBed/minecraft/blog-imgblog5-20.png "运行启动文件")

**授权之后就可以运行服务器了，输入"./start"运行**

*但是做完这些还不够，如果你或者你的好友不是正版用户，那么必须将服务器的正版验证关闭。*

**首先先关闭服务器，输入"stop",输入"vim server.properties"，将"online-mode=false"改为"true",保存退出即可**
如果还想修改一下其他的配置可以参考一下Minecraft wiki:
[https://minecraft-zh.gamepedia.com/Server.properties](https://minecraft-zh.gamepedia.com/Server.properties)

------------

到了这一步纯净服的配置基本完成了，但是想要在关闭ssh连接的时候保持mc服务器运行的话需要"screen"这个命令
**首先，安装screen，输入"apt-get update"更新系统软件，然后输入"apt-get install screen"，安装过程中会提示你是否确认安装，输入"y"同意即可安装成功**
```
apt-get update

apt-get install screen
```

![安装screen](https://cdn.jsdelivr.net/gh/yihansix6/PersonalBed/minecraft/blog-imgblog5-24.png "安装screen")

**输入"screen -S mc"创建一个名为mc的进程，"screen -ls"可以查看所有进程，"screen -r"连接进程，在进程里，可以按"Ctrl+A+D"退出进程**
*这样就可以保证mc服务器一直运行*

```
screen -S mc
screen -ls
screen -r
```

**输入"screen -r"进入进程，在服务器目录中输入"./start"即可启动服务器，按"Ctrl+A+D"退出进程，然后关闭ssh连接。**

*之后就可以进入游戏输入ip连接即可，如果想修改背景图片，可以将图片上传至服务器目录，并更名为"server-icon.png"即可*

![连接](https://cdn.jsdelivr.net/gh/yihansix6/PersonalBed/minecraft/blog-imgblog5-25.png "连接")
![连接](https://cdn.jsdelivr.net/gh/yihansix6/PersonalBed/minecraft/blog-imgblog5-26.png "连接")
![成功](https://cdn.jsdelivr.net/gh/yihansix6/PersonalBed/minecraft/blog-imgblog5-27.png "成功")

------------

**至此，Minecraft纯净服务器就配置完成了，如果不需要mod和插件，就可以愉快的和好兄弟们游玩了！**

## 模组服 ##
*想要安装模组和插件都需要先安装forge*
**首先来到服务器目录，输入指令"java -jar forge-1.12.2-14.23.5.2854-installer.jar nogui --installServer"安装forge**

```
java -jar forge-1.12.2-14.23.5.2854-installer.jar nogui --installServer
```

*需要注意：安装forge的过程很长，需要耐心等待，并且有可能出现下载出错的情况，解决办法有两种：*
1. 输入刚刚的指令再安装一遍
2. 自行上网下载在服务器下载异常的文件

例如：

![下载异常](https://cdn.jsdelivr.net/gh/yihansix6/PersonalBed/minecraft/blog-imgblog5-29.png "下载异常")
![下载异常文件](https://cdn.jsdelivr.net/gh/yihansix6/PersonalBed/minecraft/blog-imgblog5-30.png "下载异常文件")

forge安装的所有文件都在一个叫做"libraries"的文件夹上，复制刚刚下载异常的文件，在谷歌上搜索下载（至于怎么上谷歌自行解决），或是在这个网站搜：
[https://mvnrepository.com/](https://mvnrepository.com/)
在"libraries"中找到下载异常的文件，删除，把刚刚下的文字粘贴上去，然后再安装forge一遍即可安装成功

![删除异常文件](https://cdn.jsdelivr.net/gh/yihansix6/PersonalBed/minecraft/blog-imgblog5-31.png "删除异常文件")
![forge安装成功](https://cdn.jsdelivr.net/gh/yihansix6/PersonalBed/minecraft/blog-imgblog5-32.png "forge安装成功")

接下来把strat文件替换一下，输入"vim start"，将文本替换为"java -Xmx4000M -Xms3000M -jar forge-1.12.2-14.23.5.2854.jar nogui"，保存退出

"./start"运行服务器，"stop"停止服务器，会发现目录中出现一个"mods"的文件夹，这说明forge已经安装成功了

将mod放入mods文件夹，客户端也带有相应的mod，即可成功的游玩，那么模组服就算是配置成功了

```
java -Xmx4000M -Xms3000M -jar forge-1.12.2-14.23.5.2854.jar nogui
```

## 模组插件服 ##
当然，仅有模组还不够，在服务器中添加插件可以增加游戏便利性和乐趣（我添加插件的的原因就是为了使用tpa传送功能）
来到服务器目录，将"spongeforge"，"Nucleus"，"LuckPerms"插件放入mods文件夹，然后启动服务器，这样插件就安装成功了

------------

接下来就是要配置权限了，详情参考文档：
Nucleus：
[https://www.mcbbs.net/thread-732446-1-1.html](https://www.mcbbs.net/thread-732446-1-1.html)
LuckPerms：
[https://github.com/PluginsCDTribe/LuckPerms/wiki/Command-Usage#lp-info](https://github.com/PluginsCDTribe/LuckPerms/wiki/Command-Usage#lp-info)

那么我就讲一下如何配置tpa权限
使用了LuckPerms权限插件之后，只有服务器控制台有权限使用所有的命令了，所有的玩家都在"default"默认组内，没有任何权限
所以首先需要创建一个管理员组，将一个玩家添加到管理员组即代表这个玩家是管理员。

在服务器控制台中输入"lp creategroup admin"，创建一个名为admin的空权限组

"lp group admin permission set nucleus"，为admin组添加nucleus的所有命令，这个命令一般只给服主自己

"lp user yihansix parent add admin"，将用户yihansix添加到admin组，yihansix即拥有了admin组下的所有命令

**既然所有的用户都在default组中，我们就可以给default组添加tpa的指令，输入"lp group default permission set
nucleus.teleport.tpa.base"，这样default组中的用户可以想其他用户发送tpa请求了，但是需要完成tpa操作还需要其他玩家同意这个tpa请求，那么还需要一个同意指令，输入"lp group default permission set
nucleus.teleport.tpaccept.base"，这样tpa指令就能正常使用，如果还需要配置其他权限，请自行观看文档。**

```
lp creategroup admin
lp group admin permission set nucleus
lp user yihansix parent add admin
lp group default permission set nucleus.teleport.tpa.base
lp group default permission set nucleus.teleport.tpaccept.base
```

------------

至此，所有的教程就结束了，虽然我也是一个Minecraft新人，但是有什么问题欢迎在下方留言评论，感谢观看，回见~