---
title: "使用Supervisor管理进程"
date: 2015-08-11 10:07:33
updated: 2017-08-24 18:03:00
categories: 编程之路
---
参考文章：<http://segmentfault.com/a/1190000002991175>(原文中还有使用OneAPM安装Python探针的应用，可以实时监控web应用数据，暂时还未实践)

supervisor是使用Python编写的进程管理软件，在实际开发中，一般用它来同时开始一批相关的进程，无论是Django的runserver还是直接管理Nginx、Apache等，都比较方便，这里是其使用方法：

## 安装

    # ubuntu
    apt-get install supervisor
    service supervisor restart
    
    # centos
    yum install supervisor
    /etc/init.d/supervisord restart

使用

    sudo easy_install supervisor
    echo_supervisord_conf > supervisord.conf  # 生成一个配置文件
    sudo supervisord -c supervisord.conf      # 使用该配置文件启动supervisord
    sudo supervisorctl                        # 进入命令行界面管理进程

## 设置一个进程

    # 在supervisord.conf里面添加如下内容
    [program:frontend]                                           # 进程名
    command=/usr/bin/python manage.py runserver 0.0.0.0:8000     # 启动该进程的命令
    directory=/media/sf_company/frontend/frontend                # 在执行上面命令前切换到指定目录
    startsecs=0
    stopwaitsecs=0
    autostart=false
    autorestart=false
    user=root
    stdout_logfile=/root/log/8000_access.log                     # 访问日志
    stderr_logfile=/root/log/8000_error.log                      # 错误日志

这样就创建了一个进程，进程的名称为frontend。

由于ubuntu上面supervisor的配置文件可以放在`/etc/supervisor.d/*.ini`里面比较方便，但是会出现一些错误。如果是单独的ini文件，那么不仅要写`program`这个section还应该把`supervisord`、`supervisorctl`两个区块都加上，哪怕不写任何东西。

## supervisorctl常用命令：

    start name    # 开始一个进程
    stop name    # 终止一个进程
    status   # 查看当前管理状态

### TroubleShooting

- **安装过程出现`unix:///var/run/supervisor.sock no such file`**:

  ```she
  # 首先删除通过apt-get安装的supervisor
  sudo apt-get remove supervisor
  # 然后把相应的进程kill掉
  sudo ps -ef | grep supervisor
  # 最后直接用easy_install安装
  sudo easy_install supervisor
  # 然后生成配置文件
  sudo echo_supervisor_conf > /etc/supervisord.conf
  # 最后启动
  sudo supervisord
  sudo supervisorctl
  ```

- **执行`sudo supervisorctl reload`**时出现错误`error: <class 'socket.error'>, [Errno 2] No such file or directory: file: /usr/lib64/python2.7/socket.py line: 224`原因是supervisor没有启动而重启造成的，我也不知道为什么报的错误会是这个错误。这时候只需要启动supervisor即可