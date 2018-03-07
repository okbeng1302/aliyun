# pyspider environment

pyspider  python2.7.12

查看官方文档，安装所需的依赖包 

但是在安装的过程中，发现总是报错，更新一下  apt-get update 就可以安装所有的包。

配置（想要将pyspider的taskdb、projectdb和resultdb放到本地路径下，但是看不明白官方文档，所以选择存放在 mongodb中了，具体的配置如下所示）：

confid.json:

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

	
