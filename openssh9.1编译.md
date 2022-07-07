#  1、安装要求
可以在AIX 6.1 7.1 7.2下编译安装，要求安装IBM最新的openssl与openssh。
``` 
  openssh.base.server   8.1.102.2105  COMMITTED  Open Secure Shell Server
  openssl.base            1.1.1.1200  COMMITTED  Open Secure Socket Layer
``` 
其它要求
需要安装zlib和zlib开发包、gcc、perl
```
#rpm -qa |grep zlib
zlib-1.2.8-1
zlib-devel-1.2.8-1
#rpm -qa |grep gcc
libgcc-4.8.3-1
gcc-4.8.3-1
gcc-cpp-4.8.3-1
gcc-gfortran-4.8.3-1
gcc-c++-4.8.3-1
```




# 2、 编译准备
编译环境
``` 
OPENSSH_PATH="/opt/freeware/openssh90p1"
export OPENSSH_PATH
 Library path in linking time
export blibpath="$OPENSSH_PATH/lib64:/usr/lib64:/usr/lib:/lib:/opt/freeware/lib64:/opt/freeware/lib:/usr/ccs/lib"
 Library path in run time
export LIBPATH="$blibpath"
export LDFLAGS="-Wl,-blibpath:$blibpath,-brtl"
export CFLAGS="-g -O2"
export CPPFLAGS="-I$OPENSSH_PATH/include"
export CC="gcc -maix64"
export OBJECT_MODE=64
export NM="nm -X64"
export AR="ar -X64"

```
下载源代码包[openssh-9.0p1.tar.gz](https://mirrors.aliyun.com/pub/OpenBSD/OpenSSH/portable/openssh-9.0p1.tar.gz) 
解压缩，进入解压目录
### 可以解压到GPFS文件系统或者NAS上，操作系统环境相同的情况下只需编译一次，可以在集群其它非编译节点执行make install安装

``` 
 ./configure --prefix=$OPENSSH_PATH           \
          --without-stackprotect               \
               --with-pam                           \
        --with-md5-passwords                 \
		      --sysconfdir=$OPENSSH_PATH/etc/ssh
        --with-privsep-path=/var/empty/sshd  

make

make install


```

# 3、 检查编译后运行情况
stopsrc -s sshd
执行 /opt/freeware/openssh9.1/sshd -Dd 测试是否能够远程ssh 
测试正常后，修改系统链接
``` 
stopsrc -s sshd
#/opt/freeware/openssh9.1/sbin/sshd -Dd
debug1: sshd version OpenSSH_9.0, OpenSSL 1.1.1l  24 Aug 2021
debug1: private host key #0: ssh-rsa SHA256:
debug1: private host key #1: ecdsa-sha2-nistp256 SHA256:
debug1: private host key #2: ssh-ed25519 SHA256:
debug1: rexec_argv[0]='/opt/freeware/openssh9.1/sbin/sshd'
debug1: rexec_argv[1]='-Dd'
debug1: Bind to port 22 on 0.0.0.0.
Server listening on 0.0.0.0 port 22.
debug1: Bind to port 22 on ::.
Server listening on :: port 22.

# telnet 127.0.0.1 22
Trying 127.0.0.1...
Connected to 127.0.0.1.
Escape character is '^]'.
SSH-2.0-OpenSSH_9.0
```

# 4、备份与安装ssh程序
```
 tar cvf /home/ssh.orig.tar /usr/bin/ssh-keyscan                    \
 /usr/bin/scp                            \
 /usr/bin/sftp                           \
 /usr/bin/ssh                            \
 /usr/sbin/ssh-keysign                   \
 /usr/bin/ssh-add                        \
 /usr/bin/ssh-keygen                     \
 /usr/bin/ssh-agent                      \
 /usr/sbin/ssh-pkcs11-helper             \
 /usr/sbin/sshd                          \
 /usr/sbin/sftp-server     



cp /etc/ssh/ssh_host* /opt/freeware/openssh9.1/etc/ssh
mv  /etc/ssh /etc/ssh.bak
ln -s /opt/freeware/openssh9.1/etc /etc/ssh


openssh=/opt/freeware/openssh90p1
 
ln -sf $openssh/bin/scp                 /usr/bin/scp         
ln -sf $openssh/bin/sftp                /usr/bin/sftp
ln -sf $openssh/bin/ssh                 /usr/bin/ssh
ln -sf $openssh/bin/ssh-add             /usr/bin/ssh-add
ln -sf $openssh/bin/ssh-agent           /usr/bin/ssh-agent
ln -sf $openssh/bin/ssh-keygen          /usr/bin/ssh-keygen
ln -sf $openssh/bin/ssh-keyscan         /usr/bin/ssh-keyscan
ln -sf $openssh/sbin/sshd                   /usr/sbin/sshd
ln -sf $openssh/libexec/sftp-server         /usr/sbin/sftp-server
ln -sf $openssh/libexec/ssh-keysign         /usr/sbin/ssh-keysign
ln -sf $openssh/libexec/ssh-pkcs11-helper   /usr/sbin/ssh-pkcs11-helper
ln -sf $openssh/libexec/ssh-sk-helper       /usr/sbin/ssh-sk-helper

startsrc -s sshd

```
