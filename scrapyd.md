# scrapyd

安装：
	pip install scrapyd
	
	pip install scrapyd-client
	
配置文件：

	默认 /etc/scrapyd/scrapyd.conf 
	
	
	[scrapyd]
	eggs_dir = /usr/local/scrapyd/eggs
	
	logs_dir = /usr/local/scrapyd/logs
	
	jobs_to_keep = 100
	
	dbs_dir = /usr/local/scrapyd/dbs
	
	max_proc = 0
	
	max_proc_per_cpu = 800
	
	finished_to_keep = 100
	
	poll_interval = 5.0
	
	bind_address = 127.0.0.1
	
	http_port = 6800
	
	user=user
	
	pwd=pwd
	
	debug = off
	
	runner = scrapyd.runner
	
	application = scrapyd.app.application
	
	launcher = scrapyd.launcher.Launcher
	
	webroot = scrapyd.website.Root
	
	[services]
	
	schedule.json     = scrapyd.webservice.Schedule
	
	cancel.json       = scrapyd.webservice.Cancel
	
	addversion.json   = scrapyd.webservice.AddVersion
	
	listprojects.json = scrapyd.webservice.ListProjects
	
	listversions.json = scrapyd.webservice.ListVersions
	
	listspiders.json  = scrapyd.webservice.ListSpiders
	
	delproject.json   = scrapyd.webservice.DeleteProject
	
	delversion.json   = scrapyd.webservice.DeleteVersion
	
	listjobs.json     = scrapyd.webservice.ListJobs