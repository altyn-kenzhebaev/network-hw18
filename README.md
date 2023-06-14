# Архитектура сетей 
Для выполнения этого действия требуется установить приложением git:
`git clone https://github.com/altyn-kenzhebaev/network-hw18.git`
В текущей директории появится папка с именем репозитория. В данном случае network-hw18. Ознакомимся с содержимым:
```
cd network-hw18
ls -l
README.md
ansible
Vagrantfile
```
Здесь:
- ansible - папка с плэйбуками
- README.md - файл с данным руководством
- Vagrantfile - файл описывающий виртуальную инфраструктуру для `Vagrant`
Запускаем ВМ:
```
vagrant up
```
# Теоретическая часть
## Найти свободные подсети
> Сеть office1
> - 192.168.2.0/26 - dev
> - 192.168.2.64/26 - test servers
> - 192.168.2.128/26 - managers
> - 192.168.2.192/26 - office hardware

> Сеть office2
> - 192.168.1.0/25 - dev
> - 192.168.1.128/26 - test servers
> - 192.168.1.192/26 - office hardware

> Сеть central
> - 192.168.0.0/28 - directors
> - 192.168.0.32/28 - office hardware
> - 192.168.0.64/26 - wifi

**Cвободные подсети:**
 - 192.168.0.16/28
 - 192.168.0.48/28
 ---
 - 192.168.0.16/27
 ---
 - 192.168.0.128/25
 ---
 - 192.168.0.128/26
 - 192.168.0.192/26
 ---
 - 192.168.0.128/27
 - 192.168.0.160/27
 - 192.168.0.192/27
 - 192.168.0.224/27
 ---
 - 192.168.0.128/28
 - 192.168.0.144/28
 - 192.168.0.160/28
 - 192.168.0.176/28
 - 192.168.0.192/28
 - 192.168.0.208/28
 - 192.168.0.224/28
 - 192.168.0.240/28

## Посчитать сколько узлов в каждой подсети, включая свободные
Наименование подсетей были взяты из Vagrantfile:
 - Подсеть router-net-central 192.168.255.0/30 (255.255.255.252) имеюся 2 узла: 192.168.255.1, 192.168.255.2 и больше не может иметь узлов
 - Подсеть dir-net 192.168.0.0/28 (255.255.255.240) имеюся 2 узла: 192.168.0.1, 192.168.0.2 и 12 свободных узлов
 - Подсеть hw-net 192.168.0.32/28 (255.255.255.240) имеюся 1 узeл: 192.168.0.33 и 13 свободных узлов
 - Подсеть wifi-net 192.168.0.64/26 (255.255.255.192) имеюся 1 узeл: 192.168.0.65 и 61 свободный узел
 - Подсеть office1-central 192.168.255.4/30 (255.255.255.252) имеюся 2 узла: 192.168.255.5, 192.168.255.6 и больше не может иметь узлов
 - Подсеть office2-central 192.168.255.8/30 (255.255.255.252) имеюся 2 узла: 192.168.255.9, 192.168.255.10 и больше не может иметь узлов
 - Подсеть dev-office1-net 192.168.2.0/26 (255.255.255.192) имеюся 1 узeл: 192.168.2.1 и 61 свободный узел
 - Подсеть test-office1-net 192.168.2.64/26 (255.255.255.192) имеюся 1 узeл: 192.168.2.65 и 61 свободный узел
 - Подсеть mngr-office1-net 192.168.2.128/26 (255.255.255.192) имеюся 2 узла: 192.168.2.129, 192.168.2.129 и 60 свободных узлов
 - Подсеть hw-office1-net 192.168.2.192/30 (255.255.255.252) имеюся 1 узeл: 192.168.2.193 и 1 свободный узел
 - Подсеть dev-office2-net 192.168.1.0/25 (255.255.255.128) имеюся 1 узeл: 192.168.1.1 и 125 свободных узлов
 - Подсеть test-office2-net 192.168.1.128/26 (255.255.255.192) имеюся 2 узла: 192.168.1.129, 192.168.1.130 и 60 свободных узлов
 - Подсеть hw-office2-net 192.168.1.192/30 (255.255.255.252) имеюся 1 узeл: 192.168.1.193 и 1 свободный узел
