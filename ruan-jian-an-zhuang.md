#### 服务器购买

推荐购买vultr的$5/月的东京服务器，强烈不推荐阿里云服务器。系统选择ubuntu 17.10 64位操作系统，文章中也会以该配置作为演示。

#### 软件安装

系统安装成功后，安装以下常用软件

`sudo apt-get install vim wget curl git`

#### Node环境配置

nvm安装

`wget -qO- https://raw.githubusercontent.com/creationix/nvm/v0.33.2/install.sh | bash`

打开新的窗口

`nvm install v8.9.1`

`nvm use 8.9.1`

`nvm alias default 8.9.1`

安装常用node包

`npm i pm2 webpack vue-cli -g`

