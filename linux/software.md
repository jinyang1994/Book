# 软件安装

## redis安装

```
$ wget http://download.redis.io/releases/redis-4.0.1.tar.gz

$ tar xzf redis-4.0.1.tar.gz

$ cd redis-4.0.1

$ make MALLOC=libc
```

设置环境变量

``` 
$ which redis

$ pwd

$ ln -s /root/redis-4.0.1/src/redis-cli /usr/bin/redis-cli
```

## Node 安装

```
$ curl --silent --location https://rpm.nodesource.com/setup_6.x | sudo bash -

$ sudo yum -y install nodejs

$ node -v
```

## Git 安装

```
$ yum install http://opensource.wandisco.com/centos/6/git/x86_64/wandisco-git-release-6-1.noarch.rpm

$ yum install git 

$ git --version
```

## Nginx 安装

```
vim /etc/yum.repos.d/nginx.repo

[nginx]
name=nginx repo
baseurl=http://nginx.org/packages/centos/$releasever/$basearch/
gpgcheck=0
enabled=1

yum install nginx -y
```

