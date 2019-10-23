# 常用命令

## tgz 压缩和解压

```
$ tar -zcvf ../release.tgz .

$ tar zxvf filename -C path
```

## 查看系统版本

```
$ cat /etc/issue 
```

## 查看系统时间

```
$ date
```

## 查看系统状态

```
$ top
```

## 查看命令位置

```
$ which [命令名]
```

## 查看当前路径位置

```
$ pwd
```

## 进程管理

```
# 查看进程
$ ps aux | grep [进程名]

# 杀掉进程
$ kill -9 pid

# 杀掉所有进程
$ killall [进程名]
```

## 查看socket连接数

```
# IPV4
$ cat /proc/net/sockstat

# IPV6
$ cat /proc/net/sockstat6
```

## 清除登录记录

```
ssh-keygen -R "IP"
```

## git 切分支


git branch -v 查看分支

git fetch -a 同步

git checkout embed 检出分支

git status 查看状态

git log 查看log

## 开机自启动

```
cat /etc/rc.local
```


