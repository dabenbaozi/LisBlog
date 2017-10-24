# 安装mysql时忘了记下临时密码解决方案(Solution for Resetting the Root Password in mysql -Mac)

#### I was going to install mysql for one of my class assignment. But during the installation, my mind was distracted and forgot to take note on the temporary password that Mac generated. As a result, I locked myslef out from mysql and the following is the issues I encountered while solving this problem and the solution.

本来为了做个作业要装mysql但是期间走了个神，忘了记录下mac自动生成的密码，所以就废了点时间来重新设置root密码。以下是解决问题中遇到的各种情况和解决方案。

#### First of all, stop MySQL Server， you can do it in two ways:

1. System Preferences->MySQL->Stop MySQL Server

2. But sometimes it won't work (unforturnately in my case) and will give you an error in the following steps: "A mysql process already exists":

   ![Screen Shot 2017-10-23 at 12.54.38 PM](/Users/li/Desktop/mysql/Screen Shot 2017-10-23 at 12.54.38 PM.png)

首先，需要把当前的MySQL server给停掉，有两种方法，一种是直接点stop MySQL Server，但是不是每次都成功，有的时候没停的话在后面的步骤会报“A mysql process already exists”的错。

####Second，open a Terminal and run MySQL under the "safe mode", which won't ask you for the password:

```
sudo mysqld_safe --skip-grant-tables
```

But it could  give you an error that the "command can not be found"

![Screen Shot 2017-10-23 at 12.50.25 PM](/Users/li/Desktop/mysql/Screen Shot 2017-10-23 at 12.50.25 PM.png)

This is because the command is saving in another directory, so you will need to find the folder and put the path in front of your command, for example, in my case, it is under the folder /usr/local/mysql/bin

第二步是打开一个terminal然后运行👆上面的代码，这可以在安全模式下运行MySQL。不过有可能会报“command can not be found”的错，这是因为这个命令可能存在不同的路径，你需要找到它然后换到指令前面。

#### Third， you need to open a new Terminal, open mysql and then run the sql command to change the password for root:

```
mysql -u root
```

```
UPDATE mysql.user SET authentication_string=PASSWORD('your_new_password') WHERE User='root';
FLUSH PRIVILEGES;
```

![Screen Shot 2017-10-23 at 12.58.48 PM](/Users/li/Desktop/mysql/Screen Shot 2017-10-23 at 12.58.48 PM.png)

I set the new terminal green for distinguishment.

Be aware, that here for the mysql command, you need to find where it locate too. Even though I never encountered this issue, but I found online that another issue could be  "authentication_string". For MySQL 5.7 or latter versions, you can run this SQL command directly, but for any earlier versions, you will need to use "PASSWORD" instead.

第三步，需要打开一个新的terminal（我用绿背景区分）打开sql，然后输入上面👆的sql语句，就完成了！不过注意两个问题，1是和之前一样的问题，可能需要加路径在mysql命令前面，2是由于版本不同，5.7以后的可以直接用这段sql，5.7之前的需要把"authentication_string"换成"PASSWORD"。 这是因为MySQL5.7之后改了定义。

#### DONE!!!









 