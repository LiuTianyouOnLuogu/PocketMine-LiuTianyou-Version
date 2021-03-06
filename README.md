这个仓库是PocketMine Alpha_1.13.2 (Minecraft PE Alpha_0.8.1) 的改进版，增加了个人的优化以及汉化。

由于原仓库使用LGPL协议，所以修改后的代码必须公布，并使用LGPL许可证。

### Windows教程
提示：本教程应用的是电脑端。很多教程都使用手机端，但是我建议使用电脑端，因为电脑端可以不占手机内存，方便内网穿透。

提示：下文中（包括上文），内网穿透即为端口映射。

使用Sakura Frp进行内网穿透（不是广告），可以使用逍遥模拟器模拟MCPE在电脑上的运行（不是广告x2）

1. 建立PocketMine-MP文件夹，然后下载releases中的PHP单文件放进去。

（也可以直接下载仓库根目录的）

3. 注意了，下面这一步特别重要（最好别先使用现成的PHP）：

下载PHP5.6：https://windows.php.net/downloads/releases/archives/php-5.6.9-Win32-VC11-x64.zip

解压出来之后重命名为php，在PocketMine文件夹里新建bin文件夹，移动过去。

下载pthreads扩展（注意，一定要确认是否线程安全，否则会“找不到指定的模块”）：https://windows.php.net/downloads/pecl/releases/pthreads/1.0.0/php_pthreads-1.0.0-5.3-ts-vc9-x86.zip

下载yaml扩展：https://windows.php.net/downloads/pecl/releases/yaml/1.3.2/php_yaml-1.3.2-5.6-ts-vc11-x64.zip

在你的扩展文件夹内，会有四个DLL文件（每个文件夹有两个其余的是源码，没啥用）：

yaml文件夹有yaml.dll php_yaml.dll

threads文件夹有threads_VC2.dll php_threads.dll

把不带php_前缀复制到C:\Windows和php文件夹内，带php_前缀的复制到php\ext\文件夹内（那个文件夹都是带php_前缀的）

4. 运行

运行start.bat，按照安装向导指示。

### Linux & Mac

参见Android

### Android

1. 下载Termux

下载地址（官网）：https://f-droid.org/repo/com.termux_117.apk

注意：只有新版Android（Android 7.0）才能运行Termux，太老的版本不行。

此方式可能会占用大量空间（100MB以上）。

2. 安装PHP环境

**这是一个相当复杂的过程，我自己尝试了3-4个小时才搞定。然而，你可以只用半小时做这些，会很爽！**

安装前置软件包：
```bash
pkg install wget libxml2 clang vim make autoconf libtool automake pkg-config iconv libyaml zlib -y 
```

下载PHP源代码，编译：
```bash
cd ~
wget https://www.php.net/distributions/php-5.6.40.tar.gz
tar xzvf php-5.6.40.tar.gz
cd php-5.6.40
# 抱歉让大家受苦了
# 用https://www.luogu.com.cn/paste/o3vxbtgf 替换ext/standard/dns.c
# 为什么找不到库？因为你没有pkg安装。不需要编译安装，需要写上目录/data/data/com.termux/files/usr
./configure --prefix=/data/data/com.termux/files/home/php \
    --exec-prefix=/data/data/com.termux/files/home/php \
    --bindir=/data/data/com.termux/files/home/php/bin \
    --sbindir=/data/data/com.termux/files/home/php/sbin \
    --includedir=/data/data/com.termux/files/home/php/include \
    --libdir=/data/data/com.termux/files/home/php/lib/php \
    --mandir=/data/data/com.termux/files/home/php/man \
    --with-config-file-path=/data/data/com.termux/files/home/php/etc \
    --with-zlib  --enable-pcntl --enable-sockets --with-curl  --enable-opcache \
    --with-zlib-dir=/data/data/com.termux/files/usr \
    --with-libxml-dir=/data/data/com.termux/files/usr \
    --with-curl=/data/data/com.termux/files/usr \
    --with-iconv=/data/data/com.termux/files/usr --enable-maintainer-zts
make && make install -k
echo "export PATH=\"$PATH:/data/data/com.termux/files/home/php/bin\"" >> ~/.bashrc #非必须，但没有会找不到
source ~/.bashrc
```

