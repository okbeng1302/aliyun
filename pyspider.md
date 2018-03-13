# pyspider environment

(重点：在使用阿里云 安装 pyspider 时 总是报各种错误，无论是CentOS 还是 Ubuntu，猜测是阿里云默认的python环境的问题，
所以在安装pyspider前，安装 anaconda
)

Anaconda安装  

1. 首先从官网下载 linux 版本，（分三个版本，我选择的第一个版本）

2. 下载的是sh文件， 使用命令  bash Anaconda.sh，就可以安装了，一路enter yes，就可以了

	执行命令 >> source ~/.bashrc


3. 运行python 发现 python是 anaconda 默认的环境。

pyspider  python2.7.12

查看官方文档，安装所需的依赖包 

但是在安装的过程中，发现总是报错，更新一下  apt-get update 就可以安装所有的包。

配置（想要将pyspider的taskdb、projectdb和resultdb放到本地路径下，但是看不明白官方文档，所以选择存放在 mongodb中了，具体的配置如下所示）：

config.json:

	{
	  "taskdb": "mongodb+taskdb://username:password@localhost:27017/pyspider",
	  "projectdb": "mongodb+projectdb://username:password@localhost:27017/pyspider",
	  "resultdb": "mongodb+resultdb://username:password@localhost:27017/pyspider",
	  "webui": {
		"port": 5555,
		"username": "username",
		"password": "password",
		"need-auth": true
	  }
	}

	
	
	
使用 supervisor 运行 pyspider

安装

apt-get install supervisor

输出配置文件

echo_supervisord_conf  >/etc/supervisord.conf

supervisord.conf:

[unix_http_server]

file=/tmp/supervisor.sock   ;UNIX socket 文件，supervisorctl 会使用

;chmod=0700                 ;socket文件的mode，默认是0700

;chown=nobody:nogroup       ;socket文件的owner，格式：uid:gid

;[inet_http_server]         ;HTTP服务器，提供web管理界面

;port=127.0.0.1:9001        ;Web管理后台运行的IP和端口，如果开放到公网，需要注意安全性

;username=user              ;登录管理后台的用户名

;password=123               ;登录管理后台的密码

[supervisord]
logfile=/tmp/supervisord.log ;日志文件，默认是 $CWD/supervisord.log

logfile_maxbytes=50MB        ;日志文件大小，超出会rotate，默认 50MB，如果设成0，表示不限制大小

logfile_backups=10           ;日志文件保留备份数量默认10，设为0表示不备份

loglevel=info                ;日志级别，默认info，其它: debug,warn,trace

pidfile=/tmp/supervisord.pid ;pid 文件

nodaemon=false               ;是否在前台启动，默认是false，即以 daemon 的方式启动

minfds=1024                  ;可以打开的文件描述符的最小值，默认 1024

minprocs=200                 ;可以打开的进程数的最小值，默认 200

[supervisorctl]
serverurl=unix:///tmp/supervisor.sock ;通过UNIX socket连接supervisord，路径与unix_http_server部分的file一致

;serverurl=http://127.0.0.1:9001 ; 通过HTTP的方式连接supervisord

; [program:xx]是被管理的进程配置参数，xx是进程的名称
[program:xx]
command=/opt/apache-tomcat-8.0.35/bin/catalina.sh run  ; 程序启动命令

autostart=true       ; 在supervisord启动的时候也自动启动

startsecs=10         ; 启动10秒后没有异常退出，就表示进程正常启动了，默认为1秒

autorestart=true     ; 程序退出后自动重启,可选值：[unexpected,true,false]，默认为unexpected，表示进程意外杀死后才重启

startretries=3       ; 启动失败自动重试次数，默认是3

user=tomcat          ; 用哪个用户启动进程，默认是root

priority=999         ; 进程启动优先级，默认999，值小的优先启动

redirect_stderr=true ; 把stderr重定向到stdout，默认false

stdout_logfile_maxbytes=20MB  ; stdout 日志文件大小，默认50MB

stdout_logfile_backups = 20   ; stdout 日志文件备份数，默认是10

; stdout 日志文件，需要注意当指定目录不存在时无法正常启动，所以需要手动创建目录（supervisord 会自动创建日志文件）

stdout_logfile=/opt/apache-tomcat-8.0.35/logs/catalina.out

stopasgroup=false     ;默认为false,进程被杀死时，是否向这个进程组发送stop信号，包括子进程

killasgroup=false     ;默认为false，向进程组发送kill信号，包括子进程

;包含其它配置文件

[include]

files = relative/directory/*.ini    ;可以指定一个或多个以.ini结束的配置文件


添加：
	[program:pyspider]
	
	command=pyspider -c /usr/local/pyspider/config.json
	
	autorestart=true
	
	autostart=true
	
	user=root
	
	directory=/usr/local/pyspider
	
	stderr_logfile=/usr/local/pyspider/log/pyspider_err.log
	
	stdout_logfile=/usr/local/pyspider/pyspider.log
	
	
	[program:scrapyd]
	
	command=scrapyd
	
	autostart=true
	
	autorestart=true
	
	stdout_logfile=/usr/local/scrapyd/log/scrapyd.log
	
	stderr_logfile=/usr/local/scrapyd/log/scrapyd_err.log
	
启动：
	supervisord -c /etc/supervisord.conf
	
	supervisorctl reload
	
	supervisorctl status
	
	supervisorctl stop pyspider
	
	supervisorctl start pyspider
	
	supervisorctl restart pyspider
	
	supervisorctl reread
	
	supervisorctl update
	
