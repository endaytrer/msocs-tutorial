## MSoCS Linux基础操作（附小测）

>  顾真榕 GitHub: [@endaytrer](https://github.com/endaytrer)

[TOC]

### 文件系统

Linux不同于Windows以分区为基础的文件系统. 所有的文件都是**根目录`/`**的子孙.

比方说, 你的Windows用户目录是这样:

```
C:\Users\Me\
```

在其他分卷中的某个文件夹是这样:

```
D:\path\to\the\folder
```

而在Linux中, 用户目录是这样:

```bash
/home/me
~
/Users/me # macOS
```

其他分卷的文件(不推荐在其他分卷进行操作)

```bash
/mnt/d/path/to/the/folder # windows wsl
/Volumes/Data/path/to/the/folder # macOS
```

显著的特点:

1. Windows是反斜杠, 而Linux是正斜杠. 这点避免了'\\'在转义的时候被吞掉的问题
2. 通过`~`指代用户目录
3. 通过`/`指代根目录

其他重要的点

1. 用`.`指代当前目录
2. 用`..`指代上级目录. 根目录的上级目录是根目录

例: 当前你在`/home/me/path/to/file/a/content/`, 若想要在这个语境下表示路径`/home/me/path/to/file/b/content/a.txt`, 你可以有以下几种表示方法:

1. 从根目录开始计算: `/home/me/path/to/file/b/content/a.txt`. 此路径称为**绝对路径**
2. 从用户目录开始计算: `~/path/to/file/b/content/a.txt`
3. 从当前目录开始计算: `../../b/content/a.txt`. 此路径称为**相对路径**. 表示同一个文件夹下的内容是, 可以省略`./`. 例: `path` = `./path`; 但若要运行目录下的二进制文件, 必须使用`./`.
4. 胡乱表示: `./.././../a/../b/./content/../content/a.txt` 只要推过去对就行

linux隐藏文件名以`.`开头, 这就是为什么Minecraft的内容文件夹叫`.minecraft`.

> **自测1.1**
>
> 这是一个linux系统的部分文件树.
>
> ```
> /(root)
> |-	home
> |	 |- user1
> |	 |   |-	a 
> |	 |	 |	 |-.abc # 选项A
> |	 |   |- .a.abc # 选项B
> |	 |- user2
> |	     |-	a.abc # 选项C
> |	     |- .a.abc # 选项D
> |-	dir
> |	 |- a # 选项E
> |	 |- a.abc # 选项F
> |	 |- .a # 选项G
> |	 	 |-.abc # 选项H
> |-	a.abc # 选项I
> |- .a.abc # 选项J
> ```
>
> 你的用户名是user1, 目前处于/dir/的位置上. 以下路径分别表示什么? 写出路径在上述文件树上的位置# 后面的选项名称
>
> 1. `.a`
> 2. `./a`
> 3. `.a/.abc`
> 4. `./a.abc`
> 5. `/.a.abc`
> 6. `../a.abc`
> 7. `~/a/.abc`
> 8. `~/.a.abc`
>
> 答案在文末.



#### `pwd` (Present Working Directory)

若你想获取当前路径是什么 (绝对路径表示), 使用`pwd`即可

> 注: 示例中`user@device ~$ `是bash默认输出的内容, 若想自己试一试, 仅需输入`pwd`即可; 不带该前缀的, 是输出的结果.

**例**

```bash
user@device ~$ pwd
/home/user

user@device /$ pwd
/
```

#### `cd` (Change Directory)

若你想前往一个路径, 使用`cd ${pathTo} `即可. pathTo可以是上述提到的任何一种形式. 直接使用`cd`会回到用户目录. 在输入一半后, 你可以按下tab来查看符合输入条件的目录; 若只有一个匹配项, 就会帮你补全. 如果你使用了zsh, 你会在匹配项中toggle.

**例**

```bash
user@device ~$ pwd
/home/user
user@device ~$ cd ..
user@device /home$ pwd
/home
user@device /home$ cd ..
user@device /$ pwd
/
user@device /$ cd home/user/folder
user@device /home/user/folder
```

**建议在自己的设备上多试试**, 这样你就可以在控制台中灵活地前往文件夹.

#### `ls` (LiSt)

若你想列出路径下的所有文件和文件夹(不包括隐藏文件), 使用`ls `即可; 若需要包括隐藏文件, 使用`ls -a`可列出包括隐藏文件的所有文件. `ls ${path}`可列出`path`下所有文件和文件夹; `ls *.txt`则可以列出本目录下的所有txt文件. 这里的*称为**通配符**, 代表任意字符. 具体有关通配符的事项可以查看[百度百科](https://baike.baidu.com/item/%E9%80%9A%E9%85%8D%E7%AC%A6).

`ls -a`里的`-a`我们称为option. option是程序定义的, 但一般服从以下规则:

带有一个减号的, 后面跟首字母; 带有两个减号的后面跟单词.

带有一个减号的options若想同时使用, 可以仅输入一个减号, 然后在后面连续跟options的字母.

一般的程序可能都带有一下的option:

`-v, --version`: 展示程序版本

`-h --help`: 获取帮助

**例**

```bash
user@device ~$ ls
dir1		dir2		dir3		a		a.txt
user@device ~$ ls -a
.		..		.ssh		.vscode		.oh-my-zsh		.zshrc		a			a.txt		dir1		dir2		dir3
user@device ~$ ls -a /
.     	cdrom  	initrd.img      		lost+found  	proc  		snap      	tmp      	vmlinuz.old
..    	dev    	initrd.img.old  		media       	root  		srv       	usr
bin   	etc    	lib             		mnt         	run   		swapfile  	var
boot  	home   	lib64           		opt         	sbin  		sys       	vmlinuz
```

**建议在自己的设备上多试试**, 这样你就可以在控制台中灵活地前往文件夹

#### `mkdir` (MaKe DIRectory)

若你想在工作路径下新建文件夹, 使用`mkdir ${folderName} `即可. 也可以在指定工作路径中新件文件夹, 用法: `mkdir ${directory}/${folderName}`

如果`${directory}`不存在, 使用`mkdir`命令会报错. 如果你想用一个命令在新建的文件夹内递归地新建文件夹怎么办? 在后面跟一个option`-p`即可. 这样就会在相应路径新建文件夹; 若路径不存在则创建.

**例**

```bash
user@device ~$ ls
dir1		dir2		dir3		a		a.txt
user@device ~$ mkdir -p b/e
user@device ~$ mkdir c
user@device ~$ ls
dir1		dir2		dir3		b		c		a		a.txt
user@device ~$ cd b
user@device ~/b$ ls
e
user@device ~/b$ pwd
/home/user/b
```

#### `touch`

新建文件, 和mkdir用法类似

```bash
user@device ~$ ls
dir1		dir2		dir3		a		a.txt
user@device ~$ touch b
user@device ~$ touch c
user@device ~$ touch a/c
user@device ~$ ls
dir1		dir2		dir3		a		a.txt		b		c

user@device ~$ cd a
user@device ~/a$ ls
c
```

#### `rm`

删除文件. 用法: `rm ${file}`.

也可加`-r`进行递归删除, 加`-f`强制删除. 组合`rm -rf`一般用于删除文件或文件夹.

```bash
user@device ~$ ls
dir1		dir2		dir3		a		a.txt
user@device ~$ rm a.txt
user@device ~$ rm -r dir3
user@device ~$ ls
dir1		dir2		a

user@device ~$ rm -f a
user@device ~/a$ ls
dir1		dir2
```

#### `cat ` (conCATenate)

cat命令用于输出文件内容. 用法: `cat ${file}`. 

**例**

```
# a.txt
hello world
```

```bash
user@device ~/dir$ cat a.txt
hello world

user@device ~/dir$ cat ~/.ssh/authorized_keys
ssh-rsa ********
ssh-rsa ********
```

#### `echo` 

输入一个内容, 并输出.

echo命令经常和`>`, `>>`连用. `echo something > file`意为将something覆盖到file文件上, 若不存在则会创建, `echo something >> file`意为将something追加到file中.

**例**

```bash
user@device ~$ echo hello world
hello world
user@device ~$ cat a
hello world
user@device ~$ echo hello again > a
user@device ~$ cat a
hello again
user@device ~$ echo hello again >> a
user@device ~$ cat a
hello again
hello again
```

#### `clear`

清除屏幕.

#### `sudo` (Super User DO)

用超级管理员的模式完成一件事情.

为什么要用sudo?

在`ls`中, 我们仅讲解了基本用法. `ls`也可以带有一定的options. 你若加了`-al`, 或者使用`ll`, 你会获得如下信息.

```bash
user@device:~$ ll
总用量 11876
drwxr-xr-x 21 user user     4096 Mar 10 22:55 ./
drwxr-xr-x  3 root root     4096 Mar  4 20:07 ../
-rw-------  1 user user     1204 Mar  4 21:49 .ICEauthority
-rw-------  1 user user      144 Mar  4 23:37 .Xauthority
-rw-------  1 user user     5311 Mar 21 17:30 .bash_history
-rw-r--r--  1 user user      220 Mar  4 20:07 .bash_logout
-rw-r--r--  1 user user     3771 Mar  4 20:07 .bashrc
drwx------ 15 user user     4096 Mar 10 22:55 .cache/
drwx------ 15 user user     4096 Mar  9 08:41 .config/
-rw-rw-r--  1 user user       54 Mar  9 18:34 .gitconfig
drwx------  3 user user     4096 Mar  4 20:28 .gnupg/
drwx------  3 user user     4096 Mar  4 20:28 .local/
-rw-------  1 user user      147 Mar  5 00:13 .mysql_history
drwxrwxr-x  5 user user     4096 Mar 10 23:01 .npm/
-rw-r--r--  1 user user      807 Mar  4 20:07 .profile
drwxr-xr-x  2 user user     4096 Mar 20 16:30 .ssh/
-rw-------  1 user user    13902 Mar 10 18:10 .viminfo
drwxrwxr-x  5 user user     4096 Mar 10 22:55 .vscode-server/
-rw-rw-r--  1 user user      183 Mar 10 22:55 .wget-hsts
-rw-------  1 user user 11948032 Mar  5 22:44 core
drwxr-xr-x  5 user user     4096 Mar  5 00:21 dist/
drwxrwxr-x  3 user user     4096 Mar  5 01:03 logs/
drwxr-xr-x 15 user user     4096 Mar 10 18:06 yi-dian-saving/
```

可以看见, 前面有这样的字符串:

```
d | r w x | r w x | r w x
0   1 2 3   4 5 6   7 8 9
```



第0位d表示它是一个文件夹; 若是`-`, 则是单文件, 若是l, 则是链接(感兴趣可以自己查找).

第1位至第3位是创建者权限. r (Readable)代表可读, w (Writable) 代表可写, x (eXecutable)代表可运行. 例如: `r-w`代表只读并可运行, `rw-`代表不可执行.

第4位至第6位是创建者所在用户组权限.

第7位至第9位是其他用户权限.

用户, 顾名思义, 是使用者; 用户组, 顾名思义, 是用户的集合.

如何查看文件是那个用户创建和他所在的组是什么呢?

例如:

```
drwx------ 15 user user     4096 Mar  9 08:41 .config/
              用户 用户组
```

第一个user就是创建者的用户, 第二个则是用户组. 可见这个文件夹对user这个用户是开放权限的, 而对其他用户则是不可读不可写不可运行.

对于某些文件或文件夹, 作为普通用户user, 我们是无法得到全部权限的, 例如:`/bin/`

```
drwxr-xr-x   2 root root    4096 Mar   4 23:19 bin/
```

什么是root?

root是linux上的超级管理员, 和Windows上的Administrator类似. 他的用户文件夹位置与其他用户都不同 -- 其他用户的用户文件夹是`/home/username/`, 而root用户则是`/root/`. 很多根目录下的文件和文件夹都是root创建的, 若需要修改他们 (包括安装程序, 因为需要修改在`/bin`, `/usr/bin`, `/usr/local/bin`, `/lib`, `/etc`等目录下的内容), 你必须获得root权限. 暂时地获得root权限的命令就是sudo. 用法: `sudo 任何命令`

若你使用的用户是在创建时设置的, 那么你就有使用sudo的权限.

比方说你想编辑nginx的配置, 位于`/etc/nginx/nginx.conf`:

```bash
aa@aa:~$ vim /etc/nginx/nginx.conf
```

若这样, 你的更改是无法写入的.

```bash
aa@aa:~$ sudo vim /etc/nginx/nginx.conf
```

这样的话你就可以更改nginx.conf的内容了.

现在, 你就可以了解到程序员界知名的梗:

```bash
sudo rm -rf /*
```

顾名思义, **使用超级管理员, 强制而且递归地删除根目录下的所有内容.**

**千万不要尝试!!! **

**千万不要尝试!!!**

**千万不要尝试!!!**

**若在自己的设备上使用, 你的资料会无法找回, 你的系统会无法运作; 若在公共的设备上使用, 你会承担相应法律责任.**

很多现在的系统对这条命令设置了保护, 你需要使用`--no-preserve-root`来覆盖. **但macOS不会有任何的提示 (笔者的血泪教训)**

<small><small>之后在工作上如果你想跳槽, 那你就可以在公司的服务器上使用这条命令, 然后在一个包吃包住, 专人服务的地方安享晚年.</small></small>



#### 包管理(Ubuntu `apt`为例)

以上的几个步骤在Powershell中都有替代. 而包管理则是linux中最为人性化、且Windows不内置的的方面之一. (Windows也可以选择安装Chocolaty作为替代)

包管理允许你在一个可靠的源上搜索软件并安装, 并自动安装其依赖项. 同时, 也可以为你已经安装的程序提供软件更新服务.

本文以Ubuntu 20.04上的apt为例, 其他的分发版请自行查阅文档.

**搜索软件:**

```bash
apt search 软件名
```

**安装软件:**

```bash
sudo apt install 软件名
brew install 软件名 # homebrew 无需 `sudo` 
```

**更新apt索引信息**

```bash
sudo apt update
```

**更新所有软件**

```bash
sudo apt upgrade
```

**重新安装软件**

```bash
sudo apt reinstall 软件名
```

**卸载软件**

```bash
sudo apt remove 软件名
brew uninstalll 软件名
```

**查看已安装软件:**

```bash
apt list
```



### 其他常用的功能

若缺失, 可以从包管理那里安装

#### `vim` 

这是一个非常好用的命令行文本编辑器, 在之后的学习中你会深入地了解到它. 这里首先教你们如何退出vim:

```
:q
```

如果还是没有退出, 说明你在慌乱之中不小心进入了其他模式:

先按`Esc`键, 然后输入:

```
:q!
```



#### `curl`

发送请求. 之后学到HTTP协议再具体展开. 可以用于查看网站内容, 下载文件等.

比如说之前的

```bash
curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh
```

#### `wget`

更加方便的下载工具. 上述的可以简写为:

```bash
wget https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh
```

#### `ssh` (Secure Shell)

ssh是常用的远程登录和shell使用工具. 这里仅讲解如何连接到某台服务器上. 一般来说:

```bash
ssh username@address
```

username: 服务器上的用户名;

address: 服务器地址. 可以是本机, 局域网或者广域网.

ssh是一个tcp的应用层协议. 默认开在tcp的22号端口. 若要指定端口, 则可以:

```bash
ssh username@address -p port
```

如果你还没有学过大学计算机基础, 或者计算机网络, 你可能会一头雾水; 不过没关系, 现在需要做的, 仅仅是学会如何连接到一台给定的服务器上面.

一般来说, 连接后需要登录. 你会被要求输入密码.

但是现在的服务器安全起见, 一般会关闭密码验证, 而采用密钥验证.

那什么是密钥验证呢?

你可以尝试一下命令(甚至在windows中的powershell都可以使用):

```bash
ssh-keygen
```

然后狂按回车, 或者根据自己的偏好选择, 这样你就会在`~/.ssh/`目录下看到`id_rsa`和`id_rsa.pub`两个文件.

**千万不要将`id_rsa`泄露给任何人或通过网络方式传输!!!**

**千万不要将`id_rsa`泄露给任何人或通过网络方式传输!!!**

**千万不要将`id_rsa`泄露给任何人或通过网络方式传输!!!**

这两个文件分别称作私钥和公钥. 可以用来加密和解密数据. 加密的方式一般是RSA非对称加密.

什么意思呢: Alice有一条信息, 想把它发给Bob. 可是, 邪恶的Eve想要偷听这条信息.

无奈之下, Bob想到一个办法. 我把一个**只能进行加密的钥匙(我们称作`id_rsa.pub`)**发送给Alice, 但是**解密的钥匙(我们称作`id_rsa`)**留在我们自己手中, 然后让她对消息进行加密.

Eve也可以获得这个加密钥匙. 但是他并没有解密钥匙.

Alice发送一套消息时, 通过加密钥匙加密; 但是Eve对加密的消息无计可施, 只有拥有了解密钥匙的Bob才知道Alice项传达给他什么消息.

具体来说, 通过私钥可以得到公钥, 而通过公钥则不能得到私钥. 公钥仅可以加密, 而私钥却可以加密和解密.

但是这也产生了一个问题: 这种方式仅可以单向传输信息; 而ssh的传输是双向的. 

这时候传统的对称加密有发挥了作用. 你的RSA公钥(称为密钥P)会发给服务器, 然后服务器随机产生一个AES对称加密密钥(称为密钥A), 通过密钥P加密传输给你的设备上. 通过你的私钥解密后, 你就获得了密钥A. 然后你们之间的数据就可以通过密钥A进行同步的加密和解密了.

于是, 对于仅开放密钥登录的服务器, 你需要这样:

1. 想办法把你的**公钥`id_rsa.pub`** 传入服务器的`~/.ssh/authorized_keys`文件内. 一般联系服务器的管理员, 云服务器的话一般有这样的配置可供设置.

2. 通过一下命令进行第一次登录:

   ```bash
   ssh -i 你的私钥位置(一般是~/.ssh/id_rsa) 用户@地址
   ```

3. 接下来的登录你就可以直接像平时那样使用ssh了, 而且不用输入密码了.

登录完成后, 像控制自己的机器一样控制服务器吧!



#### `sftp` Secure File Transport Protocol

sftp是基于ssh的安全文件传输工具. 用法和ssh类似

```bash
sftp [-P 端口][-i 私钥] 用户名@地址 
```

一般ssh能连上的服务器, sftp也可以连上. 端口和ssh的端口是相同的

连上后, 你会进入一个界面简单的命令行.

```bash
sftp > 
```

你可以使用`put`发送文件 (从本机到服务器).也可以输入一些简单的指令(cd, ls之类), 对服务器进行操作.

在指令前加上`!`就会对本机进行操作.

```bash
user@local:~$ sftp user@192.168.31.111
sftp> ls
core                  cose                  cosr                  dist
logs                  thinclient_drives     yi-dian-saving

sftp> !ls
Desktop				QuickAccess			logs
Documents			Repositories		node_modules
Downloads			Startify			nohup.out
Library				WeChatProjects		npc-client
MATLAB-Drive		WebstormProjects	package-lock.json
MEGA				__MACOSX			package.json
Math_Modeling		a					pip.conf
Math_Modeling.git	a.cpp				settings.json
Movies				a.txt				sources
Music				config				texput.log
Pictures			dist				vim
Postman				fonts				yarn.lock
Postman Agent		hello.ts

sftp> put a.cpp
Uploading a.cpp to /home/user/a.cpp
a.cpp                                                   100%  101     7.9KB/s   00:00

sftp> ls
a.cpp                 core                  cose                  cosr
dist                  logs                  thinclient_drives     yi-dian-saving
```

>  自测答案: GEHFJIAB

