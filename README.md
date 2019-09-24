# leyou-manage-web

> A Vue.js project

## Build Setup

``` bash
# install dependencies
npm install

# serve with hot reload at localhost:8080
npm run dev

# build for production with minification
npm run build

# build for production and view the bundle analyzer report
npm run build --report
# 域名设置
进入C:\Windows\System32\drivers\etc 修改hosts文件
127.0.0.1 manage.leyou.com
127.0.0.1 api.leyou.com

# nginx 配置文件

#user  nobody;
worker_processes  1;

#error_log  logs/error.log;
#error_log  logs/error.log  notice;
#error_log  logs/error.log  info;

#pid        logs/nginx.pid;


events {
    worker_connections  1024;
}


http {
    include       mime.types;
    default_type  application/octet-stream;


    #access_log  logs/access.log  main;

    sendfile        on;
   
    keepalive_timeout  65;

   

    server {
        listen       80;
        server_name  localhost;

        location / {
            root   html;
            index  index.html index.htm;
        }

       
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }

       
    }
	server {
        listen       80;
        server_name  manage.leyou.com;
		proxy_set_header X-Forwarded-Host  $host;
		proxy_set_header X-Forwarded-Server  $host;
		proxy_set_header X-Forwarded-For  $proxy_add_x_forwarded_for;
		
        location / {
            proxy_pass http://127.0.0.1:9001;
			proxy_connect_timeout 1000;
			proxy_read_timeout 1000;
        }

       
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }

       
    }
	server {
        listen       80;
        server_name  api.leyou.com;
		proxy_set_header X-Forwarded-Host  $host;
		proxy_set_header X-Forwarded-Server  $host;
		proxy_set_header X-Forwarded-For  $proxy_add_x_forwarded_for;
		
        location / {
            proxy_pass http://127.0.0.1:10010;
			proxy_connect_timeout 1000;
			proxy_read_timeout 1000;
        }

       
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }

       
    }


}


```


## npm install  失败
```
问题描述：
Unexpected end of JSON input while parsing near '…"

解决办法：
（1）npm install --registry=https://registry.npm.taobao.org --loglevel=silly
（2） npm cache clean --force
（3） npm install
```


