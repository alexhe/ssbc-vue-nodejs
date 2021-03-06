# ssbc
手撕包菜网站

## 安装说明

使用CentOS7操作系统。

### 数据库 ###
1. 安装MongoDB

```
yum install mongodb mongodb-server
```
如有安装问题，查询官网步骤即可.

2. 运行MongoDB

```
service mongod start
```

### NodeJS ###
1. 按照nodejs10

```
curl -sL https://rpm.nodesource.com/setup_10.x | bash -
yum install nodejs
```

2. 安装pm2

```
npm install -g pm2
```

3. 运行环境

```
cd spider && npm install && cd ..
cd web && npm install && npm run build && cd ..
```

如果出现npm iconv安装失败，可能需要安装gcc/g++ 编译工具.

### 爬虫网站 ###
1. 启动爬虫

```
cd spider/ && pm2 start ecosystem.config.js && cd ..
```

2. 启动web

```
cd web && pm2 start ecosystem.config.js && cd ..
```

3. 配置web
配置nginx访问web页面。

```
yum install nginx
vim /etc/nginx/nginx.conf
```

加入proxy_pass的配置。

```
server {
...
    location / {
        proxy_pass http://localhost:3001;
    }
...
}
```

启动nginx.
```
service nginx start
```

### 搜索引擎 ###
1. 安装SphinxSearch

```
yum install http://sphinxsearch.com/files/sphinx-2.3.2-1.rhel7.x86_64.rpm
```

2. 创建目录

```
mkdir -p /data/bt/index/db /data/bt/index/binlog
```

3. 初始化索引

```
cd spider
indexer -c sphinx.conf hash
indexer -c sphinx.conf hash_delta
searchd -c sphinx.conf
```


## 网站说明
这是 www.shousibaocai.org 的网站源代码。
开源的目的是为了促进技术交流和相互学习，把DHT与搜索引擎技术应用到更广泛的领域去。

本站于2015年5月使用django改写。
本站于2019年使用nodejs改写。
与爬虫相关的代码都在目录spider目录下。

相关文章请查看作者博客：
http://xiaoxia.org/2015/05/15/shousibaocai-opensource/
