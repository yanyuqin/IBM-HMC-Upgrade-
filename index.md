#总体说明
 项目背景：梳理业务环境，调整系统架构。使用PowerVM RemoteStart代替HACMP。
 主要工作：升级HMC2台、升级系统微码、升级VIOS、升级板块微码、升级安全补丁（openssl、openssh、ntpv4）
##全局流程
```mermaid
graph TD
    A[升级HMC2台] -->| 多次重启 | B(升级空闲的8286-42A)
    B --> | 重启 | C(升级RAID块微码)
   
