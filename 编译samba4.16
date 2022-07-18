#  编译Samba 4.16.2 On CentOS 6.7
## 1、准备工作

编译要求：https://wiki.samba.org/index.php/Package_Dependencies_Required_to_Build_Samba

| |
|:--|
|python3 |Several utilities, such as  `samba-tool`  and the build system ([Waf](https://wiki.samba.org/index.php/Waf)), are written in Python 3.x. |
|perl | |
|Parse::Yapp |Used in PIDL, our IDL compiler. |
|acl |Required only on [Samba Active Directory domain controllers](https://wiki.samba.org/index.php/Active_Directory_Domain_Controller) and member servers using [Windows ACLs](https://wiki.samba.org/index.php/Setting_up_a_Share_Using_Windows_ACLs). |
|xattr |Required only on [Samba Active Directory domain controllers](https://wiki.samba.org/index.php/Active_Directory_Domain_Controller) and member servers using [Windows ACLs](https://wiki.samba.org/index.php/Setting_up_a_Share_Using_Windows_ACLs). |
|gnutls >= 3.4.7 |Required for cryptography |
|zlib |Provides the crc32 checksum across Samba and compression for DRSUAPI (AD replication) |

编译安装 gcc 6.5 使用（可选gcc6.5编译安装gcc8.5）
编译安装 Python3
编译安装 bintuil gmp mpfr mpc isl


### 编译gcc6.5
需先编译安装gmp mpfr mpc isl
```
GMP
gmp-6.2.1
./configure 
  Version:           GNU MP 6.2.1
  Host type:         ivybridge-pc-linux-gnu
  ABI:               64
  Install prefix:    /usr/local
  Compiler:          gcc -std=gnu99
  Static libraries:  yes
  Shared libraries:  yes
make
make install
ldconfig -v |grep gmp
MPFR
 mpfr-4.1.0  #使用新编译的GMP --with-gmp=/usr/local
./configure --with-gmp=/usr/local
make
make install 
[root@micapssvr01 mpfr-4.1.0]# ldconfig -v |grep mpfr
        libmpfr.so.1 -> libmpfr.so.1.2.0
MPC
 mpc-1.2.1]# ./configure --with-gmp=/usr/loca --with-mpfr=/usr/local
make
make install
mpc-1.2.1]# ldconfig -v |grep mpc
        libmpcdec.so.5 -> libmpcdec.so.5.0.2
##export PKG_CONFIG_PATH=/usr/local/lib64/pkgconfig:/usr/lib64/pkgconfig:/usr/local/lib/pkgconfig:/usr/lib/pkgconfig
export PKG_CONFIG_PATH=/usr/local/lib64/pkgconfig:/usr/local/lib/pkgconfig:/usr/lib64/pkgconfig:/usr/lib/pkgconfig
isl-0.24
./configure --with-gmp-prefix=/usr/local
make
make install
----------------------------------------------------------------------
Libraries have been installed in:
   /usr/local/lib

If you ever happen to want to link against installed libraries
in a given directory, LIBDIR, you must either use libtool, and
specify the full pathname of the library, or use the '-LLIBDIR'
flag during linking and do at least one of the following:
   - add LIBDIR to the 'LD_LIBRARY_PATH' environment variable
     during execution
   - add LIBDIR to the 'LD_RUN_PATH' environment variable
     during linking
   - use the '-Wl,-rpath -Wl,LIBDIR' linker flag
   - have your system administrator add LIBDIR to '/etc/ld.so.conf'

See any operating system documentation about shared libraries for
more information, such as the ld(1) and ld.so(8) manual pages.
----------------------------------------------------------------------
```
编译安装gcc
```
export LD_LIBRARY_PATH=/usr/local/lib:/usr/local/lib64:/usr/lib:/usr/lib64:$LD_LIBRARY_PATH

##export PKG_CONFIG_PATH=/usr/local/lib64/pkgconfig:/usr/lib64/pkgconfig:/usr/local/lib/pkgconfig:/usr/lib/pkgconfig
export PKG_CONFIG_PATH=/usr/local/lib64/pkgconfig:/usr/local/lib/pkgconfig:/usr/lib64/pkgconfig:/usr/lib/pkgconfig
./configure    --disable-multilib  --enable-languages=c,c++
make -j32
make install
----------------------------------------------------------------------
Libraries have been installed in:
   /usr/local/lib/../lib64

If you ever happen to want to link against installed libraries
in a given directory, LIBDIR, you must either use libtool, and
specify the full pathname of the library, or use the `-LLIBDIR'
flag during linking and do at least one of the following:
   - add LIBDIR to the `LD_LIBRARY_PATH' environment variable
     during execution
   - add LIBDIR to the `LD_RUN_PATH' environment variable
     during linking
   - use the `-Wl,-rpath -Wl,LIBDIR' linker flag
   - have your system administrator add LIBDIR to `/etc/ld.so.conf'

See any operating system documentation about shared libraries for
more information, such as the ld(1) and ld.so(8) manual pages.

# /usr/local/bin/gcc -v
Using built-in specs.
COLLECT_GCC=/usr/local/bin/gcc
COLLECT_LTO_WRAPPER=/usr/local/libexec/gcc/x86_64-pc-linux-gnu/6.5.0/lto-wrapper
Target: x86_64-pc-linux-gnu
Configured with: ./configure --disable-multilib --enable-languages=c,c++
Thread model: posix
gcc version 6.5.0 (GCC)


```
## 2、编译安装 binutil Python3  nettle-3.8 gnutls   texinfo make zlib-1.2.12.tar.gz


安装系统自带perl包
```
rpm -ivh /redhat6/Packages/perl-Parse-Yapp-1.05-41.el6.noarch.rpm
 rpm -ivh /redhat6/Packages/perl-JSON-2.15-5.el6.noarch.rpm
 rpm -ivh /redhat6/Packages/perl-parent-0.221-141.el6.x86_64.rpm
 rpm -qa |grep perl
 ```
 先编译安装  zlib texinfo make，再编译安装binutils-2.38
 ```
 texinfo-6.8    ./configure ;  make; make install
 [root@micapssvr01 texinfo-6.8]# ldconfig -v |grep text
        libgettextpo.so.0 -> libgettextpo.so.0.4.0
        libktexteditor.so.0 -> libktexteditor.so.0.0.0
        libkpimtextedit.so.4 -> libkpimtextedit.so.4.3.0
        libgettextlib-0.17.so -> libgettextlib.so
        libktexteditor.so.4 -> libktexteditor.so.4.3.0
        libgettextsrc-0.17.so -> libgettextsrc.so

make-4.3    ./configure ;  make; make install
make -v
GNU Make 4.3
Built for x86_64-pc-linux-gnu
Copyright (C) 1988-2020 Free Software Foundation, Inc.
License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.
```

 编译binutil支持最新的汇编直流，避免gnutls和samba编译报错
```
binutils-2.38  ./configure ;  make; make install
[root@micapssvr01 binutils-2.38]# ldconfig -v |grep bin
        libnss_winbind.so.2 -> libnss_winbind.so.2
        libdcerpc-binding.so.0 -> libdcerpc-binding.so.0.0.1
        libbind9.so.80 -> libbind9.so.80.0.4
        libkabinterfaces.so.4 -> libkabinterfaces.so.4.3.0
        libini_config.so.5 -> libini_config.so.5.0.0 
 
Python366
./configure --enable-optimizations ;  make -j16; make install
 ldconfig -v |grep pyth
        libboost_python.so.5 -> libboost_python.so.5
        libboost_python-mt.so.5 -> libboost_python-mt.so.5
        libpython2.6.so.1.0 -> libpython2.6.so.1.0
        libpyglib-2.0-python.so.0 -> libpyglib-2.0-python.so.0.0.0
```
编译安装openssl		1.1.1p
```
openssl-1.1.1p]# ./config ; make -j16; make install

openssl version
OpenSSL 1.1.1p  21 Jun 2022

rpm -e openssl-devel-1.0.1e-42.el6.x86_64 mysql-devel-5.1.73-5.el6_6.x86_64

```
编译安装nettle-3.6 必须3.6，其它版本报错不能是3.8
```
./configure ;make -j 16; make install
configure: summary of build options:

  Version:           nettle 3.6
  Host type:         x86_64-pc-linux-gnu
  ABI:               64
  Assembly files:    x86_64/fat x86_64
  Install prefix:    /usr/local
  Library directory: ${exec_prefix}/lib64
  Compiler:          gcc
  Static libraries:  yes
  Shared libraries:  yes
  Public key crypto: yes
  Using mini-gmp:    no
  Documentation:     yes

pkg-config --modversion nettle
3.6
```

编译安装GnuTls
```
./configure                            \
--without-p11-kit                      \
--without-tpm                          \
--with-included-libtasn1               \
--with-included-unistring            \
--disable-doc                        \
--without-unbound-root-key-file
make -j 32; make install 

 gnutls-3.7.6]# ldconfig -v |grep gnu
        libgnutlsxx.so.26 -> libgnutlsxx.so.26.14.12
        libgnutls.so.26 -> libgnutls.so.26.14.12
        libgnutls-extra.so.26 -> libgnutls-extra.so.26.14.12
 pkg-config --modversion gnutls
3.7.6

```

编译安装libtasn1-4.18.0
```
./configure 
configure: Summary of build options:

  Version:           4.18.0
  Libtool version    12:2:6
  DLL version:
  Header version:    major 4 minor 18 patch 0 number 0x041200
  Build/host system: x86_64-pc-linux-gnu / x86_64-pc-linux-gnu
  Install prefix:    /usr/local
  Compiler:          gcc
  Warning flags:      -fno-common -Wall -Wbad-function-cast -Wdate-time -Wdisabled-optimization -Wdouble-promotion -Wduplicated-cond -Wextra -Wformat-signedness -Winit-self -Winline -Winvalid-pch -Wlogical-op -Wmissing-declarations -Wmissing-include-dirs -Wmissing-prototypes -Wnested-externs -Wnull-dereference -Wold-style-definition -Wopenmp-simd -Wpacked -Wpointer-arith -Wshadow -Wstack-protector -Wstrict-overflow -Wstrict-prototypes -Wsuggest-attribute=const -Wsuggest-attribute=format -Wsuggest-attribute=noreturn -Wsuggest-final-methods -Wsuggest-final-types -Wsync-nand -Wtrampolines -Wuninitialized -Wunknown-pragmas -Wunsafe-loop-optimizations -Wvariadic-macros -Wvector-operation-performance -Wvla -Wwrite-strings -Warray-bounds=2 -Wformat=2 -Wshift-overflow=2 -Wunused-const-variable=2 -Wno-type-limits -Wno-unused-parameter -fdiagnostics-show-option -fdiagnostics-color=always
  CFLAGS:            -g -O2
  LDFlags:
  Documentation:     yes
  Library types:     Shared=yes, Static=yes
  Valgrind:          yes valgrind -q --error-exitcode=1 --leak-check=full
  Version script:    yes
  Fuzzing build:     no

make ; make install
 ldconfig -v |grep asn
        libtasn1.so.3 -> libtasn1.so.3.1.6


```
## 3、编译安装Samba 4.16.2
```
export LD_LIBRARY_PATH=/usr/local/lib:/usr/local/lib64:/usr/lib:/usr/lib64
export PKG_CONFIG_PATH=/usr/local/lib64/pkgconfig:/usr/lib64/pkgconfig:/usr/local/lib/pkgconfig:/usr/lib/pkgconfig
 pkg-config  --modversion gnutls
2.8.5
export PKG_CONFIG_PATH=/usr/local/lib64/pkgconfig:/usr/local/lib/pkgconfig:/usr/lib64/pkgconfig:/usr/lib/pkgconfig
[root@micapssvr01 samba-4.16.2]# pkg-config  --modversion gnutls
3.7.6
```
删除系统Guntls
```
ls /usr/lib64/*gnu*
 tar cvf /root/libgnutls.2.85.tar /usr/lib64/*gnu*
 cd  /usr/lib64
 rm -rf *gnu*


```


```
export LIBRARY_PATH=$LD_LIBRARY_PATH
./configure  --disable-python --without-ad-dc --disable-cups   --disable-iprint  --without-json --without-ldap \
 --without-acl-support  --without-libarchive --without-ads --without-pam  --with-shared-modules='!vfs_snapper,!vfs_gpfs' \
 --disable-glusterfs  --disable-cephfs     --without-acl-support      --with-gnutls=/usr/local                       \
--prefix=/usr/local/samba4.16.2

--with-gnutls

Checking for openpty                                                                            : not found
Checking for library util                                                                       : yes
Checking for openpty in util                                                                    : ok
Checking for header linux/sockios.h                                                             : yes
Checking for header linux/ethtool.h                                                             : yes
Checking for system libtasn1 (>=3.8)                                                            : yes
Checking for program 'asn1Parser'                                                               : /usr/local/bin/asn1Parser
Checking linker accepts -Wl,-no-undefined                                                       : yes
Checking linker accepts ['-undefined', 'dynamic_lookup']                                        : no
Checking linker accepts -Wl,--as-needed                                                         : yes
-lc not needed                                                                                  : -lc is unnecessary
Checking configure summary                                                                      : ok
Checking compiler for PIE support                                                               : yes
Checking compiler for full RELRO support                                                        : yes
Checking if compiler accepts -fstack-protector-strong                                           : yes
Checking if compiler accepts -fstack-clash-protection                                           : no
'configure' finished successfully (1m32.761s)



