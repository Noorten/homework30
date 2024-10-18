# homework30
после запуска вагрантфайла устанавливаем knockd
sudo apt install -y knockd

Редактируем настройки важно установить прослуживаемый интерфейс и порты в которые мы будем стучать
sudo vim /etc/knockd.conf 

[options]
 UseSyslog
 Interface = eth1

[SSH]
 sequence    = 7000,8000,9000
 seq_timeout = 15
 tcpflags    = syn
 start_command     = /sbin/iptables -I INPUT -s %IP% -p tcp --dport 22 -j ACCEPT
 stop_command     = /sbin/iptables -D INPUT -s %IP% -p tcp --dport 22 -j ACCEPT
 cmd_timeout   = 60

включаем
sudo vim /etc/default/knockd
START_KNOCKD=1
KNOCKD_OPTS="-i eth1"

стартуем
sudo systemctl start knockd
sudo systemctl enable knockd

проверка на скиншоте 1

Второе задание
на centralRouter нужно выполнить следующие команды
Включаем проброс 
sudo sysctl -w net.ipv4.ip_forward=1
Указываем порты и направления
sudo iptables -t nat -A PREROUTING -p tcp --dport 8080 -j DNAT --to-destination 192.168.255.13:80
sudo iptables -t nat -A POSTROUTING -p tcp -d 192.168.255.13 --dport 80 -j MASQUERADE
Результат на скриншоте 2
