## IBM微码更新检查工具
[参考网站](https://esupport.ibm.com/customercare/mds/fetch?page=home.html)

使用AIX命令检查当前微码与最新微码直接差距
需要下载最新的微码目录放在


```
# pwd
/var/adm/invscout
# cd mic*
# pwd
/var/adm/invscout/microcode
# ls -la
total 656
drwxrwxrwx    2 root     system          256 May 18 2018  .
drwxr-xr-x   10 root     system         4096 Apr 02 00:06 ..
-rw-r--r--    1 root     system       327962 Apr 02 00:11 catalog.mic
```


[最新微码目录](https://www3.software.ibm.com/ibmdl/pub/software/server/firmware/catalog.mic) 

在AIX上执行
```
# invscout

******  Command  ----  V2.2.0.20
******  Logic Database V2.2.0.2

Initializing ...
Identifying the system ...
Working ...
Getting system microcode level(s) ...
Scanning for device microcode level(s) ...

210 devices detected; each dot (.)
represents 10 devices processed:
....................

Writing Microcode Survey upload file ...

Microcode Survey complete

The output files can be found at:
Upload file: /var/adm/invscout/FY4ANODE48VIOSA.mup
Report file: /var/adm/invscout/invs.mrp
Report file: /var/adm/invscout/invs.mrrup

To transfer the invscout 'Upload file' for microcode
comparison, see your service provider's web page.
```
## 上传运行差异结果
[上传链接](https://esupport.ibm.com/customercare/mds/fetch?page=upload.jsp)
上传后马上得到需要更新的系统与IO微码，同时提供下载链接

