# Docker FQ Node

## 拥塞算法

```
# 查询当前使用的 TCP 拥塞控制算法
sysctl net.ipv4.tcp_congestion_control
# 查询当前Linux版本
uname -r

# 启用BBR TCP拥塞控制算法
echo "net.core.default_qdisc=fq" >> /etc/sysctl.conf
echo "net.ipv4.tcp_congestion_control=bbr" >> /etc/sysctl.conf
sysctl -p
```

## 开放端口

```
iptables -I INPUT -p tcp --dport 80 -j ACCEPT
iptables -I INPUT -p tcp --dport 443 -j ACCEPT
iptables -I INPUT -p tcp --dport 8083 -j ACCEPT
```