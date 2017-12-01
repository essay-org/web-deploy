#### 本地密钥生成

本地环境要和服务器环境基本保持一致，所以本地也要安装好git，node.js，pm2安装方法不在赘述

本地git bash执行`ssh-keygen -t rsa`，一路回车，完成后在你的用户目录下会生成一个.ssh文件夹，内容如下：

![](/assets/2017-12-01_181752.png)

复制id\_rsa.pub中的内容，登录github，在设置选项添加ssh key，这样你就可以本地提交你的代码到github了

#### 服务器密钥生成

服务器同本地一样，也是执行`ssh-keygen -t rsa`，一路回车

同样在github添加你的服务器密钥

执行`echo "[your public key]" > ~/.ssh/authorized_keys`将本机的公钥拷贝到服务器的authorized\_keys文件

完成以上操作你的本机就和服务器建立的联系，无需密码就可以操作服务器了

#### 项目部署

为了更好地说明部署细节，以我的开源项目vueblog的服务端部分作为演示，vueblog-server是一个由nodejs开发的api服务

在本地执行



