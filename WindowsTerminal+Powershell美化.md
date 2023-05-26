

# 前言

好看对我来说是生产力的前提 ~~虽然好看也没生产力~~ ，有哪个程序员不希望自己的每个编程界面都优雅高效呢？我姑且是被Windows Terminal的[官方宣传视频](https://www.bilibili.com/video/BV1P541147TY?from=search&seid=14976278141629704066)震撼到了，真的是太炫酷辣！然后为了方便以后再配置，故有了下文。

# Terminal（终端）是什么？

**我们总在说在终端中如何操作，那么终端到底是什么呢？为什么它会有这么大的权利？**

要说清终端是什么，我们先来看看操作系统的组成。简化来说，**操作系统分为两个部分，一部分称作内核，另一部分成为用户交互界面**。内核部分负责系统的全部逻辑操作，由海量命令组成，这一部分是系统运行的命脉，不与用户接触；交互界面则是开机之后所有我们所看到的东西，比如窗口，软件，应用程序等等。

那么我们若我们想对系统内核的某些操作逻辑做出一些修改，应该怎么办呢？**终端就是连接内核与交互界面的这座桥**，它允许用户在交互界面上打开一个叫做「Terminal 终端」的应用程序，在其中输入命令，系统会直接给出反馈。

因为终端这座桥，实际允许用户间接控制系统内核，也就是系统的大脑，因此它理论上具备控制一切的权利。

作者：WorldPeace_hp
链接：https://www.jianshu.com/p/7ce57c3060b3
来源：简书

# 题外话：安装包管理器scoop

scoop 是Windows中一个超级好用的包管理器，很多软件都可以通过scoop 一个命令安装，省去了配置环境变量等很多麻烦。 你可以理解为一个没有可视化界面的应用商店，在这个应用商店安装想要的应用只要一个命令就可以了，是不是很方便呢？

详细的安装步骤可以看另一篇文章

# 开始吧！

## 安装Terminal

可以去微软的应用商店里直接搜索 Windows Terminal 进行下载（商店版的应该有某些限制），也可以去 github 上找安装包进行安装，直接搜索即可。  [官方链接](https://github.com/microsoft/terminal/releases)

往下翻找到 `Asset`下的内容点击标题即可下载安装包，如果长时间仍未开始下载或者下载出错建议尝试科学上网解决问题。

Terminal 只是提供一个统一的终端窗口，我们可以发现再屏幕的上方可以打开很多命令行的界面，例如：git，cmd，powershell 等。

安装 posh-git 提供 git 的相关功能显示，但要提前安装好git

在安装好scoop后git可以使用命令 `scoop install git`一键安装

## 配置 Terminal

安装好 Terminal 后就可以配置所要搭载的终端信息了

+ 点开 Terminal的设置
+ 接着点击页面左下角的 `打开JSON格式文件`。
+ 在 `JSON`格式文件中添加/修改这样一条信息(看图，看好我添加信息的位置和两边的括号，如果在list中添加的信息不是最后一系列信息，大括号后面要加逗号)

  ```json
   "list": 
          [
              {
                  "commandline": "powershell.exe -nologo", //在你的shell配置好环境变量后直接输入shell名字即可，我这里使用系统预装powershell。后面的 -nologo参数是屏蔽每次启动powershell都会弹出的提示升级信息。
                  "fontFace": "JetBrains Mono",			//这里可以选择你已经安装的系统字体。
                  "guid": "{61c54bbd-c2c6-5271-96e7-009a87ff44bf}",
                  "hidden": false,
                  "name": "Windows PowerShell",
                  "useAcrylic": true						//是否开启毛玻璃效果，这里我选择了开启
              },
          ]
  ```

## 配置 Powershell

我们先安装两个插件，右键 Windows 选择 `Windows Powershell 管理员模式`

oh-my-posh 就是我们所要用到的主题模块了,输入命令 `Install-Module posh-git`

如果有git的需求可以输入命令 `Install-Module oh-my-posh`来使 powershell 支持对 git 的功能

**最后一步！**

如果安装了vscode，在命令行界面输入 `code $profile`

在打开的文件中输入如下代码并保存

```
Import-Module posh-git
Import-Module oh-my-posh
Set-PoshPrompt -Theme Paradox
```

前两个命令在开启powershell的时候默认打开已经下好的两个模块功能，最后一个命令是选择主题的命令，最后一个参数可以根据喜好进行更改。

输入命令 `Get-PoshThemes`可以查看所有当前缓存的主题。

输入命令 `Update-Module oh-my-posh`可以对主题模块进行升级。

# 问题

+ 如果出现显示乱码，建议更换字体，首先安装字体，这里建议使用[Nerd Fonts](https://www.nerdfonts.com/)字体，安装后在 Terminal 中配置使用即可，注意名称需要与系统设置里更改字体部分保持一致。

安装后的字体会出现在这里，名称要一样。
