## 克隆升级VIOS  from 2.2.4.0 to 2.2.6.65


https://developer.ibm.com/articles/au-aix-updating-vios-to-higher-level/

```
root: 
loopmount -i /NAS/H22065994.iso -o "-V cdrfs -o ro " -m /mnt
ls /mnt
lspv

#如果有虚拟光驱需要unmirror前删除，否则unmirror报错
   umount /var/vio/VMLibrary
   rmrep -f

padmin:
unmirrorios hdisk1
reducevg -rmlv rootvg hdisk1
bootlist -mode normal -ls
lspv

alt_root_vg -target hdisk1 -bundle update_all -location /mnt 2>&1|tee alt_root_updateall.log
oem_setup_env

tail /var/adm/ras/install_all_updates.log
sync
lspv
shutdown -Fr
```
重启后检查ioslevel，配置设备规则，提交update后再次重启
```
rulescfgset  #配置设备规则
updateios -commit
shutdown -restart -force
```

重新镜像
```
extendvg rootvg <new hdisk#>
mirrorios -defer <new hdisk#>
bootlist -mode normal -ls (to view)
bootlist -mode normal <new hdisk#>
bootlist -mode normal -ls (to view)
oem_setup_env (to get root shell)
bosboot -ad <new hdisk#> (using root shell)
exit (using root shell, to go back to VIOS shell from root shell)
```
