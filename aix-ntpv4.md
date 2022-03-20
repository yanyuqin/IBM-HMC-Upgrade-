# 下载NTPV4并安装
[NTP下载](https://www-01.ibm.com/marketing/iwm/iwm/web/pickUrxNew.do?source=aixbp) 

``` 
VIOS：
cd /soft/download
updateios -dev .  -install -accept
AIX：
installp -acgXY -d . all
```
# 手工切换到V4

``` 
cd /usr/sbin
stopsrc -s xntpd
ln -sf /usr/sbin/ntp4/ntpdate4 ntpdate
ln -sf /usr/sbin/ntp4/ntpq4 ntpq
ln -sf /usr/sbin/ntp4/ntptrace4 ntptrace
ln -sf /usr/sbin/ntp4/sntp4 sntp
ln -sf /usr/sbin/ntp4/ntpd4 xntpd
ln -sf /usr/sbin/ntp4/ntpdc4 xntpdc
```
# 修改自启动和系统守护

``` 
chssys -s xntpd -Q -R  #守护和禁止多实例
chrctcp -S -a xntpd   #随系统自启动，并且启动服务包含startsrc
```
# V3与V4对比

``` 
# lssrc -ls xntpd    #V3
 Program name:    /usr/sbin/xntpd
 Version:         3
 Leap indicator:  11 (Leap indicator is insane.)
 Sys peer:        no peer, system is insane
 Sys stratum:     16
 Sys precision:   -17
 Debug/Tracing:   DISABLED
 Root distance:   0.000000
 Root dispersion: 0.000000
 Reference ID:    no refid, system is insane
 Reference time:  no reftime, system is insane
 Broadcast delay: 0.003906 (sec)
 Auth delay:      0.000122 (sec)
 System flags:    pll monitor filegen
 System uptime:   8 (sec)
 Clock stability: 0.000000 (sec)
 Clock frequency: 0.000000 (sec)
 Peer: 10.x.x.x
      flags: (configured)
      stratum:  1, version: 3
      our mode: client, his mode: server
Subsystem         Group            PID          Status
 xntpd            tcpip            10551496     active
 
# lssrc -ls xntpd
 Program name:    xntpd
 Version:         4
 Leap indicator:  11 (Leap indicator is insane.)
 Sys peer:        no peer, system is insane
 Sys stratum:     16
 Sys precision:   -20
 Debug/Tracing:   DISABLED
 Root distance:   0.000000
 Root dispersion: 0.000000
 Reference ID:    73.78.73.84
 Reference time:  no reftime, system is insane
 Broadcast delay: 0.000000 (sec)
 Auth delay:      +0.000000 (sec)
 System flags:    auth monitor filegen
 System uptime:   16 (sec)
 Clock stability: 0.000000 (sec)
 Clock frequency:
Subsystem         Group            PID          Status
 xntpd            tcpip            11665412     active
```


