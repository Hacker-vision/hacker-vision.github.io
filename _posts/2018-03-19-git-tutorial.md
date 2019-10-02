---
title: "Linux & git 常用命令"
date: 2018-03-19 21:30:23 +0800
category: Markdown
tags: [Markdown, Minimalism]
excerpt: 汇总笔者在Linux开发过程中常用shell命令，以及git、vim命令等,方便查阅。
---

&#160; &#160; &#160; &#160;  git 是一种分布式版本管理工具，特点是git分支管理，与开源的远程仓库github配合使用。使用时注意区分工作目录、暂存区域、本地仓库、远程仓库。

- [ ] 基本命令

```bash
git init
git add readme.txt
git commit -m "wrote a readme file"
git remote add origin https://github.com/Hacker-vision/itchat-master.git //using https
git remote set-url origin https://github.com/Hacker-vision/itchat-master.git //using https
或git remote add origin git@github.com:Hacker-vision/leetcode.git //using ssh,需要本地生成秘钥，在SSH服务器上注册,优点是不用每次都输入密码
git push -u origin master 
```
- [ ] 可能用到的命令

```shell
git clone --branch new_branch https://github.com/Hacker-vision/itchat-master.git new-diretory[指定目录，缺省会创建与远程仓库名相同的目录名]
git config --global user.name "ajksunkang"
git config --global user.email "ajksunkang@pku.edu.cn"
git pull origin master //远程仓库有更新
git clean -d -fx //暂存区域的文件修改全部删除
```
- [ ] 查看历史日志

```bash
git status
git log
```
- [ ] 回退版本

```bash
git checkout 3628164       //切换到某个commit id
git reset --hard HEAD^
git reset --hard 3628164
git reflog                 //回退后返回最新版本
```
- [ ] 分支管理

```bash
git branch new_branch       //创建 new branch
git checkout new_branch     //切换 branch
git branch                  //查看 branch情况
git merge new_branch        //合并 branch
git branch -d new_bacnch    //删除 old branch
```

