#### 本地密钥生成

本地环境要和服务器环境基本保持一致，所以本地也要安装好git，node.js，pm2安装方法不在赘述

本地git bash执行`ssh-keygen -t rsa`，一路回车，完成后在你的用户目录下会生成一个.ssh文件夹，内容如下：

![](/assets/2017-12-01_181752.png)

复制id\_rsa.pub中的内容，登录github，在设置选项添加ssh key，这样你就可以本地提交你的代码到github了

#### 服务器密钥生成

服务器同本地一样，也是执行`ssh-keygen -t rsa`，一路回车

同样在github添加你的服务器密钥

完成以上操作后在服务端执行`echo "[your public key]" > ~/.ssh/authorized_keys`该命令会将本机的公钥拷贝到服务器的authorized\_keys文件，可打开root/.ssh/authorized\_keys，查看是否拷贝成功

#### 设置文件和目录权限

设置authorized\_keys权限:`$ chmod 600 authorized\_keys`

设置.ssh目录权限:`$ chmod 700 -R .ssh`

完成以上操作你的本机就和服务器建立的联系，无需密码就可以操作服务器了，这个权限设置很重要

#### 项目部署

为了更好地说明部署细节，以我的开源项目vueblog的服务端部分作为演示，vueblog-server是一个由nodejs开发的api服务

在本地项目目录下执行`pm2 ecosystem`，然后会在根目录下生成一个`ecosystem.config.js`文件

```js
module.exports = {
  /**
   * Application configuration section
   * http://pm2.keymetrics.io/docs/usage/application-declaration/
   */
  apps : [

    // First application
    {
      // 项目名称
      name      : 'vueblog-server',
      // 入口文件
      script    : 'server.js',
      env: {
        COMMON_VARIABLE: 'true'
      },
      env_production : {
        NODE_ENV: 'production'
      }
    }
  ],

  /**
   * Deployment section
   * http://pm2.keymetrics.io/docs/usage/deployment/
   */
  deploy : {
    // 生产环境
    production : {
      // 服务器用户名
      user : 'root',
      // 服务器IP
      host : '198.13.32.165',
      ref  : 'origin/master',
      // github上项目的地址
      repo : 'git@github.com:wmui/vueblog-server.git',
      path : '/www/vueblog-server',
      'post-deploy' : 'npm install && pm2 reload ecosystem.config.js --env production'
    },
    // 开发环境
    dev : {
      user : 'root',
      host : '198.13.32.165',
      ref  : 'origin/master',
      repo : 'git@github.com:wmui/vueblog-server.git',
      path : '/www/vueblog-server',
      'post-deploy' : 'npm install && pm2 reload ecosystem.config.js --env dev',
      env  : {
        NODE_ENV: 'dev'
      }
    }
  }
};
```

以上有注释的部分，你要根据你自己的项目进行修改，完成修改后，提交到github

第一次你先要在你的服务器上clone你的项目，我克隆到了/www目录下，cd到该目录，服务端执行`pm2 deploy ecosystem.config.js production setup`初始化项目

紧接着重点来了，打开你的root目录下的.baserc文件，把最底部的以下两行代码移动到最上面，这应该是一个坑

```
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm
```

然后在本地的git bash中测试，修改一些东西，提交到github，执行`pm2 deploy ecosystem.config.js production`你会看到你下内容，恭喜成功，服务端会自动执行安装启动等操作

![](/assets/1.png)

[pm2](https://github.com/Unitech/pm2)有很多强大的功能，比如我们项目部署到线上后发现有问题，可以执行`pm2 deploy ecosystem.config.js production revert 1`回滚到上一个版本，更多强大功能可查看官方文档

