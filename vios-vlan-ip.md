## VIOS修改登录提示及shell
修改后支持主机名和目录作为PS1显示，同时支持上下键翻页
```
$ oem_setup_env

# echo "export ENV=/home/padmin/.kshrc" >> /home/padmin/.profile

# echo "export HOST=\"\$(/usr/bin/uname -n)\"\nif [ \"\`whoami\`\" = \"root\" ]; then\n  PS1=\"\`whoami\`@\$HOST:\\\$PWD # \"\nelse\n  PS1=\"\`whoami\`@\$HOST:\\\$PWD $ \"\nfi\n\nset -o emacs;alias -x __A=\"\$(echo '\\\\020')\"; alias -x __B=\"\$(echo '\\\\016')\"; alias -x __C=\"\$(echo '\\\\006')\"; alias -x __D=\"\$(echo '\\\\002')\"; alias -x __H=\"\$(echo '\\\\001')\"; alias -x __Y=\"\$(echo '\\\\005')\"\n" >> /home/padmin/.kshrc

#批处理
dsh -c -g vioshosts "oem_setup_env<< EOF
hostname 
cp /NAS/kshrc /home/padmin/.kshrc
cp /NAS/padmin.profile /home/padmin/.profile

EOF"

```

## VIOS 增加VLAN脚本
HMC上执行
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
HMC上执行

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
HMC上执行
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

##方案1
# /usr/sbin/rsct/install/bin/recfgct -n            #resets the node id file (default option)
# /usr/sbin/rsct/install/bin/recfgct -s            #saves the node id file.

##方案2
[root@testsrv]/root# /usr/sbin/rsct/install/bin/uncfgct -n
[root@testsrv]/root# /usr/sbin/rsct/install/bin/cfgct

/usr/sbin/rsct/install/bin/cfgct

Restart RMC on AIX client.
[root@aix:/] /usr/sbin/rsct/bin/rmcctrl -z
[root@aix:/] /usr/sbin/rsct/bin/rmcctrl -A
[root@aix:/] /usr/sbin/rsct/bin/rmcctrl -p

```
## 创建虚拟网卡配置IP地址
``` 
cat  hp-lpm-ip.cfg |while read hostn ipaddr
do
#ssh $host "ioscli hostname;ioscli mkvdev -vlan ent25 -tagid 346"
echo $hostn
ssh -n $hostn "ioscli mkvdev -vlan ent25 -tagid 346"
echo "en26 add ip address"
ssh -n $hostn "ioscli mktcpip -hostname $hostn-lpm -interface en26 -inetaddr $ipaddr -netmask 255.255.255.0"
ssh -n $hostn "chdev -dev en26 -attr mtu_bypass=on mtu=9000 rfc1323=1 tcp_recvspace=262144 tcp_sendspace=262144 tcp_nodelay=1"
done
```



