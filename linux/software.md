# 软件安装

## redis安装

```
sudo yum install epel-release
sudo yum install redis -y
sudo systemctl start redis.service
sudo systemctl enable redis
sudo systemctl status redis.service
```

设置环境变量

``` 
$ which redis

$ pwd

$ ln -s /root/redis-4.0.1/src/redis-cli /usr/bin/redis-cli
```

## Node 安装

```
// centos 6 
curl -sL https://rpm.nodesource.com/setup_8.x | bash -
// centos 7
sudo yum install epel-release
sudo yum install nodejs
npm i n -g
n 10.15.1
```

## Git 安装

```
sudo yum install git
git --version
```

## Nginx 安装

```
sudo yum install epel-release
sudo yum install nginx
sudo systemctl start nginx
```


## MySQL 安装


1. 获取安装包

`wget https://dev.mysql.com/get/mysql57-community-release-el7-9.noarch.rpm`

2. 添加到`yum`存储库

`sudo rpm -ivh mysql57-community-release-el7-9.noarch.rpm`

3. 安装

`sudo yum install mysql-server`

4. 启动`mysql`

 `sudo systemctl start mysqld`
 
5. 查看状态

 `sudo systemctl status mysqld`
 
6. 查看密码

`sudo grep 'temporary password' /var/log/mysqld.log`

7. 密码复杂度

`set global validate_password_policy=0;`

7. 重启设置密码

`sudo mysql_secure_installation`

8. 登录mysql

`mysql -u root -p`

9. 添加远程权限

`GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY '密码' WITH GRANT OPTION;`