trygit 15分钟教程：[https://try.github.io](https://try.github.io)

---
### Linux命令备忘录

- [ ] apt-get安装和卸载命令

```bash
apt-get update                          //更新源
apt-get upgrade                         //升级软件
apt-get install software_name           //安装软件（推荐）
apt-get purge(--purge remove) software_name  //卸载软件及其配置文件，保留依赖的安装包（推荐）
=====
apt --reinstall install software_name   //强制重新安装
apt-get remove  software_name                //卸载软件但保留配置文件，保留依赖的安装包
apt-get autoremove software_name        //卸载软件及其依赖的安装包，保留配置
```
- [ ] diff & patch

1.单个文件
```bash
diff –uN  from-file  to-file  > to-file.patch  //创建补丁
patch –p0 < to-file.patch                      //打补丁
patch –RE –p0 < to-file.patch                  //清除补丁
```
2.多个文件
```bash
diff –uNr  old_docu  new_docu >  myfirst.patch  //创建补丁
cd old_docu/
patch –p1 < ../myfirst.patch                    //打补丁
patch –R –p1 < myfirst.patch                     //清除补丁
```

- [ ] 查看服务器配置

```bash
lscpu                          //处理器信息
free -m                        //内存大小（MB）
df -h                          //硬盘大小
```
- [ ] 查看内核版本和发行版本

```bash
//查看内核版本（linux 4.19.23）
uname -a
cat /proc/version
//查看发行版本（ubuntu、CentOS、Debian等）
lsb_release -a
cat /etc/issue
```

- [ ] U盘挂载

```bash
lsblk                          //打印块设备
sudo mount /dev/sdb1 /mnt
sudo umount /mnt 
```

- [ ] 查看文件大小

```bash
du -h --max-depth=0 vmlinux    //查看某个文件or目录的字节大小
```

- [ ] 软连接

```bash
ln -s 源目录或文件 新链接文件
```

- [ ] 环境变量

```bash
（1）vim /etc/profile     //所有用户生效（永久）
（2）vim ~/.bash_profile
source  ~/.bash_profile    //当前用户,不source的话下次重进此用户才生效（永久）
（3）export PATH=$PATH:/some/path  //当前用户，退出终端失效

```


---
### 我的vim配置（~/.vimrc）

```bash
" 自动显示行号
set nu
" ColorScheme
set background=dark
" 搜索逐字符高亮
set hlsearch
set incsearch
"突出显示当前行
set cursorline
```
### 常用vim操作

```bash
shift+d  //删除光标到行末
shift+g  //调到文件最后一行
shift+a  //光标调到当前行的行末
:vsp filenname     //分屏
ctrl + ww         //分屏切换
:%s/sunkang//gn   //显示匹配个数
o                 //插入空行
```

```bash
shell: ctrl + a  //光标跳到当前指令的第一个字符处
```



### linux压缩解压缩命令大全
```bash
.tar 
解包：tar xvf FileName.tar
打包：tar cvf FileName.tar DirName
（注：tar是打包，不是压缩！）
———————————————
.xz
解包: xz -d Filename.xz
解包：unxz Filename.xz
———————————————
.gz
解压1：gunzip FileName.gz
解压2：gzip -d FileName.gz
压缩：gzip FileName

.tar.gz 和 .tgz
解压：tar xzvf FileName.tar.gz
压缩：tar zcvf FileName.tar.gz DirName
———————————————
.bz2
解压1：bzip2 -d FileName.bz2
解压2：bunzip2 FileName.bz2
压缩： bzip2 -z FileName

.tar.bz2
解压：tar jxvf FileName.tar.bz2
压缩：tar jcvf FileName.tar.bz2 DirName
———————————————
.bz
解压1：bzip2 -d FileName.bz
解压2：bunzip2 FileName.bz
压缩：未知

.tar.bz
解压：tar jxvf FileName.tar.bz
压缩：未知
———————————————
.tar.xz

解压：(xz -d FileName.tar.xz & tar xvf FileName.tar) | tar -Jxvf FileName.tar.xz
压缩：tar -Jcvf FileName.tar.xz DirName
_______________
.Z
解压：uncompress FileName.Z
压缩：compress FileName
.tar.Z

解压：tar Zxvf FileName.tar.Z
压缩：tar Zcvf FileName.tar.Z DirName
———————————————
.zip
解压：unzip FileName.zip
压缩：zip -r FileName.zip DirName
———————————————
.rar
解压：rar x FileName.rar
压缩：rar a FileName.rar DirName
———————————————
.lha
解压：lha -e FileName.lha
压缩：lha -a FileName.lha FileName
———————————————
.rpm
解包：rpm2cpio FileName.rpm | cpio -div
———————————————
.deb
解包：ar p FileName.deb data.tar.gz | tar zxf -
```

### 服务器网络配置+添加管理员

注：虚拟机配置静态IP时要将网络设置成桥接，而NAT网络默认虚拟机是强制DHCP的.

```bash
ifconfig -a           #查看所有网卡设备，包括没启用、ifconfig显示不出来的
ifconfig etho up      #启动网卡eth0
ifconfig etho down    #关闭网卡eth0

ubuntu宿主机配置静态IP
sudo vim /etc/network/interfaces #网络配置文件
修改内容如下：
# The loopback network interface
auto lo
iface lo inet loopback

#The primary network interface
auto enp2s0f0
iface enp2s0f0 inet static
        address 192.168.200.32
        netmask 255.255.255.0
        network 192.168.200.0
        broadcast 192.168.200.255
        gateway 192.168.200.2
        dns-nameservers 162.105.129.27 #pku dns
保存文件，重启网卡设备。   
/etc/init.d/networking restart
ifconfig  //查看新的网络设置
```

```bash
sudo adduser sunkang  #添加用户,会创建同名的用户主目录
sudo vim /etc/sudoers  #新用户设置为管理员
修改文件如下：
# User privilege specification
root ALL=(ALL) ALL
sunkang ALL=(ALL) ALL
保存退出，sunkang就有了root权限
```

### U盘启动盘安装心得

[1.window下制作ubuntu的启动盘->ultraISO(https://blog.csdn.net/zjx2014430/article/details/49303785)](https://blog.csdn.net/zjx2014430/article/details/49303785)

[2.window下制作win7/win10的启动盘(winPE)->大白菜(http://www.dabaicaipe.cn/upqdzz.html)](http://www.dabaicaipe.cn/upqdzz.html)

[3.ubunutu下制作ubuntu的启动盘->dd(https://blog.csdn.net/wang4it/article/details/78998217)](https://blog.csdn.net/wang4it/article/details/78998217)

格式化成ext4(U盘仅Linux识别):`sudo mkfs.ext4 /dev/sdb4`

格式化成fat32(兼容性更好):`sudo mkfs.fat /dev/sdb4 -I`
