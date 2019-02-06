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

![](/home/victor/桌面/Ubuntu安装软件手册/images/tensorflow虚拟环境.png)

要Ananconda navigator的Home中选择Applications on tensorflow,以及其他包。

![](/home/victor/桌面/Ubuntu安装软件手册/images/查看tensorflow安装的包.png)

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

