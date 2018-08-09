#### Nginx安装

`sudo apt-get install nginx`

通过nginx -v查看版本号

#### 代理设置

打开/etc/nginx/conf.d/文件夹，创建配置文件hello-8081.conf，内容如下：

```bash
# 这里是代理端口号 hello和8081根据你的情况配置
upstream hello {
    server 127.0.0.1:8081;
}

server {
    listen 80;
    # 域名配置
    server_name hello.example.com;

    location / {
        proxy_set_header Host  $http_host;
        proxy_set_header X-Real-IP  $remote_addr;  
        proxy_set_header X-Forwarded-For  $proxy_add_x_forwarded_for;
        proxy_set_header X-Nginx-proxy true;
        # 不要忘记这个模块的配置
        proxy_pass http://hello;
        proxy_redirect off;
    }
}
```



解释：配置文件类型必须以.conf结尾，文件名可自定义，为了方便记忆，遂以项目名加端口号的方式命名。

完成配置后，执行`sudo nginx -t`查看是否配置成功

![](/assets/2017-12-01_171923.png)
`sudo service nginx start` 启动服务
`sudo nginx -s reload`重启服务

#### 域名解析

解析你的域名到你的服务器ip，这样就可以通过访问`hello.example.com`代理服务器的8081端口，Nginx在这里的作用就是让你可以在一台服务器跑多个Node项目。

