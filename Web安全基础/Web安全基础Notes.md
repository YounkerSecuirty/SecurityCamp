# Web Security Summer Camp Resources&Tools
## VirtualMachine Software
#### Virtualbox (Windows、Linux、Mac)
```
https://www.virtualbox.org/
```
#### Vmware (Windows、Linux、Mac)
```
https://www.vmware.com/
```
#### Parallels Desktop (Mac)
```
https://www.parallels.com
```
## 渗透测试系统
```ParrotSecurity OS```Offical Site
```
https://www.parrotsec.org/
https://parrotsec-cn.org ParrotSec中文社区（推荐）
```
```Kali Linux``` Offical Site
```
https://www.kali.org/ (垃圾不要用)
```
## LAMP/LNMP搭建
安装必要组件
```
sudo apt-get install build-essential
```
___
apache
```
sudo apt-get install apache2
```
mysql
```
sudo apt-get install mysql-server libapache2-mod-auth-mysql php5-mysql
```
php
```
sudo apt-get install php5 libapache2-mod-php5 php5-mcrypt
```
phpmyadmin
```
sudo apt-get install phpmyadmin
```
nginx
```
sudo apt-get install nginx
```
## 常用安全工具
Burpsuite、Nmap、Metasploit什么的渗透测试系统基本都有可以直接虚拟机，不爽的话你可以直接装物理机
Windows的话，不愿意用虚拟机，可以安装一下那个Windows linux terminal
Mac的话，先安装brew包管理器
```
/usr/bin/ruby -e "$(curl -fsSL ￼https://raw.githubusercontent.com/Homebrew/install/master/install)"
```
```
brew install Nmap
brew install sqlmap
```
先去下载metasploit包，官网如下：
```
https://www.metasploit.com/
```
```
curl https://raw.githubusercontent.com/rapid7/metasploit-omnibus/master/config/templates/metasploit-framework-wrappers/msfupdate.erb > msfinstall && \
  chmod 755 msfinstall && \
  ./msfinstall
```
