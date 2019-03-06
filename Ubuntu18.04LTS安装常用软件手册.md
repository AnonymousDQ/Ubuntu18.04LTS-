

# 一、安装Ubuntu18.04LTS

- 去老毛桃[官网下载](http://www.laomaotao.org/download.html?download=http://down.lmtxz1.cn/20190125/LaoMaoTao_STA_gw.exe)软件，制作老毛桃U盘。我的笔记本比较旧，要设置BIOS setup的boot选项为**Legacy Support**才能进入WIN PE。

- 利用里面的DiskGenius磁盘分区工具，进行磁盘分区等。
- 利用软碟通Utral ISO软件进行镜像快捷U盘刻录

- 安装Ubuntu的时候，设置BIOS，在BIOS SETUP中选择boot 选项，<u>**选择boot mode为UEFI，U盘设置为Enabled**</u>

然后在Security中设置**Secure boot为disabled**，否则可能导致安装无线插件的时候报错。

```shell
#更新软件源
sudo apt-get update
#安装无线网卡驱动
sudo apt install boradcom-sta-dkms
#提示有未能满足的依赖关系执行以下命令
sudo apt --fix-broken install
```

# 二、Ubuntu18.04LTS更换清华的镜像源

- 先是进行源码备份：
  cp /etc/apt/sources.list /etc/apt/sources.list.backup
- 然后修改源列表：
  sudo vim /etc/apt/sources.list
- 删除源列表的所有内容，复制如下内容并保存：
- [清华镜像源官网：](https://mirrors.tuna.tsinghua.edu.cn/help/ubuntu/)

```shell
# 默认注释了源码镜像以提高 apt update 速度，如有需要可自行取消注释
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-updates main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-updates main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-backports main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-backports main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-security main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-security main restricted universe multiverse

# 预发布软件源，不建议启用
# deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-proposed main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-proposed main restricted universe multiverse
```

# 三、Ubuntu18.04LTS安装谷歌浏览器

- 第一步获取linux的google chrome列表
  sudo wget http://www.linuxidc.com/files/repo/google-chrome.list -P /etc/apt/sources.list.d/

- 第二步获取google的linux密钥
  wget -q -O - https://dl.google.com/linux/linux_signing_key.pub  | sudo apt-key add -

- 第三步进行镜像源更新
  sudo apt update

- 第四步安装获取的google-chrome
  sudo apt install google-chrome-stable

- 第五步：启动谷歌浏览器

  /usr/bin/google-chrome-stable

# 四、Ubuntu18.04LTS安装Typora

```shell
# sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys BA300B7755AFCFAE
wget -qO - https://typora.io/linux/public-key.asc | sudo apt-key add -

# add Typora's repository
sudo add-apt-repository 'deb https://typora.io/linux ./'
sudo apt-get update

# install typora
sudo apt-get install typora
```

# 五、Ubuntu18.04LTS右键怎么添加新建空白文本文档

- 第一步：在Terminal终端上切换到用户主目录的模板文件夹里

  ```shell
  cd ~/模板/
  ```

- 第二步：使用gedit命令打开一个文本文件

  ```shell
  sudo gedit 文本文件
  ```

- 会打开一个空白文件窗口，不要做任何修改直接点击**保存**
- 接着在模板文件夹中会多出一个我们创建的文本文件
- 现在我们可以在桌面上右键，在新建文档中就有文本文件选项。

# 六、Ubuntu18.04LTS安装NotePad++

```shell
#Ununtu下的安装notepad方法：
sudo add-apt-repository ppa:notepadqq-team/notepadqq
sudo apt-get update
sudo apt-get install notepadqq

#Ubuntu下的卸载notepad方法：
sudo apt-get remove notepadqq
sudo add-apt-repository --removeppa:notepadqq-team/notepadqq
```

# 七、安装JDK1.8

- 去Oracle官网下载，[点击链接](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)

- 由于从浏览器下载的东西一般默认都会在下载目录下，所以可以修改一下目录

  ```shell
  #切换到/usr目录，然后新建一个java文件夹
  cd /usr
  sudo mkdir java
  
  #切换到~/下载，然后把刚下在的jdk移动到java文件夹
  cd ~/下载
  sudo mv jdk-8u201-linux-x64.tar.gz /usr/java
  
  ```

- 切换到/usr/java文件夹,解压tar.gz包

  ```shell
  #解压jdk
  sudo tar -zxvf jdk-8u201-linux-x64.tar.gz
  ```

- 设置环境变量

  - 方案一：修改全局配置文件，作用于所有用户：

    ```shell
    #修改配置文件
    sudo vim /etc/profile
    
    #按i进入编辑模式
    export JAVA_HOME=/usr/java/jdk1.8.0_201
    export JRE_HOME=${JAVA_HOME}/jre
    export CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib
    export PATH=.:${JAVA_HOME}/bin:$PATH
    #按ESC后，输入:wq回车
    
    #在终端，输入使得修改的配置立刻生效
    source /etc/profile
    
    #检查是否安装成功
    java -version
    ```

  - 方案二：修改当前用户配置文件，只作用与当前用户：

    ```shell
    #修改配置文件
    vim ~/.bashrc
    
    #按i进入编辑模式
    export JAVA_HOME=/usr/java/jdk1.8.0_201
    export JRE_HOME=${JAVA_HOME}/jre
    export CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib
    export PATH=.:${JAVA_HOME}/bin:$PATH
    #按ESC后，输入:wq回车
    
    #在终端，输入使得修改的配置立刻生效
    source ~/.bashrc
    
    #检查是否安装成功
    java -version
    ```

# 八、安装Maven

- 去官网下载，[点击链接](https://maven.apache.org/download.cgi)

- 由于从浏览器下载的东西一般默认都会在下载目录下，所以可以修改一下目录

  ```shell
  #切换到/usr目录，然后新建一个maven文件夹
  cd /usr
  sudo mkdir maven
  
  #切换到~/下载，然后把刚下在的移动到maven文件夹
  cd ~/下载
  sudo mv apache-maven-3.6.0-bin.tar.gz /usr/maven
  ```

- 切换到/usr/maven文件夹,解压tar.gz包

  ```shell
  #解压maven
  sudo tar -zxvf apache-maven-3.6.0-bin.tar.gz 
  ```

- 设置环境变量

  - 方案一：修改全局配置文件，作用于所有用户：

    ```shell
    #修改配置文件
    sudo vim /etc/profile
    
    #按i进入编辑模式
    export M2_HOME=/usr/maven/apache-maven-3.6.0 
    export PATH=${M2_HOME}/bin:$PATH
    #按ESC后，输入:wq回车
    
    #在终端，输入使得修改的配置立刻生效
    source /etc/profile
    
    #检查是否安装成功
    mvn -v
    ```

  - 方案二：修改当前用户配置文件，只作用与当前用户：

    ```shell
    #修改配置文件
    vim ~/.bashrc
    
    #按i进入编辑模式
    export M2_HOME=/usr/maven/apache-maven-3.6.0 
    export PATH=${M2_HOME}/bin:$PATH
    #按ESC后，输入:wq回车
    
    #在终端，输入使得修改的配置立刻生效
    source ~/.bashrc
    
    #检查是否安装成功
    mvn -v
    ```

# 九、安装Intellij IDEA

- 去IDEA官网下载，[点击链接](https://www.jetbrains.com/idea/download/#section=linux)

- 由于从浏览器下载的东西一般默认都会在下载目录下，所以可以修改一下目录

  ```shell
  #切换到/usr目录，然后新建一个idea文件夹
  cd /usr
  sudo mkdir idea
  
  #切换到~/下载，然后把刚下在的移动到maven文件夹
  cd ~/下载
  sudo mv ideaIU-2018.3.4.tar.gz /usr/idea
  ```

- 切换到/usr/idea文件夹,解压tar.gz包

  ```shell
  #解压maven
  sudo tar -zxvf ideaIU-2018.3.4.tar.gz
  #赋予权限
  sudo chmod 755 -R idea-IU-183.5429.30/
  ```



- 破解，下载破解jar包，[点击链接](http://idea.lanyus.com/jar/JetbrainsIdesCrack-4.2-release-sha1-3323d5d0b82e716609808090d3dc7cb3198b8c4b.jar )

  ```shell
  #移动下载的jar包到idea-IU-183.5429.30/bin路径
  cd ~/下载
  sudo mv JetbrainsIdesCrack-4.2-release.jar /usr/idea/idea-IU-183.5429.30/bin
  
  #赋予权限
  cd /usr/idea/idea-IU-183.5429.30/bin
  sudo chmod 755 JetbrainsIdesCrack-4.2-release.jar
  ```

- 编辑idea-IU-183.5429.30/bin路径下的两个配置文件

  在两个文件末尾加入：

  ```shell
  #编辑idea.vmoptions配置文件
  sudo vim idea.vmoptions
  -javaagent:/usr/idea/idea-IU-183.5429.30/bin/JetbrainsIdesCrack-4.2-release.jar
  #点击ESC然后输入:wq保存并推出
  
  #编辑idea64.vmoptions配置文件
  sudo vim idea64.vmoptions
  -javaagent:/usr/idea/idea-IU-183.5429.30/bin/JetbrainsIdesCrack-4.2-release.jar
  #点击ESC然后输入:wq保存并推出
  ```

- 然后在idea-IU-183.5429.30/bin路径下的执行命令

  ```shell
  ./idea.sh
  ```

- 点击下一步，选择[激活码激活](http://idea.lanyus.com/)，并将激活码粘贴进去：

# 十、安装搜狗输入法

搜狗输入法官网下载[linux版本搜狗输入法](https://pinyin.sogou.com/linux/?r=pinyin)

```shell
#首先安装fcitx输入框架
sudo apt install fcitx
#解压安装
cd ~/下载
sudo dpkg -i sogoupinyin_2.2.0.0108_amd64.deb

#安装过程有错则运行如命令
sudo apt --fix-broken install
```

# 十一、安装MySQL-8

```shell
#卸载mysql
sudo apt remove mysql-*
#清除mysql的缓存
dpkg -l |grep ^rc|awk '{print $2}' |sudo xargs dpkg -P

```

### 1、Ubuntu安装mysql8

[官方链接](https://dev.mysql.com/downloads/file/?id=482330):选择最底下**No thanks, just start my download.**

然后更新镜像源

```shell
#切换到下载的目录
cd ~/下载
sudo dpkg -i mysql-apt-config_0.8.12-1_all.deb
```

下载mysql8

```shell
sudo apt update
sudo apt install mysql-server
```

新建数据库和用户

```sql
--创建数据库
create database victor;

--以下都是在root用户下执行
--创建用户victor
create user 'victor'@'localhost' identified by 'root';
--赋予victor数据库的所有权限
grant all privileges on victor.* to victor@localhost;
--删除用户
drop user victor;
```

注意：安装的Navicat无法连接，直接报错退出，可能是版本问题，导致Navicat已经不怎么维护Linux版本了。

所以选择MySQL官网的MySQL Workbench客户端来使用就行。

### 2、安装MySQL5.7

用sudo apt 镜像源装默认是MySQL5.7，而且mysql的控制台有中文显示bug。

```shell
#更新源
sudo apt-get update
#安装mysql
sudo apt-get install mysql-server
#配置mysql
sudo mysql_secure_installation
#检查mysql服务状态
systemctl status mysql.service
#登陆mysql
sudo mysql -u root -p
```

注意：使用Navicat，导致中文乱码，设置start_navicat文件将export LANG="en_US.UTF-8"改为export LANG="zh_CN.UTF-8"，保存,又会导致英文乱码。

```shell
sudo vim start_navicat
```

Navicat试用期到了，可以执行以下命令：

```shell
cd ~
rm -rf .navicat64
```



# 十二、安装Anaconda3和Tensorflow

[下载链接](https://mirrors.tuna.tsinghua.edu.cn/anaconda/archive/Anaconda3-2018.12-Linux-x86_64.sh)

#### 1、安装步骤

执行以下命令就行

```shell
wget https://mirrors.tuna.tsinghua.edu.cn/anaconda/archive/Anaconda3-2018.12-Linux-x86_64.sh
#赋予权限
chmod +x Anaconda3-2018.12-Linux-x86_64.sh 
#执行shell
sudo ./Anaconda3-2018.12-Linux-x86_64.sh
#设置安装路径
/usr/anaconda3
#最后提示要不要安装vscode选择no

#设置anaconda3的路径
export PATH=/usr/anaconda3/bin:$PATH
#路径加到$PATH就行,然后保存退出
source ~/.bashrc
#测试是否可以打开anaconda navigator
anaconda-navigator

#查看conda的版本
conda --version
#查看conda自带的包
conda list
#查看python版本
python --version

#删除conda创建的虚拟环境
conda remove -n tensorflow --all
#查看conda的虚拟环境
conda env list

#设置清华conda镜像
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main/
conda config --set show_channel_urls yes

#创建tensorflow的虚拟环境防止污染anaconda3
conda create --name [虚拟环境名] python版本
conda create --name tensorflow python=3.7

#激活tensorflow虚拟环境，进入到tensorflow虚拟环境
source activate tensorflow
#安装tensorflow
conda install tensorflow
```

#### 2、喜欢在spyder上编写tensorflow和运行

```shell
anaconda-navigator
选择sypder来运行就行。
```

在激活tensorflow的虚拟环境运行，使用spyder会报错，没有tensorflow模块等。是因为tensorflow的虚拟环境中，没有spyder。要先在tensorflow的环境中安装spyder等插件。

![](/home/victor/文档/Ubuntu安装软件手册/images/tensorflow虚拟环境.png)

要Ananconda navigator的Home中选择Applications on tensorflow,以及其他包。

![](/home/victor/文档/Ubuntu安装软件手册/images/查看tensorflow安装的包.png)

#### 3、tensorboard的使用

```shell
#切换到anaconda建立的tensorflow的虚拟环境
source activate tensorflow
tensorboard --logdir=/tmp/tmp_4sbz6pn/
```

# 十三、安装Node

#### 1、安装node

```shell
#更新源
sudo apt update
#安装nodejs
sudo apt install nodejs
#安装npm
sudo apt install npm
#验证版本
node -v
npm -v 
#设置淘宝镜像来使用cnpm
#因为利用npm下载，国内很慢。
sudo npm install -g cnpm --registry=https://registry.npm.taobao.org
#安装完成后，默认的淘宝镜像放在
/usr/local/lib/node_modules/cnpm/bin/cnpm
#测试cnpm
cnpm -v
```

#### 2、安装vue

```shell
#安装vue脚手架工具vue-cli
sudo cnpm install -g vue-cli
#测试vue的版本
vue -V
#创建vue项目
sudo vue init webpack vue-demo
一直回车，然后选择n就可以
#切换到vue-demo目录,安装项目依赖
sudo cnpm install
#然后运行项目
sudo npm run dev
```

#### 3、安装hexo

hexo init命令执行的时候卡死在Install Dependencies

可能还是由于使用默认的npm库，速度极慢。可以安装nrm

nrm是一个npm源管理器。

```shell
#使用cnpm初始化nrm包
cnpm install nrm -g
#切换到淘宝npm镜像
nrm use taobao
#新建一个文件夹
mkdir hexo
#安装hexo的脚手架
sudo npm install -g hexo-cli
#初始化hexo
hexo init
#生成静态页面
hexo g
#本地运行测试。
hexo s
#将public文件内容部署到github仓库
hexo d -g
```



# 十四、安装Eclipse

[下载链接](http://mirrors.neusoft.edu.cn/eclipse/technology/epp/downloads/release/2018-12/R/eclipse-jee-2018-12-R-linux-gtk-x86_64.tar.gz)

```shell
#usr路径下新建eclipse文件夹
cd /usr
sudo mkdir eclipse
#移动到/usr/eclipse路径
cd ~/下载
sudo mv eclipse-jee-2018-12-R-linux-gtk-x86_64.tar.gz /usr/eclipse/
#解压
sudo tar -zxvf eclipse-jee-2018-12-R-linux-gtk-x86_64.tar.gz
#进入eclipse文件目录
cd /usr/eclipse
#给解压后的eclipse文件夹赋予权限
sudo chown -R eclipse
#建立桌面启动项
cd /usr/share/applications
sudo gedit eclipse.desktop
#将下面内容复制到desktop文件里保存
#Exec路径是eclipse安装文件夹下的可执行文件
#Icon路径是图标路径
[Desktop Entry]
Encoding=UTF-8
Name=Eclipse
Comment=Eclipse IDE
Exec=/usr/eclipse/eclipse/eclipse
Icon=/usr/eclipse/eclipse/icon.xpm
Terminal=false
StartupNotify=true
Type=Application
Categories=Application;Development;
#赋予可执行权限
sudo chmod u+x eclipse.desktop
#复制到桌面
cp eclipse.desktop ~/桌面
```

# 十五、安装Tomcat

[下载链接](https://tomcat.apache.org/download-80.cgi)

```shell
#/usr路径下新建tomcat文件夹
cd /usr
sudo mkdir tomcat
#把下载的tomcat移动到新建文件夹
cd ~/下载
sudo mv apache-tomcat-8.5.37.tar.gz /usr/tomcat

cd /usr/tomcat
#解压
sudo tar -zxvf apache-tomcat-8.5.37.tar.gz
#赋予文件夹权限
sudo chmod 755 -R apache-tomcat-8.5.37
#到tomcat的bin路径下
cd /usr/tomcat/apache-tomcat-8.5.37/bin
#修改启动
sudo vim startup.sh
#加入jdk路径和tomcat路径
#set java environment
#JAVA PATH
export JAVA_HOME=/usr/java/jdk1.8.0_201
export JRE_HOME=${JAVA_HOME}/jre
export CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib
export PATH=.:${JAVA_HOME}/bin:$PATH
#tomcat
export TOMCAT_HOME=/usr/tomcat/apache-tomcat-8.5.37

#测试启动tomcat
./startup.sh
```

# 十六、安装Docker

```shell
#安装之前先更新
sudo apt update
sudo apt upgrade
sudo apt install docker.io
#启用docker
sudo systemctl start docker
sudo systemctl enable docker
docker -v
#查看已经pull的镜像
sudo docker image
```

#### 1、docker下的RabbitMQ拉取

```shell
#使用国内镜像加速安装unbuntu18.04镜像
sudo docker pull registry.docker-cn.com/library/ubuntu:18.04
#安装RabbitMQ
sudo docker pull registry.docker-cn.com/library/rabbitmq:3-management
#运行RabbitMQ
# -d:表示后台运行
# -p：表示暴露端口号
# --name myrabbitmq：指定名字
# a829a97a0435：对应的image id
sudo docker run -d -p 5672:5672 -p 15672:15672 --name myrabbitmq a829a97a0435
#查看进程
sudo docker ps
#访问RabbitMQ
localhost:15672
0.0.0.0:15672
#登录
用户名：guest
密码：guest
```

![安装RabbitMQ](/home/victor/文档/Ubuntu安装软件手册/images/docker安装RabbitMQ.png)

![docker的RabbitMQ运行](/home/victor/文档/Ubuntu安装软件手册/images/docker运行RabbitMQ.png)

#### 2、docker下的redis拉取

```shell
#下载Redis镜像
sudo docker pull registry.docker-cn.com/library/redis
#启动镜像
#redis默认的端口是6379，可以将虚拟机的6379映射到容器的6379
sudo docker run -d -p 6379:6379 --name myredis 0f55cf3661e9
```

![docker镜像命名冲突](/home/victor/文档/Ubuntu安装软件手册/images/docker名字冲突.png)

解决方法：

```shell
#查看docker的进程
sudo docker ps -a
#删除掉创建的镜像
#886183d7a2cc：container id，镜像id
sudo docker rm 886183d7a2cc
```

![](/home/victor/文档/Ubuntu安装软件手册/images/查看docker的进程ps.png)

```shell
#重新启动docker已经有的容器
sudo docker restart myrabbitmq
```

## 安装XMind8

[官网链接](https://www.xmind.net/xmind/download/)

- 解压压缩包

- 然后移动到/usr/xmind8

  ```shell
  cd /usr
  sudo mkdir xmind8
  cd ~/下载 
  sudo mv xmind-8-update8-linux /usr/xmind8/
  cd /usr/xmind8/xmind-8-update8-linux/
  #执行setup.sh文件，安装相关依赖。
  
  sudo /setup.sh 
  #百度下载一个xmind8的图片
  sudo mv xmind8.png /usr/xmind8/xmind-8-update8-linux/XMind_amd64
  
  #创建桌面快捷方式
  sudo vim /usr/share/applications/xmind8.desktop
  
  #编辑xmind8.desktop
  [Desktop Entry]
  Type=Application
  Path=/usr/xmind8/xmind-8-update8-linux/XMind_amd64
  Exec=/usr/xmind8/xmind-8-update8-linux/XMind_amd64/XMind
  Name=XMind 8
  Comment=Create mind maps
  GenericName=Planning Tool
  Icon=/usr/xmind8/xmind-8-update8-linux/XMind_amd64/xmind8.png
  Categories=Office
  
  cp /usr/share/applications/xmind8.desktop ~/桌面
  ```

  