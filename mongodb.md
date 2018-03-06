# mongodb

工具：xshell xftp


1. 首先从官网下载mongodb linux 64位 版本 压缩包

2. 在云服务器 /usr/local 下建立文件夹  mongo 路径 /usr/local/mongo

3. 使用 xftp 将压缩包传到云服务器中的 mongo 文件夹中

4. 使用 tar -zxvf mongo-*-.tar.gz  解压

5. 重命名文件夹  mongodb

6. 建立 mongo 数据存储文件夹data 路径 /usr/local/mongo/data
	datapath=/usr/local/mongo/data/data
	logpath=/usr/local/mongo/data/log

7. 在 /etc 中建立 mongodb配置文件

	/etc/mongodb.conf
	内容如下：
		port=27017 # mongo端口
		dbpath=/usr/local/mongo/data/data # 数据存储路径
		logpath=/usr/local/mongo/data/log # 日志存储路径
		fork=true
		logappend=true
		auth=true # 需要账号密码验证
		bind_ip=0.0.0.0 # 远程登录时，需要使用


8. 运行mongo

	进入到 bin 路径下 ./mongod --config /etc/mongodb.conf

9. 进入到 mongo 中  ./mongo

10. 用户权限设置 

	添加用户，默认使用哪个db,就为哪个db添加用户。

	例如：

		use admin # admin数据库 存户用户名密码

		db.createUser({user:"admin",pwd:"1234",roles:[{role:"root",db:"admin"}]})
		其中，root 表示用户对本数据库有超级管理员权限

		root 超级管理员
		userAdminAnyDatabase 分配角色和用户的权限，没有查写的权限
		readWrite 读写权限
		read 读权限


11. 关闭数据库
	使用admin
	use admin
	db.auth("admin","1234")
	db.shutdownServer()

