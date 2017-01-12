---
title: "Linux 教程"
date: 2013-09-08 11:02:30
updated: 2016-09-05 23:40:00
categories: system
---
# Linux指南

## 基础安装

    # CentOS
    sudo yum install epel-release

## 命令行Tips
#### 进程及端口

```shell
# 查看端口占用情况
netstat -ap | grep 端口号   # 查看某一个端口
netstat -ntlp
top -p 进程ID：查看进程的实时情况，包括内存大小，内存占用率、CPU占用率，运行时间
cat /proc/进程ID/status：查看进程详细信息，包括线程数，线程名称，线程状态，占用内存大小
pstree -p 进程ID：查看线程的进程数以及进程ID
lsof -i :端口号   # 查看端口占用情况，不仅能看到哪个进程开启的端口，还能查看谁在使用该端口
lsof -i -n -P | egrep ':8000.+ESTABLISHED'   # 查看8000端口的连接列表
lsof -i -n -P | egrep -c ':8000.+ESTABLISHED' # 查看8000端口的连接数字

# 结束进程
kill -s 9 进程ID
```

#### 查找与统计

    ls -lR | grep "^-" | wc -l # 递归统计文件夹下所有文件的个数
    wc -l: 统计行数
    grep -c "词语"   # 统计出现的次数
    grep 字符串 文件名  # 在文件中查找某个字符串
    grep ^字符串 文件名 # 在文件中查找以某字符串开始的行
    grep [0-9] 文件名  # 在文件中查找包含数字的行
    grep 字符串 -r 目录 # 在特定目录及其子目录中的文件查找str
    fdupes：快速查找重复文件
    find / -name filename	# 精确查找某个文件
    find / -name '*.txt'	# 模糊查找某个文件
    find / -mmin -60    # 查找60分钟内修改的文章
    find / -type d -mtime -1 # 查找1天内修改过的文件夹(好吧，我用了rm -rf / 命令才知道的)

#### 文件操作

```shell
# 压缩
tar -czvf 结果.tar.gz 目标/    # 打包并使用gzip压缩
tar -cjvf 结果.tar.bz2 目标/   # 打包并使用bzip2压缩
zip *.zip file          # 压缩file为zip格式
zip -r *.zip file dir   # 压缩文件或目录一起为zip格式
zip -e 结果.zip 目标     # 压缩并加密(OSX可用)

# 解压
xz -d *.tar.xz
tar xvf *.tar
tar zxvf *.tgz
tar -xjf tar.bz2   # 解压bz2文件
gunzip *.gz     # 解压gz文件
tar -xzf *.tar.gz

# 复制
cp 文件1 文件2
cp -r 目录1 目录2  # 递归复制
cp -a 目录1 目录2  # 递归复制目录，同时将文件属性也复制过去

# 文件分割
split -b 1024m   # 文件分隔-b表示按大小分隔，-l表示按行数分隔

# 查看文件内容
cat filename | more  # 表示分页查看文件内容

# 输出内容到文件
cat ./test.conf >> /etc/supervisord.conf
sudo bash -c 'cat ./test.conf >> /etc/supervisord.conf'  # 上一句如果出现权限问题可以尝试使用这条命令

# 建立链接，最好都用绝对路径
软连接：ln -s 源 目的地
软连接可以给目录创建，如果删除了对源文件不会有影响
硬连接：ln -d 源 目的地
硬连接不能给目录创建，对连接做的更改会影响源文件，只能在同一文件系统中创建

# 文件创建
mkdir -p path/2 # 创建目录树
mkdir -pv path/{path1,path2} # 建立子目录
mkdir -v a+wt path	# 创建一个粘滞模式的文件，其他用户可以修改，但是只有该文件的owner才能进行删除操作，这条命令即使把0755(rwxr-xr-x)改为1777(rwxrwxrwt)

# 找不同
diff 文件1 文件2   # 找出两个文件的不同
sdiff 文件1 文件2  # 以对比的方式找文件的不同

# 批量转换文件编码
find *.txt -exec sh -c "iconv -f GBK -t UTF8 {} > change.{}" \;	# 这里将GBK转换为UTF8
```