## Указать broadcast адрес для каждой подсети
 - Подсеть router-net-central 192.168.255.0/30 (255.255.255.252) broadcast 192.168.255.3
 - Подсеть dir-net 192.168.0.0/28 (255.255.255.240) broadcast 192.168.0.15
 - Подсеть hw-net 192.168.0.32/28 (255.255.255.240) broadcast 192.168.0.47
 - Подсеть wifi-net 192.168.0.64/26 (255.255.255.192) broadcast 192.168.0.127
 - Подсеть office1-central 192.168.255.4/30 (255.255.255.252) broadcast 192.168.255.7
 - Подсеть office2-central 192.168.255.8/30 (255.255.255.252) broadcast 192.168.255.11
 - Подсеть dev-office1-net 192.168.2.0/26 (255.255.255.192) broadcast 192.168.2.63
 - Подсеть test-office1-net 192.168.2.64/26 (255.255.255.192) broadcast 192.168.2.127
 - Подсеть mngr-office1-net 192.168.2.128/26 (255.255.255.192) broadcast 192.168.2.191
 - Подсеть hw-office1-net 192.168.2.192/30 (255.255.255.252) broadcast 192.168.2.195
 - Подсеть dev-office2-net 192.168.1.0/25 (255.255.255.128) broadcast 192.168.1.127
 - Подсеть test-office2-net 192.168.1.128/26 (255.255.255.192) broadcast 192.168.1.191
 - Подсеть hw-office2-net 192.168.1.192/30 (255.255.255.252) broadcast 192.168.1.195

# Практическая часть
## Соединить офисы в сеть согласно схеме и настроить роутинг + Все сервера должны видеть друг друга + отключение дефолт на нат (eth0)
Роутинг настраивается согласно установке маршрута по-умолчанию и включения параметра net.ipv4.conf.all.forwarding
Для ***inetRouter*** необходимо указать путь до следующих подсетей через 192.168.255.2:
```
192.168.1.128/26 via 192.168.255.2
192.168.2.128/26 via 192.168.255.2
192.168.255.4/30 via 192.168.255.2
192.168.255.8/30 via 192.168.255.2
192.168.0.0/28 via 192.168.255.2
```
Для роутеров ***centralRouter,office1Router,office2Router*** и серверов ***centralServer,office1Server,office2Server***  необходимо установить свой маршрут по-умолчанию и отключить дефолт на нат (eth0) (Выжимка из ansible):
```
  - name: add default gateway for centralServer
    lineinfile:
      dest: /etc/sysconfig/network-scripts/ifcfg-eth0
      line: DEFROUTE=no

  - name: add default gateway for centralServer
    lineinfile:
      dest: /etc/sysconfig/network-scripts/ifcfg-eth1
      line: GATEWAY=192.168.*.*     # <-- Тут прописываем свой default gateway
```
Дополнительно для ***centralRouter*** необходимо также указать путь до следующих подсетей через 192.168.255.6, 192.168.255.10
```
$ echo /etc/sysconfig/network-scripts/ifcfg-eth5
192.168.2.128/26 via 192.168.255.6
$ /etc/sysconfig/network-scripts/route-eth6
192.168.1.128/26 via 192.168.255.10
```
## Все сервера и роутеры должны ходить в инет черз inetRouter
Для настравиается файрволл с в ключениям NAT (Выжимка из ansible):
```
 - name: Stop and disable Firewalld
    systemd:
      name: firewalld
      state: stopped
      enabled: false

  - name: Install iptables
    yum:
      name:
      - iptables
      - iptables-services
      state: present
      update_cache: true
      
  - name: Copy iptables config
    copy:
      dest: /etc/sysconfig/iptables
      src: iptables
      owner: root
      group: root
      mode: 0600

  - name: Start and enable iptables
    systemd:
      name: iptables
      state: restarted
      enabled: true
$ echo /etc/sysconfig/iptables
# Generated by iptables-save v1.4.21 on Mon Jan 17 09:53:32 2022
*filter
:INPUT ACCEPT [0:0]
:FORWARD ACCEPT [0:0]
:OUTPUT ACCEPT [37:2828]
-A INPUT -m state --state RELATED,ESTABLISHED -j ACCEPT
-A INPUT -p icmp -j ACCEPT
-A INPUT -i lo -j ACCEPT
-A INPUT -p tcp -m state --state NEW -m tcp --dport 22 -j ACCEPT
# Deny ping trafic
# -A INPUT -j REJECT --reject-with icmp-host-prohibited
# -A FORWARD -j REJECT --reject-with icmp-host-prohibited
COMMIT
# Completed on Mon Jan 17 09:53:32 2022
# Generated by iptables-save v1.4.21 on Mon Jan 17 09:53:32 2022
*nat
:PREROUTING ACCEPT [1:161]
:INPUT ACCEPT [0:0]
:OUTPUT ACCEPT [0:0]
:POSTROUTING ACCEPT [0:0]
-A POSTROUTING ! -d 192.168.0.0/16 -o eth0 -j MASQUERADE
COMMIT
# Completed on Mon Jan 17 09:53:32 2022
```