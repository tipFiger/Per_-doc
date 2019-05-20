## scp是什么?
scp是 secure copy的缩写, scp是linux系统下基于ssh登陆进行安全的远程文件拷贝命令。
## 格式与参数
格式： scp [可选参数] file_source(源) file_target(目标) 
常用参数： 
-p：保留原文件的修改时间，访问时间和访问权限。
-r： 递归复制整个目录。
-P port：注意是大写的P, port是指定数据传输用到的端口号

## 从本地复制到远程
scp local_file remote_username@remote_ip:remote_folder 
或者 
scp local_file remote_username@remote_ip:remote_file 
或者 
scp local_file remote_ip:remote_folder 
或者 
scp local_file remote_ip:remote_file 
> 第1,2个指定了用户名，命令执行后需要再输入密码，第1个仅指定了远程的目录，文件名字不变，第2个指定了文件名；
> 第3,4个没有指定用户名，命令执行后需要输入用户名和密码，第3个仅指定了远程的目录，文件名字不变，第4个指定了文件名；
> 复制目录需要参数 -r  递归拷贝
## 从远程复制到本地
scp root@www.runoob.com:/home/root/others/music /home/space/music/1.mp3 
scp -r www.runoob.com:/home/root/others/ /home/space/music/
```
1.如果远程服务器防火墙有为scp命令设置了指定的端口，我们需要使用 -P 参数来设置命令的端口号，命令格式如下：
*scp 命令使用端口号 4588*
scp -P 4588 remote@www.runoob.com:/usr/local/sin.sh /home/administrator
2.使用scp命令要确保使用的用户具有可读取远程服务器相应文件的权限，否则scp命令是无法起作用的。
```

