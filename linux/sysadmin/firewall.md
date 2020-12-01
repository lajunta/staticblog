A Firewall
===

iptables -F
iptables -t nat -F
iptables -X
iptables -t filter -N DOCKER

iptables -A INPUT -i lo -j ACCEPT

iptables -P INPUT DROP
iptables -P OUTPUT ACCEPT
iptables -P FORWARD ACCEPT

iptables -A INPUT -p tcp -m state --state ESTABLISHED -j ACCEPT
iptables -A INPUT -p tcp -m multiport --dports 20,25,80,443 -j ACCEPT
iptables -A INPUT -p udp -m multiport --dports 8000,8001 -j ACCEPT
iptables -A INPUT -p udp -s 114.114.114.114 --sport 53 -j ACCEPT
iptables -A INPUT -p udp -s 114.114.114.114 --sport 8087 -j ACCEPT