下载pthread和yaml扩展，安装：
```bash
cd ~
wget https://pecl.php.net/get/pthreads-1.0.1.tgz
tar xzvf pthreads-1.0.1.tgz
cd pthreads-1.0.1
phpize
./configure --with-php-config=/data/data/com.termux/files/home/php/bin/php-config
make && make install

cd ~
wget https://pecl.php.net/get/yaml-1.2.0.tgz
tar xzvf yaml-1.2.0.tgz
cd yaml-1.2.0
phpize
./configure --with-php-config=/data/data/com.termux/files/home/php/bin/php-config --with-yaml=/data/data/com.termux/files/usr
make && make install

echo "[PHP]" > ~/php/etc/php.ini #php.ini许多值是默认的，所以只需要三行
echo "extension = pthreads.so" >> ~/php/etc/php.ini #线程库
echo "extension = yaml.so" >> ~/php/etc/php.ini #yaml库
```

3. 运行

进入你的文件夹，运行```php -d enable_dl=On PocketMine-MP.php```即可。

注意：你可以通过写一个bash脚本实现：
```bash
#!/bin/bash
DIR="$(cd -P "$( dirname "${BASH_SOURCE[0]}" )" && pwd)"
cd "$DIR"
php -d enable_dl=On PocketMine-MP.php
```

4. 内网穿透

参见下文Android节

### 内网穿透

1. 下面说我们的重点：内网穿透。

大部分教程都是使用路由器穿透的，然而大部分人（中国移动出来挨打）没有固定的公网IP，甚至连公网IP都没有。我向大家推荐一个网站：https://www.natfrp.com/，这个网站完全免费（穿透Web需要实名认证收费）。

注册一个账号，按照图片中的配置穿透隧道（选UDP，可以改备注）

![](https://cdn.luogu.com.cn/upload/image_hosting/1av0nep2.png)

记住最开头的ID，还有你的访问密钥：
![](https://cdn.luogu.com.cn/upload/image_hosting/rru1dvsc.png)

下载frpc软件：https://getfrp.sh/d/frpc_windows_amd64.exe

放在PocketMine文件夹内，在start.cmd（或是start_without_mintty.cmd）内添加：

```frpc.exe -f 访问密钥:ID1```

示例：```frpc.exe -f 114514abcd:114514```

3.如图，如果没有域名，就可以输入节点的域名（IP也行）和外网端口。
![](https://cdn.luogu.com.cn/upload/image_hosting/87qn8293.png)

如果你有域名，就请CNAME解析，使用你的域名和外部端口。
![](https://cdn.luogu.com.cn/upload/image_hosting/0wt54jgq.png)

域名解析大概是这样：
![](https://cdn.luogu.com.cn/upload/image_hosting/43z2ut50.png)

这里有一个深坑啊，解析之后别立马尝试，要等待1-5分钟，否则域名解析不生效也不行

3. Android

首先，确认你的Android架构（使用`uname -a`）。然后，选择在Sakura Frp下载相应的版本。

```
架构	输出结果
i386	i386, i686
amd64	x86_64
armv6	arm
armv7	armv7l
arm64	aarch64, armv8l
mips*	mips
mips64*	mips64
不支持	alpha, arc, blackfin, c6x, cris, frv, h8300, hexagon, ia64, m32r, m68k, metag, microblaze, mn10300, nios2, openrisc, parisc, parisc64, ppc, ppcle, ppc64, ppc64le, s390, s390x, score, sh, sh64, sparc, sparc64, tile, unicore32, xtensa
```

```bash
cp (你下载的frpc文件) ~/php/bin/ # 这是为了加入PATH
```

接着，如上文所述使用即可。你可能需要编写一个脚本来实现自动运行：
```bash
DIR="$(cd -P "$( dirname "${BASH_SOURCE[0]}" )" && pwd)"
cd "$DIR"
frpc xxxx:xxx,xxx & # &符号指的是异步运行
php -d enable_dl=On PocketMine-MP.php
```

### 本次提交的更新日志

1. 大改变！废除players文件夹，用password.db存储玩家信息，避免同步错误

（知道为啥叫这名吗？我要为验证更新做准备）

如果需要将旧版信息同步至数据库，请手动修改。

2. 增加AuthAPI.php文件

### 有关版本号的约定

版本号固定为Alpha_1.3.12(Hack_x.x)，适用于MCPE Alpha_0.8.1（也许0.9.x也能用吧）

目前的版本代号以半条命的人物命名，例：戈登·弗里曼（Gordon Freeman）

### 招募

欢迎fork本仓库然后提交合并请求，为这个开源仓库做贡献。

本人实在没时间+看不懂代码，所以会很少更新。