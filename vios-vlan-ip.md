## VIOS 增加VLAN脚本
``` 
for i in `lssyscfg -r sys -F name`
do 
   echo "$i vios1 add vlan 342"
   chhwres -r virtualio --rsubtype eth -m $i -o s --id 1 -s 101 -a "addl_vlan_ids+=342"
   echo "$i vios1 add vlan 346"
   chhwres -r virtualio --rsubtype eth -m $i -o s --id 1 -s 102 -a "addl_vlan_ids+=346"
   echo "$i vios2 add vlan 342"
   chhwres -r virtualio --rsubtype eth -m $i -o s --id 2 -s 101 -a "addl_vlan_ids+=342"
   echo "$i vios2 add vlan 346"
   chhwres -r virtualio --rsubtype eth -m $i -o s --id 2 -s 102 -a "addl_vlan_ids+=346"
done
```
有报错，有两个VIOS RMC未建立连接。 *HSCL2957 管理控制台与分区 2 之间当前未建立 RMC 连接，或者该分区不支持动态分区操作。* 
## 保存当前profile脚本

``` 
for i in `lssyscfg -r sys -F name`
do 
   echo "frame $i "
   lssyscfg -r lpar -m $i -F name,default_profile |grep vios |sed s/,/\ /g| while read lpn lpp
   do 
   echo "save  $i vios $lpn profile";
   mksyscfg -r prof -m $i -o save -p $lpn  -n $lpp --force
   done
done
```

## 检查哪个出错VIOS
``` 
 for i in `lssyscfg -r sys -F name`
 do 
 lssyscfg -r prof -m $i -F lpar_name,virtual_eth_adapters | grep vios|grep 102|grep -v 346
 echo 
 done
```

## RMC故障处理

[参考](http://emmanuel.iffly.free.fr/doku.php?id=aix:rsct_reconfig) 

``` 
cat /etc/ct_node_id
#Reconfigure RSCT ID on AIX client.
# /usr/sbin/rsct/bin/lsnodeid
2641-036 The node does not have an assigned node identifier.

11111
# /usr/sbin/rsct/install/bin/recfgct -n            #resets the node id file (default option)
# /usr/sbin/rsct/install/bin/recfgct -s            #saves the node id file.

222222
[root@testsrv]/root# /usr/sbin/rsct/install/bin/uncfgct -n
[root@testsrv]/root# /usr/sbin/rsct/install/bin/cfgct

/usr/sbin/rsct/install/bin/cfgct

Restart RMC on AIX client.
[root@aix:/] /usr/sbin/rsct/bin/rmcctrl -z
[root@aix:/] /usr/sbin/rsct/bin/rmcctrl -A
[root@aix:/] /usr/sbin/rsct/bin/rmcctrl -p

```


