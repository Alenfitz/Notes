Mac use Homebrew install mysql

MySQL重装过程记录

**1:使用brew  install mysql 安装**

```
  另一种安装方式是在官网下载dmg镜像安装,但是在通过系统面板启动MySQL时一直出现问题,无法启动,也尝试过寻找办法解决,无果。遂使用brew 在线安装的方式
```

Last login: Mon Nov 20 19:00:48 on console

MacBookPro:~ alenfitz$ brew install mysql

Updating Homebrew...

==&gt;**Auto-updated Homebrew!**

Updated 1 tap \(homebrew/core\).

==&gt;**Updated Formulae**

erlang heroku mame wpcli-completion

flatbuffers  hugo orc  wtf

fn jabba  protobuf-swift yle-dl

folly  jenkins  rom-tools

gnupg  lgogdownloader simple-obfs

==&gt;**Downloading **[https://homebrew.bintray.com/bottles/mysql-5.7.20.sierra.bottle](https://homebrew.bintray.com/bottles/mysql-5.7.20.sierra.bottle)**.**

\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\# 100.0%

==&gt;**Pouring mysql-5.7.20.sierra.bottle.tar.gz**

==&gt;**/usr/local/Cellar/mysql/5.7.20/bin/mysqld --initialize-insecure --user=alenf**

==&gt;**Caveats**

We've installed your MySQL database without a root password. To secure it run:

mysql\_secure\_installation

MySQL is configured to only allow connections from localhost by default

To connect run:

mysql -uroot

To have launchd start mysql now and restart at login:

brew services start mysql

Or, if you don't want/need a background service you can just run:

mysql.server start

==&gt;**Summary**

🍺 /usr/local/Cellar/mysql/5.7.20: 324 files, 233.7MB

**2:启动MySQL服务**

```
   这里要特别感谢 http://blog.csdn.net/lkxlaz/article/details/54580735原贴主人
```

以前在重新安装完MySQL后在启动服务前总会要求配置/设置一些参数来满足启动的条件。但是

好多教程其实已经过期了，不再具有参考意义，即使教程日期是15年，16年的，其实从13年的5.76版本后

MySQL就改变了很多，这个版本也被称为milestone version

------------以下内容为copy--------------------

其中很重要的一点就是 网上不少教程说安装完MySQL后要运行

`mysql_install_db --verbose --user=`whoami`--basedir="$(brew --prefix mysql)" --datadir=/usr/local/var/mysql --tmpdir=/tmp`，

我试了一下行不通，原因mysql官方已经说了，看这段：

> mysql\_install\_db is deprecated as of MySQL 5.7.6 because its functionality has been integrated into mysqld, the MySQL server. To initialize a MySQL installation, invoke mysqld with the –initialize or –initialize-insecure option. For more information, see Section 2.10.1.1, “Initializing the Data Directory Manually Using mysqld”. mysql\_install\_db will be removed in a future MySQL release.

--------------无耻的分割线---------------------------------

我在这里想吐槽的是:何止行不通，简直就是坑爹！！！我一个下午重装不少次MySQL，每次都会在安装后启动失败，MySQL的完全卸载又很头疼，所以折腾的真实 欲仙欲死。

其实官方的意思很清楚了，mysql\_install\_db 这个功能已经取消了，功能集成在mysqld里面。

其实在brew install mysql 的过程中

> ==&gt;**/usr/local/Cellar/mysql/5.7.20/bin/mysqld --initialize-insecure --user=alenf**
>
> ==&gt;**Caveats**

已经设置好了 不需要再去设置了

> We've installed your MySQL database without a root password. To secure it run:
>
> mysql\_secure\_installation

运行  mysql\_secure\_installation 运行后提示报错

```
MacBookPro:~ alenfitz$ mysql_secure_installation 

Securing the MySQL server deployment.

Enter password for user root: 
Error: Can't connect to local MySQL server through socket '/tmp/mysql.sock' (2)
```

take it easy ~ 提示说找不到mysql.sock   the reason  is havn't  start the process for mysql

启动mysql 服务

```
MacBookPro:~ alenfitz$ mysql.server start
Starting MySQL
. SUCCESS!
```

再次运行 mysql\_secure\_installation

```
MacBookPro:~ alenfitz$ mysql_secure_installation
```

成功执行命令

```
Securing the MySQL server deployment.

Connecting to MySQL using a blank password.

VALIDATE PASSWORD PLUGIN can be used to test passwords
and improve security. It checks the strength of password
and allows the users to set only those passwords which are
secure enough. Would you like to setup VALIDATE PASSWORD plugin?

Press y|Y for Yes, any other key for No: y

There are three levels of password validation policy:

LOW    Length >= 8
MEDIUM Length >= 8, numeric, mixed case, and special characters
STRONG Length >= 8, numeric, mixed case, special characters and dictionary                  file
//这里选择一个密码强度等级,友情提示：一般没太高要求的话就选0，选了1的我觉得很坑爹 要求大小写以及数字和特殊字符，并且长度在8位以上。
Please enter 0 = LOW, 1 = MEDIUM and 2 = STRONG: 1
Please set the password for root here.

New password: 

Re-enter new password: 

Estimated strength of the password: 50 
Do you wish to continue with the password provided?(Press y|Y for Yes, any other key for No) : y
 ... Failed! Error: Your password does not satisfy the current policy requirements
//这里就是因为密码规格不满足所选的 密码强度=1 的条件  重新设定密码
New password: 

Re-enter new password: 

Estimated strength of the password: 100 
Do you wish to continue with the password provided?(Press y|Y for Yes, any other key for No) : y
By default, a MySQL installation has an anonymous user,
allowing anyone to log into MySQL without having to have
a user account created for them. This is intended only for
testing, and to make the installation go a bit smoother.
You should remove them before moving into a production
environment.
//选择是否删除默认的无密码用户
Remove anonymous users? (Press y|Y for Yes, any other key for No) : y
Success.


Normally, root should only be allowed to connect from
'localhost'. This ensures that someone cannot guess at
the root password from the network.
//是否禁止远程登录该数据库
Disallow root login remotely? (Press y|Y for Yes, any other key for No) : 

 ... skipping.
By default, MySQL comes with a database named 'test' that
anyone can access. This is also intended only for testing,
and should be removed before moving into a production
environment.

//是否删除 默认的test数据库
Remove test database and access to it? (Press y|Y for Yes, any other key for No) : y
 - Dropping test database...
Success.

 - Removing privileges on test database...
Success.

Reloading the privilege tables will ensure that all changes
made so far will take effect immediately.
//是否立即重载权限表
Reload privilege tables now? (Press y|Y for Yes, any other key for No) : y
Success.

All done!
```

测试登录mysql

> MacBookPro:~ alenfitz$ mysql -u root -p
>
> Enter password:
>
> Welcome to the MySQL monitor. Commands end with ; or \g.
>
> Your MySQL connection id is 5
>
> Server version: 5.7.20 Homebrew
>
> Copyright \(c\) 2000, 2017, Oracle and/or its affiliates. All rights reserved.
>
> Oracle is a registered trademark of Oracle Corporation and/or its
>
> affiliates. Other names may be trademarks of their respective
>
> owners.
>
> Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

登录成功！

# **总结：**

网上教程五花八门，自己还是要对要安装的东西有一定的了解，要能够仔细筛选出自己所需要的教程，不要一味的按照其中某篇教程

闷头做。

ps:完全卸载mysql的一些命令

```
sudo rm /usr/local/mysql
sudo rm -rf /usr/local/mysql*
sudo rm -rf /Library/StartupItems/MySQLCOM
sudo rm -rf /Library/PreferencePanes/My*
vim /etc/hostconfig  (and removed the line MYSQLCOM=-YES-)
rm -rf ~/Library/PreferencePanes/My*
sudo rm -rf /Library/Receipts/mysql*
sudo rm -rf /Library/Receipts/MySQL*
sudo rm -rf /var/db/receipts/com.mysql.*
```

参考:[https://stackoverflow.com/questions/1436425/how-do-you-uninstall-mysql-from-mac-os-x](https://stackoverflow.com/questions/1436425/how-do-you-uninstall-mysql-from-mac-os-x "How do you uninstall MySQL from Mac OS X?")

or :[https://stackoverflow.com/questions/1436425/how-do-you-uninstall-mysql-from-mac-os-x](https://stackoverflow.com/questions/1436425/how-do-you-uninstall-mysql-from-mac-os-x)