#### 远程ssh

    # 配置免密码登录
    ssh-keygen -t dsa # 生成自己的ssh，然后将~/.ssh/id_dsa.pub的内容添加到主机的~/.ssh/authorized_keys里面面去
    
    # CentOS下的安装
    yum install openssh-clients
    
    # 传输文件
    scp 用户名@地址:远程路径 本地路径  # 获取/下载远程服务器的文件，目录加-r参数
    scp 本地路径 用户名@地址:远程路径  # 将本地文件上传到远程目录，目录加-r
    
    # 仅允许SSH登录，vim /etc/ssh/sshd_conf
    PubkeyAuthentication yes
    AuthorizedKeysFile .ssh/authorized_keys
    PasswordAuthentication no
    
    #保存，然后重启ssh服务
    service sshd restart
    
    # 进制特定IP登录，vim /etc/hosts.deny
    sshd:IP
    
    # 登录shell和非登录shell的区别: 加载的文件不同，登录式shell加载/etc/profile、/.bash_profile和~/.profile，而非登录式shell加载/etc/bashrc或者/etc/bash.bashrc、~/.bash_rc，所以在切换用户是最好加上-，即su - haofly就切换到那个心的地方了

#### 包管理

    dpkg -i *.deb # 安装deb包，但是它不会自动解决依赖，安装完成后还要使用apt-get -f install这条命令来安装没有安装好的依赖
    apt-cache show 包名 # 显示apt库里面的软件的版本号
    sudo apt-get clean # 自动清理安装程序时缓存的deb包
    sudo apt-get autoclean  # 清理已卸载软件的无用的依赖包
    rpm -ql 包名	# 查询已经安装的包的文件路径

#### 磁盘管理

```shell
sudo fdisk -lu   # 显示硬盘及分区情况
sudo fdisk /dev/sdb # 对某一硬盘进行分区(千万不要在当前硬盘进行分区)
sudo mkfs -t ext4 /dev/sdb   # 将硬盘格式化为ext4文件系统
sudo df -lh   # 显示硬盘挂载情况
sudo mount -t ext4 /dev/sdb /mydata  # 挂载某个分区文件为ext4
vim /etc/fstab中添加
UUID=硬盘的UUID  /挂载位置   ext4 defaults 0  0   # 在系统启动时自动挂载硬盘blkid /dev/sda1  查看硬盘UUID用sudo blkid

sudo du -h -d 1 /path	# 获取指定目录下一级的各个目录的大小
```

#### 用户管理

    # 添加用户
    sudo useradd -s /bin/bash -d /home/username -m username
    
    # 修改用户密码
    sudo passwd username
    
    # 给用户添加sudo权限
    vim /etc/sudoers 修改如下内容
    # User privilege specification  
    root    ALL=(ALL:ALL) ALL      # 在这一行下面写  
    username1 ALL=(ALL:ALL) ALL    # 该用户可以执行所有sudo操作
    username2 ALL=NOPASSWD:/usr/bin/git # 该用户可以执行'sudo git'的操作
    
    # 查看所有用户
    cat /etc/passwd  

#### 系统相关

```shell
lsb_release -a       // 查看系统信息
echo $HOSTTYPE     // 查看系统位数
cat /proc/cpuinfo    # 查看CPU信息
cat /etc/issue     // Debian系列查看系统版本
cat /etc/redhat-release // redhat系列查看系统版本

# 更新系统
sudo apt-get install update-manager-core
sudo do-release-upgrade

# 系统开关
shutdown -h now # 关机
shutdown -r now # 重启

# 查看某个命令的系统调用
strace + 命令: 这条命令十分强大，可以定位你程序到底是哪个地方出了问题

# 增加虚拟内存
sudo dd if=/dev/zero of=/swapfile bs=1024 count=500000  # 500MB，这两行是连在一起的，我日
sudo mkswap /swapfile
sudo chown root:root /swapfile
sudo chmod 0600 /swapfile
sudo swapon /swapfile
```

#### 防火墙

    service iptables status     # 查询防火墙状态
    vim /etc/sysconfig/iptables # 新增端口
    service iptables restart    # 重启防火墙

## 其它工具

#### supervisor

