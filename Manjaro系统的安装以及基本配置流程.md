
# Manjaro 系统的安装以及基本配置流程

## 前言

### 为什么要用 Linux

因为要学习嵌入式系统高级编程，在课程学习的时候需要使用 Linux 虚拟机，虽然虚拟机可以多开，备份系统也更加方便，但因为想学习体验流畅美观的系统故有此文。

这里将记录一些网络上随处可见的刚装好 Linux 系统的通用配置。装好 Linux 后看这个就可以了！

+ 美化后，好看！
+ 安装软件一个命令就搞定，方便！
+ Linux 内核基于标准规范 32/64 位计算机最佳化设计，稳定！
+ 因为稳定所以很多服务器都是使用 Linux 系统，所以网络工具就十分丰富！
+ xxxxxxxxxx int fputc(int ch,FILE *f){    HAL_UART_Transmit(&hlpuart1,(uint8_t*)&ch,1,0xFFFF);    return ch;}c
+ ...

### Manjaro 是什么？以及为什么选择 Manjaro

Manjaro 基于 Arch Linux，是一种 Linux 操作系统。

原因嘛，pacman 好用？滚动式更新？社区比较活跃？用的人多，大家都推荐？

## 换源

打开终端输入命令：

```
sudo vim /etc/apt/sources.list
```

`sudo` 超级用户权限

`vim` 编辑器，如果你有 vscode 也可以替换成 `code` 命令打开

`/etc/apt/sources.list` 是要修改文件的地址

注释掉之前源的连接，并加入新的源链接：

```
#清华大学：
deb http://mirrors.tuna.tsinghua.edu.cn/kali/ kali-rolling contrib main non-free
deb-src http://mirrors.tuna.tsinghua.edu.cn/kali/ kali-rolling contrib main non-free
#阿里云：
deb http://mirrors.aliyun.com/kali kali-rolling main non-free contrib
deb-src http://mirrors.aliyun.com/kali kali-rolling main non-free contrib
#中科大：
deb http://mirrors.ustc.edu.cn/kali kali-rolling main non-free contrib
deb-src http://mirrors.ustc.edu.cn/kali kali-rolling main non-free contrib
#官方源：
deb http://http.kali.org/kali kali-rolling main non-free contrib
deb-src http://http.kali.org/kali kali-rolling main non-free contrib
```

使用 vim 的话需要按下 Esc 键后键入 `:wq`才能完成写入并退出！

最后键入以下命令更新源，如果遇到问题请检查网络

```
sudo apt update
```

`apt` Linux 下的一款包管理工具

键入以下命令更新当前可供升级的程序

```
sudo apt upgrade
```

## 系统汉化 (kali Linux)

终端输入

```
sudo dpkg-reconfigure locales
```

下拉找到 zh-CH.UTF-8，使用 `reboot`命令重启生效

## 配置 zsh

### 安装 zsh

```
sudo pacman -S zsh
```

### 安装 oh my zsh

[oh my zsh 官网](https://ohmyz.sh/)

```
#通过 wget 安装
sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"

#通过 curl 安装
sh -c "$(wget https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh -O -)"
```

`sh`	Linux 系统中执行 shell 脚本的命令

`-c`	将一个字符串作为完整的命令执行

`wget`	Linux 命令，用来从指定的 URL 下载文件

`curl`	linux 命令，是一种命令行工具，作用是发出网络请求，然后得到和提取数据并显示。
