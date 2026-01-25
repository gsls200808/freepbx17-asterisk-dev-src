# freepbx17 asterisk-dev源码构建和使用

在编译EC20时需要使用到asterisk-dev包，但是各发行版提供的有限

而且freepbx17 对于的LTS版本与debian ubuntu发行版的asterisk-dev对应的版本并不一致。所以需要自主构建

官网的LTS
18.26.4
20.18.0
22.8.0

freepbx17的lts
18.26.4
20.17.0
22.7.0

debian的asterisk-dev

asterisk-dev_22.7.0

ubuntu的asterisk-dev

asterisk-dev_20.6.0



如果你用sudo asterisk-version-switch切换到了20LTS版本，可以直接使用

```
git clone https://github.com/gsls200808/freepbx17-asterisk-dev-src.git
cd freepbx17-asterisk-dev-src/src
sudo tar -zxvf include20.17.0.tar.gz -C /usr/include/ --strip-components=1
```



如果你要自主编译include目录，可以按如下方式操作

找到asterisk版本对应的源码，

如果你的版本是20.17.0

可访问

https://github.com/asterisk/asterisk/releases/tag/20.17.0

其他版本替换urltag后面的版本号，找到tar.gz下载，我这里是

https://github.com/asterisk/asterisk/releases/download/20.17.0/asterisk-20.17.0.tar.gz



使用下面的脚本生成include目录需要的文件

```
cd /usr/src
wget https://github.com/asterisk/asterisk/releases/download/20.17.0/asterisk-20.17.0.tar.gz
tar -zxvf asterisk-20.17.0.tar.gz
cd asterisk-20.17.0

#选择配置模块 弹窗后 按ESC退出
make menuselect

#编译 但不安装，避免覆盖freepbx17自身的文件
contrib/scripts/get_mp3_source.sh
contrib/scripts/install_prereq install
./configure  --libdir=/usr/lib64 --with-pjproject-bundled --with-jansson-bundled
make

#拷贝到 /usr/include/目录
sudo cp -rpv /usr/src/asterisk-20.17.0/include/* /usr/include/
```



在EC20编译项目里注意指定DESTDIR目录,必须用root权限

```
su root
git clone https://github.com/IchthysMaranatha/asterisk-chan-quectel
 cd asterisk-chan-quectel
 ./bootstrap
./configure --with-astversion=20.17.0 DESTDIR=/usr/lib/x86_64-linux-gnu/asterisk/modules
 make
 make install
cp uac/quectel.conf /etc/asterisk
```



