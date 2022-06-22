# 编译准备
[openssh-9.0p1.tar.gz](https://mirrors.aliyun.com/pub/OpenBSD/OpenSSH/portable/openssh-9.0p1.tar.gz) 
需要安装zlib和zlib开发包，

openssl.base            1.1.1.1200  COMMITTED  Open Secure Socket Layer   IBM自带的

gcc

perl     IBM自带的


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




``` 
 ./configure --prefix=$OPENSSH_PATH           \
          --without-stackprotect               \
               --with-pam                           \
        --with-md5-passwords                 \
        --with-privsep-path=/var/empty/sshd  

make

make install



```

# 检查编译后运行情况
执行 /opt/freeware/openssh9.1/sshd  测试是否能够远程ssh 

测试正常后，修改系统链接
``` 
telnet  remotehost 22


 tar cvf /home/dhcc/ssh.orig.tar /usr/bin/ssh-keyscan                    \
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

stopsrc -s sshd

cp /etc/ssh/ssh_host* /opt/freeware/openssh9.1/etc


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
