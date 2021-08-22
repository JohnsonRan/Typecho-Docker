# Typecho-Docker

想用Typecho却不想在宿主机安装LNMP/LAMP之类的环境可以试试这个项目

## 环境
`php:8.0.9-fpm-alpine` , `caddy:latest`.(问我为什么没有mysql之类的数据库？您好，小鸡带不动  
默认已安装`pdo_mysql` 和 `pdo_sqlite` 扩展，可使用MySQL或SQLite

## 目录结构
```
├─app
│  └─php                // PHP 镜像
│          Dockerfile
│
├─config
│  │  Caddyfile         // Caddy 配置文件
│  │
│  └─cert               // 自定义证书
│          cert.pem
│          key.pem
│
├─data                  // Typecho 源码目录及SQLite数据库
│  │  typecho.db
│  │
│  └─typecho
└─logs                  // 日志目录
└─docker-compose.yml
```

## 用法
从 [这里](https://github.com/typecho/typecho/archive/refs/heads/master.zip) 下载 Typecho源码存放到 `data\typecho` (如果没有则新建一个)  
修改 `Caddyfile` 首行的 `domain.com` 为自己的域名  
**如果想使用 `Caddy` 获取的证书请删除 `Caddyfile` 里的 `tls /etc/caddy/cert/cert.pem /etc/caddy/cert/key.pem`**  
**如果宿主机已安装 `nginx` , `apache` , `caddy` 等环境,请自行修改`Caddyfile` , `docker-compose.yml` 中的端口**  

执行步骤:
```
git clone https://github.com/JohnsonRan/docker-typecho
cd docker-typecho
git clone https://github.com/typecho/typecho ./data/typecho
rm -rf ./data/typecho/.git
docker-composed up -d
