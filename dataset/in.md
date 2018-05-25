# Linux发送邮件的命令行应用
> 先说明下：不管是什么邮件客户端，都是可以直接发邮件的。但是，因为默认的话，发件人是很随便地设置成你本机地名字。并且**100%**会被邮箱当成垃圾邮件处理。如果你去垃圾箱里找，还是可以看到的。这就是为什么，我们还是需要配置它，让它登录某个邮箱来使用它的身份发邮件了，比如gmail邮箱或阿里云邮箱。（国内的163和qq邮箱都已经屏蔽第三方客户端登录了）


## `Mutt`
> Mutt是Linux邮箱客户端榜上有名的利器了。

先不说什么界面操作之类的，因为我们用命令行的邮箱客户端都是用来自动化的，不想用什么界面。

[参考：Linux使用mutt发送邮件](http://blog.51cto.com/wzlinux/2043647)

### 安装
其中`mutt`是软件本身，`msmtp`是用来帮助发件的工具。
```shell
# Linux
$ sudo apt-get install mutt msmtp

# 或Mac
$ brew install mutt msmtp
```

### 配置
你需要配置两个文件，一个是`~/.muttrc`用来配置Mutt本身，一个是`~/.msmtprc`用来配置发件人的，需要写入密码一类的。

[参考：Linux下使用mutt,msmtp发信](http://coolnull.com/82.html)

配置`~/.msmtprc`:
```shell
account     Aliyun
host        smtp.aliyun.com
from        jason@aliyun.com
auth        login
user        jason@aliyun.com
password    abcde123123123
account default : Aliyun
logfile ~/.msmtp.log
```
然后必须修改`~/.msmtprc`文件的权限，否则程序无法读取，发邮件时会报错。修改如下：
```shell
chmod 600 ~/.msmtprc
```

配置`~/.muttrc`：
```shell
set sendmail="/usr/bin/msmtp"
set use_from=yes
set realname="Jason"
set from="Jason@aliyun.com"
set envelope_from=yes
set editor="vim -nw"
```
注意：第一条`set sendmail`中的位置不一定是这样的，在Mac和Linux上都会不同，所以需要用`which msmtp`来找到它的真实位置，再填进去。

关于配置的解释可以看这里：
![image](https://user-images.githubusercontent.com/14041622/40438772-8415e3da-5eeb-11e8-8733-83b6aadab2b4.png)


### 发送邮件命令格式
注意：收件人的地址前一定要明确指定参数名`--`，如下所示。否则无法正确发送附件。

```shell
# 常用格式如下 -s   “标题”  -c    抄送  -a  附件
$ echo “HELLO WORLD” | mutt -s “TITLE” -- RECIPIENT@gmail.com

# 发送给多人，抄送，添加附件
$ echo "hello" | mutt -s "TITLE" aaa@gmail.com, bbb@gmail.com -c ccc@gmail.com -a /home/pi/pic.jpg address="RECIPIENT@gmail.com"

# 发送邮件时设置邮件的文本类型为：html格式，邮件的等级为:重要
$ echo $content | mutt  -s "${subject}" -e 'set content_type="text/html"' -e 'send-hook . "my_hdr  X-Priority: 1"' $address
```

语法：
![image](https://user-images.githubusercontent.com/14041622/40440006-455fbfa4-5eef-11e8-93b2-3b405e0215fb.png)

参数：
![image](https://user-images.githubusercontent.com/14041622/40440013-4b1cc57c-5eef-11e8-943b-d2bb7e762fe5.png)


## 邮箱配置
- [阿里云邮箱](https://help.aliyun.com/knowledge_detail/36576.html)
  ![image](https://user-images.githubusercontent.com/14041622/40422684-b1df8542-5ec2-11e8-96e1-8e8ad4045a98.png)
- 163邮箱
  ![image](https://user-images.githubusercontent.com/14041622/40435788-12ad41ea-5ee4-11e8-838d-4969e6224c92.png)

## ~`Mail`和`Sendmail`~
> 注：Mail的配置相当麻烦，网上找文章也寥寥无几，有也都是十几年前的东西。所以建议放弃，使用更先进的客户端。