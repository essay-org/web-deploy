#### 安装

官方文档[https://docs.mongodb.com/manual/tutorial/install-mongodb-on-ubuntu/](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-ubuntu/)

按照官方文档ubuntu版本号安装，按顺序执行以下命令：

* `sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 0C49F3730359A14518585931BC711F9BA15703C6`

* `echo "deb [ arch=amd64,arm64 ] http://repo.mongodb.org/apt/ubuntu xenial/mongodb-org/3.4 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.4.list`

* `sudo apt-get update`

* `sudo apt-get install -y mongodb-org`

* `sudo service mongod start`

完成以上操作，你的服务器已成功安装MongoDB

#### 初始数据的备份和导入

很多时候我们项目要上线，需要把本地的初始化数据导入到线上  
，步骤如下

首先在本地开启你的mongodb，并把数据备份到本地，比如说我要备份vueblog数据库到c:\vueblog-backup文件夹：

`mongodump -h 127.0.0.1:27017 -d vueblog -o C:\vueblog-backup`

备份出来的数据长这样

![](/assets/1027889-20170716140700113-1104261961.png)

通过ftp工具把数据上传到远程服务器，例如我上传到了/www文件夹下

接着在服务器导入数据到vueblog数据库  
`mongorestore -h 127.0.0.1:27017 -d vueblog /www/vueblog-backup/vueblog`

查看是否导入成功：

`mongo`

`use vueblog`

![](/assets/2017-12-01_180609.png)