进程监控工具，`apt-get install supervisor`进行安装，默认的监控配置都放在`/etc/supervisor/conf.d`里面，配置文件语法如下:

	[group:fenzu]
	programs:一个进程名,另一个进程名	# 这样可以分组控制一批program
	
	[program:去一个进程名称]
	process_name=%(program_name)s_%(process_num)02d # 当前进程的名称
	directory=/home/...     # 工作目录，启动程序前会切换到这个地方
	command=python manage.py runserver ....   # 启动命令
	autostart=true				# 在supervisord启动的时候自动启动
	autorestart=true			# 程序异常退出后自动重启
	startretries=3 			# 启动失败自动重试次数，默认是3
	user=root					# 用哪个用户启动
	numprocs=8					# 进程数
	redirect_stderr=true		# 把stderr重定向到stdout，默认为false
	stdout_logfile=/var/log/...	# 日志文件位置，若该目录不存在则无法正常启动，需要手动创建目录
常用操作

	supervisord	# 启动所有监控			
#### CURL

```shell
curl -o a.txt url	# 将文件下载到本地并命名为a.txt
curl -O url			# 将文件下载到本地用它本来的命名
```

#### FTP

#### Crontab定时任务

```shell
crontab -e # 直接打开定时任务文件进行编辑
格式如下：
第1列：分钟
第2列：小时
第3列：日
第4列：月
第5列：星期
第6列：命令
其中，每一列可以逗号和小横线表示特殊的意义，比如
3,15 8-11 * * * 命令   # 表示在上午8点到11点的第3和15分钟执行
* 23 * * * 命令			# 注意这个表示的是23点的每分钟都执行
0 */1 * * * 命令			# 每隔一小时
需要注意的是coontab是不会自动加载环境变量的哟，所以有时候发现命令没有被执行，可能是这个原因

# crontab日志，默认是关闭的，如果要打开可以在配置文件里面进行打开,vim /etc/rsyslog.d/50-defaullt.conf
cron.*	/var/log/cron.log	# 将cron前面的注释去掉
service rsyslog restart		# 重启rsyslog
```

**其他命令**

	cd -: 返回上一次的目录，真他妈实用
	history：查看历史命令，如果需要查看命令执行时间，需要先export HISTTIMEFORMAT='\%F \%T '
	tzselect：更改时区
	# 命令命名，例如如果想通过python命令调用python3而不是默认的python2，那么可以这样子：
	alias python=python3
	alias pip=pip3
	alias run8000='python manage.py runserver 0.0.0.0:8000'
	
	# yes命令：重复输出字符串，不带参数则默认输出y。例如 `yes | apt-get install xxx`会默认输出y
	
	# 网络相关
	ifdown eth0 # 禁用eth网卡
	ifup eth0
	
	# 随机数
	echo $RANDOM
	
	# 除法
	echo $RANDOM / 28 | bc
	echo $RANDOM % 28 | bc

