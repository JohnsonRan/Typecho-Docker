# Typecho-Docker

想用Typecho却不想在宿主机安装LNMP/LAMP之类的环境可以试试这个项目

## 环境
`php:8.0.10-fpm-alpine` , `caddy:latest` , `mariadb:latest` , `phpmyadmin:latest` .  
默认已安装 `pdo_mysql` 和 `pdo_sqlite` 扩展，可使用MySQL或SQLite

## 目录结构
```
├─app
│  └─php                // PHP 镜像
│          Dockerfile
│          run.sh
│
├─config
│  │  Caddyfile         // Caddy 配置文件
│  │
│  └─cert               // 自定义证书
│          cert.pem
│          key.pem
│
├─data                  // Typecho 源码目录及 SQLite 数据库
│  │  typecho.db
│  │
│  └─typecho
├─logs                  // 日志目录
└─docker-compose.yml
```

## 用法
从 [这里](https://github.com/typecho/typecho/archive/refs/heads/master.zip) 下载 Typecho 源码存放到 `data/typecho` (如果没有则新建一个)  
修改 `Caddyfile` 首行的 `domain.com` 为自己的域名  
## 注意
- **默认使用预编译的 `server-php` ，如想使用自己机器编译请自行修改 `docker-compose.yml` 第 `18` 行为**
```
build:
            context: app/php
``` 
- **Typecho 数据库地址请填写 `server-mariadb` 否则会连不上数据库**
- **MairaDB默认关闭外网访问，如想启用请删除 `docker-compose.yml` 第 `29` 行的 `127.0.0.1:` 并修改第 `41` 行的 `server-mariadb` 为自己服务器的公网IP**  
- **phpMyAdmin默认公网访问端口为 `8283` 如想更改请修改第 `39` 行的 `8283` 为想设置的端口**  
- **修改 `docker-compose.yml` 第 `31` 行的 `PMA_PASSWD` 为自己想设置的 MariaDB 密码**  
- **如果想使用 `Caddy` 获取的证书请删除 `Caddyfile` 里的 `tls /etc/caddy/cert/cert.pem /etc/caddy/cert/key.pem`**  
- **如果宿主机已安装 `nginx` , `apache` , `caddy` 等环境,请自行修改 `Caddyfile` , `docker-compose.yml` 中的端口**    

### 操作步骤:  
**请先按照 Docker 官方文档安装 Docker 以及 Docker-Compose:**  
```
https://docs.docker.com/engine/install/  
https://docs.docker.com/compose/install/  
```
### 随后:  
```
git clone https://github.com/JohnsonRan/Typecho-Docker
cd Typecho-Docker
git clone https://github.com/typecho/typecho ./data/typecho
docker-compose up -d
```
