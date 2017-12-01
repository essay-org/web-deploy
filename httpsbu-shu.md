#### 申请ssl证书

免费申请的机构有很多，我用的是腾讯云的，进入腾讯云官网，云产品的ssl证书管理，可免费申请。 ![](/assets/1027889-20170720222553255-1350255215.png)

申请后5分钟左右就会颁发证书，我们下载下来解压后长这样  

![](/assets/1027889-20170720222859974-537752330.png)

nginx证书长这样  

![](/assets/1027889-20170720225403990-2119602044.png)

### DNS解析

申请完证书官方有详细的部署教程，这里简单介绍。我们需要做dns解析![](/assets/1027889-20170720224628271-1605895131.png)

### 服务器部署ssl证书

前面我们已经成功安装了nginx，所以以部署nginx服务器的证书为例，首先建立一个ssl文件夹，把nginx证书放进去，然后通过ftp上传到www目录下，接着要配置nginx的配置文件  

```json
upstream vueblog {
    server 127.0.0.1:8080;
}

server {
    listen 80;
    # 修改为自己的域名
    server_name vueblog.86886.wang;
    # 301 重定向
    return 301 https://vueblog.86886.wang$request_uri;
}

server {
    listen 443;
    server_name vueblog.86886.wang;
    ssl on;
    # 证书路径不要写错
    ssl_certificate /www/ssl/1_vueblog.86886.wang_bundle.crt;
    ssl_certificate_key /www/ssl/2_vueblog.86886.wang.key;
    ssl_session_timeout 5m;
    ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
    ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:HIGH:!aNULL:!MD5:!RC4:!DHE;
    ssl_prefer_server_ciphers on;
    if ($ssl_protocol = "") {
    rewrite ^(.*) https://$host$1 permanent;
    }
    
    location / {
        proxy_set_header Host  $http_host;
        proxy_set_header X-Real-IP  $remote_addr;  
        proxy_set_header X-Forwarded-For  $proxy_add_x_forwarded_for;
        proxy_set_header X-Nginx-proxy true;
        # 这里也要修改为你的二级域名前缀
        proxy_pass http://vueblog;
        proxy_redirect off;
    }
}
```

把配置文件长传到etc/nginx/conf.d文件夹下，执行\`sudo nginx -s reload\`重启服务器，至此就完成了ssl证书的部署，并拥有301重定向功能

