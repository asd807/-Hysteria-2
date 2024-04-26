一键安装Hysteria 2


bash <(curl -fsSL https://get.hy2.sh/)


设置自启动Hysteria 2


systemctl enable hysteria-server.service


生成自签证书


openssl req -x509 -nodes -newkey ec:<(openssl ecparam -name prime256v1) -keyout /etc/hysteria/server.key -out /etc/hysteria/server.crt -subj "/CN=bing.com" -days 36500 && sudo chown hysteria /etc/hysteria/server.key && sudo chown hysteria /etc/hysteria/server.crt
服务器配置代码：


cat << EOF > /etc/hysteria/config.yaml
listen: :443

tls:
  cert: /etc/hysteria/server.crt
  key: /etc/hysteria/server.key

auth:
  type: password
  password: 123456 
  
masquerade:
  type: proxy
  proxy:
    url: https://bing.com 
    rewriteHost: true
EOF
启动Hysteria 2

Bash
systemctl start hysteria-server.service
查看启动状态

Bash
systemctl status hysteria-server.service
关闭防火墙

Bash
ufw disable
重启Hysteria 2

Bash
systemctl restart hysteria-server.service


停止Hysteria 2

Bash
systemctl stop hysteria-server.service
客户端配置代码：

Bash
server: ip:443
auth: 123456

bandwidth:
  up: 20 mbps
  down: 100 mbps
  
tls:
  sni: bing.com 
  insecure: true

socks5:
  listen: 127.0.0.1:1080
http:
  listen: 127.0.0.1:8080
v2下载地址：https://github.com/2dust/v2rayN/releases/tag/6.23

Hysteria下载地址：https://github.com/apernet/hysteria/releases
