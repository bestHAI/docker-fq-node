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

## 自签证书

```
# 必要的变量：
export DOMAIN_TEM=你的域名
export OUTPUT_PATH_TEM=部署的目录

# 安装acme：
curl https://get.acme.sh | bash

# 安装socat：
yum install -y socat

# 添加软链接：
ln -s ~/.acme.sh/acme.sh /usr/local/bin/acme.sh

# 切换CA机构：
acme.sh --set-default-ca --server letsencrypt

# 申请证书：
acme.sh --issue -d $DOMAIN_TEM --standalone -k ec-256

# 安装证书：
acme.sh --installcert -d $DOMAIN_TEM --ecc --key-file $OUTPUT_PATH_TEM/$DOMAIN_TEM.key --fullchain-file $OUTPUT_PATH_TEM/$DOMAIN_TEM.pem
```