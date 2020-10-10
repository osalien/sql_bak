### 简介

这是Coding实验室的数据库自动备份仓库，每隔3天自动将数据库脚本提交到该仓库。下面是教程：

### 教程

1. 在github上创建仓库 sql_bak

2. 配置ssh：在服务器执行ssh-keygen，将生成好的ssh密文配置到github

3. 编写数据库导出脚本mysqlBak.sh

```
#!bin/sh
###################数据库配置信息######################
createAt=`date +%Y-%m-%d-%H:%M:%S`
user=数据库用户名
passwd=数据库密码
dbname=数据库名
mysql_back_path=/var/www/sql-bak 数据库备份路径
###################执行命令#######################
/usr/bin/mysqldump -u $user -p$passwd $dbname > $mysql_back_path/$createAt.sql
cd /var/www/sql-bak
/usr/bin/git pull origin main
/usr/bin/git add .
/usr/bin/git commit -m $createAt
/usr/bin/git push origin main
```
> 脚本说明：备份数据库并push到远程仓库

4. 设置定时任务执行 输入命令：
```
crontab -e
```
再键入如下，此处设定的是每三天执行一次
```
0 0 */3 * * /bin/sh /var/www/sql-bak/mysqlBak.sh
```
最后退出保存即可