## Shell Script
**数据结构**

	VAR2=${VAR:-haofly}	# 如果变量VAR不存在，后面就是它的默认值
	VAR2=${VAR/.tar.gz}	# 如果VAR的值为haofly.tar.gz，那么VAR2=haofly，一种替换
	length=$(#array[@]}或者length=$(#array[*]} # 获取数组长度

**流程控制**

	if语句：
		-z：为空
		-n：不为空
		-gt：大于
		
	# 判断文件是否存在
	if [ ! -f "$filename" ]; then
	touch "$filename"
	fi
	
	# 判断文件是否为空
	if [[ ! -s filename ]]; then
	echo 'a'
	fi

**特殊符号**

	[[]]：双中括号，之间的字符不会发生文件名扩展或者单词分割
	(())：双小括号，整数扩展，其中的变量可以不适用$符号前缀
	$?：上一条命令的退出码

**日期处理**

	date +"%s"	# 按照时间戳来显示
	date +"%m-%d-%y"	# mm-dd-yy格式
	date +"%T"	# 仅显示时间，比如10:44:00

**随机数**
	$RANDOM	# 生成一个随机数

**特殊操作**
	. /etc/*.conf		# 导入配置文件，这样配置文件里面的变量就可以直接使用了

	find ./ -name "*.log" -mtime -1 | which read line; do tail -n 5 "$line" > ~/bak/"$line"; done # 查找，然后按行进行执行
	while read line do 语句 done  # 一行一行地进行处理，真正的处理
	
	# awk：以行为单位将输入进行处理，貌似这里的处理智能进行print
		-F 参数将行做分割，例如：ps | awk -F ' ' '\{print $1\}'  # 将ps的第二列输出
	
	# sed：同样以行为单位将输入进行处理
	    -n 输出第几行，例如：ps | sed -n '1p'  # 将ps的第一行输出
	
	# xargs：将上一个管道的输出直接作为这个管道的输入
	    ps | grep python | awk -F ' ' '\{print $1\}' | xargs kill
	
	date+\%Y-\%m-\%d   # 获取今天的日期


# TroubleShooting
- **Linux下笔记本触摸板无法使用的问题**  
  我的电脑是宏碁E1-471G, ubuntu13.04，以下几个方法依次尝试  
  方法一：Fn + F7  
  方法二：Ctrl + Fn + F7  
  方法三：执行命令`sudo modprobe -r psmouse`然后`sudo modprobe psmouse proto=imps`同时，为了开机有效，新建文件/etc/modprobe.d/options，添加代码`options psmouse proto=imps`

- **Ubuntu无法调节屏幕亮度**  
  方法一：  
  编辑/`etc/default/grub`文件，将  
  `GRUB_CMDLINE_LINUX=""`修改为`GRUB_CMDLINE_LINUX="acpi_backlight=vendor"`  
  然后执行`sudo update-grub`更新grub  
  然后编辑`/etc/rc.local`文件，在exit 0这行代码之前加上`echo 100 > /sys/class/backlight/intel_backlight/brightness`  
  再重启    
  方法二：  
  此方法来自[IT之家](http://www.ithome.com/html/soft/71265.htm)  
  编辑文件`/usr/share/X11/xorg.conf.d/20-intel.conf`添加如下代码：

  Section "Device"
  Identifier "card0"
  Driver "intel"
  Option "Backlight" "intel_backlight"
  BusID "PCI:0:2:0"
  EndSection
  最后重启

- **Linux内核的更新**  
  **CentOS**：[CentOS 6.5 升级内核到 3.10.28](http://cn.soulmachine.me/blog/20140123/)
  **Ubuntu**：[使用dpkg安装ubuntu内核](http://blog.skyx.in/archives/216/)

- **Deepin升级时出现"这种情况的原因可能是本地依赖已经损坏"**  
  刚装上Deepin(2014.2)发现在应用商店升级时，有3个安装包无法升级(使用apt-get也提示无法升级)，原因可能是下载的包的依赖关系发生改变而引起的，解决方法如下：  
  执行以下命令对这三个不能更新的包进行更新：

      sudo apt-get clean    清除已经下载的安装包
      sudo apt-get update   重新更新软件列表
      sudo apt-get dist-upgrade	# 与upgrade不同，dist-upgrade能够忽略Packages中的相关性问题进行强制升级，在依赖关系前后发生改变后也仍然会进行升级。


- **XShell连接FTP出现`The data connection could not be established: ETIMEDOUT - Connection attempt timed out`错误**

  在连接管理里面讲当前ftp连接类型设置为普通ftp`only use plain FTP (insecure)`

- **Linux网卡设置**  
  查看网卡设置：

      # ifconfig
      enp2s0    Link encap:以太网  硬件地址 fc:aa:14:4e:ed:18    
                inet 地址:192.168.1.41  广播:192.168.1.255  掩码:255.255.255.0  
                inet6 地址: fe80::feaa:14ff:fe4e:ed18/64 Scope:Link  
                UP BROADCAST RUNNING MULTICAST  MTU:1500  跃点数:1  
                接收数据包:182927 错误:0 丢弃:0 过载:0 帧数:0  
                发送数据包:89825 错误:0 丢弃:0 过载:0 载波:0  
                碰撞:0 发送队列长度:1000   
                接收字节:203392327 (203.3 MB)  发送字节:7662359 (7.6 MB)  
      
      lo        Link encap:本地环回    
                inet 地址:127.0.0.1  掩码:255.0.0.0  
                inet6 地址: ::1/128 Scope:Host  
                UP LOOPBACK RUNNING  MTU:65536  跃点数:1  
                接收数据包:7423 错误:0 丢弃:0 过载:0 帧数:0  
                发送数据包:7423 错误:0 丢弃:0 过载:0 载波:0  
                碰撞:0 发送队列长度:0   
                接收字节:628267 (628.2 KB)  发送字节:628267 (628.2 KB)  

一般lo表示本地还回，eth0表示以太网卡，wlan0表示无线网，但是有时候非常奇葩，比如我这里这台服务器的以太网卡叫enp2s0，我树莓派上的网卡叫什么wl0  
静态网络设置:

    **# vim /etc/network/interfaces  # 这里仅设置eth0网卡  
    auto eth0  
    iface eth0 inet static  
    address 192.168.1.100  
    netmask 255.255.255.0  
    gateway 192.168.1.1  
    
    # vim /etc/resolv.conf  # 不知道为什么有次设置了上面的还是无法联网，再修改DNS设置就行
    nameserver 192.168.1.1  # 这里加一行，这里的IP是我路由的IP，如果重启后又不能联网了，可能是这个文件又复原了。。。  
    
    # 重启网卡  
    /etc/init.d/networking restart**

WIFI网络设置

    **$ vim /etc/network/interfaces  # 这里设置wlan0我那个卡
    auto wlan0
    allow-hotplug wlan0
    iface wlan0 inet dhcp
    wpa-ssid WIFI名称
    wpa-psk WIFI密码  
    
    # 重启网卡  
    sudo ifdown wlan0 && sudo ifup wlan0
    **

- **Virtualbox虚拟机网卡丢失**  
  一般发生在采用以前的vdi新建虚拟机之后，发现只有lo网卡，eth0网卡丢失，修复过程如下：

      # 首先使用如下命令查看可用的网卡
      ifconfig -a  # 这不仅会显示lo网卡，还会显示其它可用的网卡，如eth0，名字不固定，比如我这次是eth1
      
      # 记住上面以太网卡的名字，然后执行
      sudo dhclient eth0   # 将该网卡打开
      
      # 当然很有可能在网络设置里面没有该网卡，导致重启后依然无法联网的情况，我的网卡设置里就是eth0但现在应该变为eth1了，参考上面的/etc/network/interfaces文件

- **apt-get update时出现Unknown error executing gpgv等问题** 

      cd /usr/local/lib/
      mv libreadline* /temp
      ldconfig
      apt-get update

- **CentOS安装Nginx/Tengine出现"configure: error: the HTTP rewrite requires the PCRE library"**

    	yum -y install pcre-devel openssl openssl-devel

- **全局中文utf8环境**

      # CentOS
      localedef -i zh_CN -c -f UTF-8 zh_CN.utf-8
      export LANG=zh_CN.UTF-8
      然后全局生效,vim /etc/locale.conf
      LANG=zh_CN.UTF-8
      LC_COLLATE=zh_CN.UTF-8

- **免密码登录仍然要求输入密码**

      # 权限问题
      chmod 700 ~/.ssh
      chmod 600 ~/.ssh/authorized_keys

- **CentOS7 无法使用命令netstat，nmap**  
  原因是CentOS7抛弃了这几个老旧的命令，使用新的命令进行[替代](https://dougvitale.wordpress.com/2011/12/21/deprecated-linux-networking-commands-and-their-replacements/#netstat)了，如果要使用那几条命令，可以`yum install net-tools`

- **安装FTP/VSFTP**  

  ```shell
  	sudo yum install vsftpd
  sudo vim /etc/vsftpd/vsftpd.conf 修改如下几项：
  anonymous_enable=NO
  local_enable=YES
  chroot_local_user=YES
  service vsftpd restart
  chkconfig vsftpd on 	# 开机启动

  # 创建用户
  sudo adduser ftpuser
  sudo passwd ftpuser

  # 常用命令
  ftp domain/ip	# 连接目标ftp服务器
  put a.txt		# 上传当前目录的一个文件
  mput ./*		# 同时上传多个文件
  ```

- **CentOS出现:"cannot find a valid baseurl for repo"**  
  CentOS minimal默认是没有开启网卡的，需要将`vim /etc/sysconfig/network-scripts/ifcfg-eth0
  `中的`ONBOOT=no`修改为`ONBOOT=yes`，然后执行`dhclient`，再重启就可以了

- **make: yacc: Command not found**  

  	sudo apt-get install bison -y
  	sudo apt-get install byacc -y

- **/bin/bash: line 9: makeinfo: command not found**

  	sudo apt-get install texinfo

- **g++: command not found**

  	yum install gcc-c++


- `Too many levels of symbolic links `

  出现这个错误是由于在创建软链接的时候使用的是相对路径，重新用绝对路径创建链接即可

- **ssh 出现"Too many authentication for "root""**
  连接的时候加一个参数:`ssh -o PubkeyAuthentication=no root@...`

- ​