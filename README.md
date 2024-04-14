# ssa2004demo

### Общая информация

<details>
<summary>ТЫКНИ</summary>

ДЕМО ЭКЗАМЕН, ПРИМЕР ВЫПОЛНЕНИЯ

Оценочные материалы демонстрационного экзамена 2024 года 09.02.06 «Сетевое и системное администрирование»

    https://bom.firpo.ru/

КОД 09.02.06-1-2024 Том 1

    https://bom.firpo.ru/file/9791/%D0%9A%D0%9E%D0%94%2009.02.06-1-2024%20%D0%A2%D0%BE%D0%BC%201.pdf

[Задание Модуль 1](http://wiki.prcit.ru/Demo-2024/%D0%9C%D0%BE%D0%B4%D1%83%D0%BB%D1%8C-1)

В данном варианте решения предполагается использовать RedOS 7.3.4 Сервер минимальный и RedOS 7.3.4 Рабочая станция

    https://files.red-soft.ru/redos/7.3/x86_64/iso/redos-MUROM-7.3.4-20231220.0-Everything-x86_64-DVD1.iso
калькулятор ipv4

    https://ipmeter.ru/
калькулятор ipv6

    https://www.coderstool.com/ipv6-subnet-calculator
drawio

    https://app.diagrams.net/
    
)

</details>

# КАРТИНОЧКИИИ

<details>
<summary>ТЫКНИ</summary>

![topology](https://github.com/be2glaz/demoSSA/assets/89695370/51869b97-0f01-4894-be7b-8880d9043859)

![tab_1](https://github.com/be2glaz/demoSSA/assets/89695370/db56b436-90ba-4555-9e80-40e25aa7e930)

![ip_adr_tabl(2)](https://github.com/be2glaz/demoSSA/assets/89695370/54fb9f81-34f2-41df-9df3-021bf54f87fd)

![l3_topologiya_(2)](https://github.com/be2glaz/demoSSA/assets/89695370/7c06c5fe-8100-4fa4-b5c0-326c8ba4f106)


</details>

## 1. Базовая настройка

<details>
<summary>ТЫКНИ</summary>
    
![topology](https://github.com/beglaz/ssa004demo/assets/89695370/5447aa7-573-4f55-b19-bf314e30f1ec)

1. Выполните базовую настройку всех устройств:
a. Присвоить имена в соответствии с топологией
b. Рассчитать IP-адресацию IPv4 и IPv6. Необходимо заполнить таблицу №1, чтобы эксперты могли проверить ваше рабочее место.
c. Пул адресов для сети офиса BRANCH - не более 16
d. Пул адресов для сети офиса HQ - не более 64

![tab_1](https://github.com/beglaz/ssa004demo/assets/89695370/a48d854b-784-4f67-8318-ce1c1a6ead)


![ip_adr_tabl()](https://github.com/beglaz/ssa004demo/assets/89695370/91c000bb-9a96-4017-8ee9-86e59870074c)


а. Присвоить имена в соответствии с топологией
Имена устройств (hostname) – прописывать строчными символами (маленькими буквами)

    [root@localhost ~]# hostnamectl set-hostname <NAME>
    [root@localhost ~]# exec bash

NAME - имя устройства

exec bash — перезапуск оболочки bash для отображения нового хостнейма

Для устройств BR-SRV и CLI желательно сразу установить полное доменное имя. Потребуется для ввода этих машин в домен во второй части задания.

> Например:
> - ISP: isp
> - CLI: cli.hq.work
> - HQ-R: hq-r.hq.work
> - HQ-SRV: hq-srv.hq.work
> - BR-R: br-r.branch.work
> - BR-SRV: br-srv.branch.work

Пример:

![1-1](https://github.com/beglaz/ssa004demo/assets/89695370/cb447ca-e79-496b-8643-97fe1d349fe8)


b. Рассчитать IP-адресацию IPv4 и IPv6. Необходимо заполнить таблицу №1, чтобы эксперты могли проверить ваше рабочее место.

c. Пул адресов для сети офиса BRANCH - не более 16


> [!WARNING]
> 
> - Для пула адресов IPv4 не более 16 - маска подсети /28
> - Для пула адресов IPv6 не более 16 - длина префикса /124



d. Пул адресов для сети офиса HQ - не более 64


> [!WARNING]
> 
>  - Для пула адресов IPv4 не более 64 - маска подсети /26
>  - Для пула адресов IPv6 не более 64 - длина префикса /122

</details>

### Настройка сетевых интерфейсов

<details>
<summary>ТЫКНИ</summary>

**ISP**
Определяемся имена интерфейсов и какой интерфейс в какую сторону смотрит

Выводим информацию о сетевых интерфейсах:

    # ip -c a

![1-2](https://github.com/be2glaz/ssa2004demo/assets/89695370/cf26d254-96d5-495e-9c99-ebf201d9a5c4)

Открываем настройки виртуальной машины

Выбираем необходимую виртуальную машину
Выбираем Оборудование
Смотрим MAC-адрес сетевых интерфейсов, и запоминаем их (лучше записать на черновик)

![1-3](https://github.com/be2glaz/ssa2004demo/assets/89695370/a52f6ddf-f930-466e-87cd-d0426988931a)

С помощью утилиты ```nmtui``` задаем IP адреса сетевым интерфейсам

**Результаты настройки сетевых интерфейсов**

**ISP**

В данном примере получаем:

- ens18 – WAN интерфейс (в Интернет);
- ens19 - интерфейс в сторону офиса HQ;
- ens20 - интерфейс в сторону CLI;
- ens21 - интерфейс в сторону офиса Branch;

![1-4](https://github.com/be2glaz/ssa2004demo/assets/89695370/21e62765-542e-40b0-88b5-ddd9f1971ed0)

**HQ-R**

В данном примере для HQ-R:

- ens18 - интерфейс в сторону ISP;
- ens19 - интерфейс в строну офиса HQ;
- ens20 - интерфейс в сторону CLI (временное подключение) ;

![1-5](https://github.com/be2glaz/ssa2004demo/assets/89695370/128d0d00-366b-4d76-ba91-df4899399c8e)


**HQ-SRV**
Получает IP адрес по DHCP от HQ-R. Настройка описана ниже.

В данном примере для HQ-SRV:

- ens18 - интерфейс в строну офиса HQ;

> Режим КОНФИГУРАЦИЯ IPv4 <Автоматически>
> 
> Изменяем режим КОНФИГУРАЦИЯ IPv6 с <Автоматически> на <Автоматически (только DHCP)>


**BR-R**

В данном примере для HQ-R:

- ens18 - интерфейс в сторону ISP;
- ens19 - интерфейс в строну офиса Branch;

![1-6](https://github.com/be2glaz/ssa2004demo/assets/89695370/9a0c8268-41be-4698-9981-5a78c95dec27)


**BR-SRV**

BR-SRV - 1 интерфейс в сторону BR-R

![1-7](https://github.com/be2glaz/ssa2004demo/assets/89695370/9ede3139-bc65-4611-b36f-9f2cd51a1039)


**CLI**

Настройка интерфейса CLI_ISP

![1-8](https://github.com/be2glaz/ssa2004demo/assets/89695370/ec1ce4b0-79a8-407f-9511-c033ea9e6ef4)

![1-9](https://github.com/be2glaz/ssa2004demo/assets/89695370/23717674-91a4-4d38-8e5f-fe7bfa1324af)

Настройка интерфейса HQ-R_CLI (временное соединение)

Настраивается аналогично CLI_ISP

![1-10](https://github.com/be2glaz/ssa2004demo/assets/89695370/55685e14-7bf5-4134-958f-1d17cda77e41)

</details>

## Настройка доступа в интернет

### Маршрутизация транзитных IP-пакетов

<details>
<summary>ТЫКНИ</summary>

> На устройствах ISP, HQ-R, BR-R необходимо включить пересылку пакетов между интерфейсами - forwarding

Чтобы включить пересылку пакетов между интерфейсами, необходимо отредактировать файл sysctl.conf

    # nano /etc/sysctl.conf
В данном файле прописываем следующие строки:

    net.ipv4.ip_forward=1
    net.ipv6.conf.all.forwarding=1

После необходимо применить внесенные изменения:

    # sysctl -p

> Необходимо предоставить доступ в сеть Интернет для всех устройств предложенных в демо-экзамене для установки необходимых пакетов. Для этого необходимо настроить Nftables на устройствах ISP, HQ-R и BR-R

> Nftables - подсистема ядра Linux, обеспечивающая фильтрацию и классификацию сетевых пакетов/датаграмм/кадров.


#### Настройка nftables на ISP

> Данная настройка позволит получить доступ к сети Интернет с HQ-R и BR-R

Установка nftables

Перед установкой необходимо убедиться что имеется доступ в интернет с ВМ ISP

    ping -c4 ya.ru
Если ping проходит успешно то устанавливаем nftables

    # dnf install -y nftables

#### Настройка nftables
По умолчанию создаются несколько примеров файлов для работы с nftables в директории /etc/ nftables/.

Настройка с использованием собственного файла настроек

Можно не использовать ни один из файлов примеров, а написать свой.

Создаем и открываем файл

    # nano /etc/nftables/isp.nft
Прописываем следующие строки

    table inet my_nat {
            chain my_masquerade {
            type nat hook postrouting priority srcnat;
            oifname "ens18" masquerade
            }
    }
где ```ens18``` - публичный интерфейс ISP (смотрящий в Интернет)

Затем необходимо включить использование данного файла в ```sysconfig``` , по умолчанию ```nftables``` не читает ни один из конфигурационных файлов в ```/etc/nftables```

    # nano /etc/sysconfig/nftables.conf
Ниже строки начинающейся на ```include```, прописываем строку

    include "/etc/nftables/isp.nft"
Запуск и добавление в автозагрузку сервиса ```nftables```

    # systemctl enable --now nftables
> При успешной и правильной настройке машины ```HQ-R``` и ```BR-R``` получат выход в Интернет

> На устройствах ```HQ-R``` и ```BR-R``` необходимо произвести настройку ```Nftables``` аналогичным способом для доступа HQ-SRV и BR-SRV к сети Интернет

### Настройка nftables на HQ-R
Установка nftables

    # dnf install -y nftables
Создаем и открываем фалй

    # nano /etc/nftables/hq-r.nft
Прописываем следующие строки

    table inet my_nat {
            chain my_masquerade {
            type nat hook postrouting priority srcnat;
            oifname "ens18" masquerade
            }
    }
Включаем использование данного файла в ```sysconfig```

    # nano /etc/sysconfig/nftables.conf
Ниже строки начинающейся на ```include```, прописываем строку

    include "/etc/nftables/hq-r.nft"
Запуск и добавление в автозагрузку сервиса ```nftables```

    # systemctl enable --now nftables

### Настройка nftables на BR-R
Установка nftables

    # dnf install -y nftables
Создаем и открываем файл

    # nano /etc/nftables/br-r.nft
Прописываем следующие строки

    table inet my_nat {
            chain my_masquerade {
            type nat hook postrouting priority srcnat;
            oifname "ens18" masquerade
            }
    }
Включаем использование данного файла в ```sysconfig```

    # nano /etc/sysconfig/nftables.conf
Ниже строки начинающейся на ```include```, прописываем строку

    include "/etc/nftables/br-r.nft"
Запуск и добавление в автозагрузку сервиса ```nftables```

    # systemctl enable --now nftables



</details>

## 1.2. Настроить внутреннюю динамическую маршрутизацию по средствам FRR. Выбрать и обосновать выбор протокола динамической маршрутизации из расчёта, что в дальнейшем сеть будет масштабироваться.

<details>
<summary>ТЫКНИ</summary>

##### a. Составьте топологию сети L3.
### Решение

> Маршрутизация внешних сетей по заданию не описана, следовательно, на HQ-R и BR-R достаточно настроить статическую маршрутизацию.
> 
> При настройке IP-адресации в качестве шлюза задать соответствующие адреса маршрутизатора ISP

> Необходимо связать HQ-R и BR-R туннелем. Обмен между внутренними сетями должен происходить строго между маршрутизаторами HQ и BRANCH. ISP не должен иметь к ним прямого доступа.
> 
> Достаточно реализовать простой GRE туннель

#### GRE-туннель между HQ-R и BR-R
> Имена tun0, gre0 и sit0 являются зарезервированными в iproute2 («base devices») и имеют особое поведение.

##### Настройка HQ-R
Так как в РЕД ОС используется NetworkManager - следовательно переходим в nmtui:

    # nmtui
**Производим настройку**
- Выбираем «Изменить подключение»
- Выбираем «Добавить»
- Выбираем «IP-туннель
- Задаём понятные имена «Имя профиля» и «Устройство»
- «Режим работы» выбираем «GRE»
- «Родительский» указываем интерфейс в сторону ISP (ens18)
- Задаём «Локальный IP» (IP на интерфейсе HQ-R в сторону IPS)
- Задаём «Удалённый IP» (IP на интерфейсе BR-R в сторону ISP)
- Переходим к «КОНФИГУРАЦИЯ IPv4»
- Задаём адрес IPv4 для туннеля
- Переходим к «КОНФИГУРАЦИЯ IPv6»
- Задаём адрес IPv6 для туннеля
- Активируем интерфейс tun1

![gre-gif](https://github.com/be2glaz/ssa2004demo/assets/89695370/7a5f7735-22b5-40bc-b905-ad2c73826e6a)

> Для корректной работы протокола динамической маршрутизации требуется увеличить параметр TTL на интерфейсе туннеля:

    # nmcli connection modify tun1 ip-tunnel.ttl 64
Проверяем:

    ip -c a

![2-1](https://github.com/be2glaz/ssa2004demo/assets/89695370/bf42efae-339f-4cf2-9d5c-01a02deed555)

##### Настройка BR-R
Настройка GRE – туннеля на BR-R производится аналогично HQ-R

    # nmtui
**Производим настройку**
- Выбираем «Изменить подключение»
- Выбираем «Добавить»
- Выбираем «IP-туннель
- Задаём понятные имена «Имя профиля» и «Устройство»
- «Режим работы» выбираем «GRE»
- «Родительский» указываем интерфейс в сторону ISP (ens18)
- Задаём «Локальный IP» (IP на интерфейсе BR-R в сторону IPS)
- Задаём «Удалённый IP» (IP на интерфейсе HQ-R в сторону ISP)
- Переходим к «КОНФИГУРАЦИЯ IPv4»
- Задаём адрес IPv4 для туннеля
- Переходим к «КОНФИГУРАЦИЯ IPv6»
- Задаём адрес IPv6 для туннеля
- Активируем интерфейс tun1

> Был создан новый виртуальный интерфейс (туннель) для прямого взаимодействия устройств HQ-R и BR-R. Они будут напрямую обмениваться маршрутами внутренних сетей HQ и BRANCH через это соединение.

Проверяем

<details>
<summary>ТЫКНИ</summary>

HQ-R

![2-2](https://github.com/be2glaz/ssa2004demo/assets/89695370/dbc71991-395b-477c-84ab-fd8a0b6ca039)

BR-R

![2-3](https://github.com/be2glaz/ssa2004demo/assets/89695370/7db6d99e-24b9-433e-800a-c077e5e35fce)

</details>

### Настройка динамической (внутренней) маршрутизации средствами FRR
#### Настройка на HQ-R
Установка пакет frr

    # dnf install -y frr
Для настройки внутренней динамической маршрутизации для IPv4 и IPv6 будет использован протокол ```OSPFv2``` и ```OSPFv3```

Для настройки ```ospf``` необходимо включить соответствующий демон в конфигурации ```/etc/frr/daemons```

    # nano /etc/frr/daemons
В конфигурационном файле ```/etc/frr/daemons``` необходимо активировать выбранный протокол для дальнейшей реализации его настройки:

> ```ospfd = yes``` - для OSPFv2 (IPv4)
>
> ```ospf6d = yes``` - для OSPFv3 (IPv3)

Включаем и добавляем в автозагрузку службу FRR

    # systemctl enable --now frr
Переходим в интерфейс управление симуляцией FRR при помощи vtysh (аналог cisco)

    # vtysh
Настройки OSPFv2 и OSPFv3 на HQ-R

![2-4](https://github.com/be2glaz/ssa2004demo/assets/89695370/a41c38ba-ef9b-4075-ae7b-44d07cf26911)



> OSPFv2
>
> conf t или configure terminal - вход в режим глобальной конфигурации
> router ospf - переход в режим конфигурации OSPFv2
> passive-interface default - перевод всех интерфейсов в пассивный режим
> network - объявляем локальную сеть офиса HQ и сеть (GRE-туннеля)
> exit - выход и режима конфигурации OSPFv2
> туннельный интерфейс tun1 делаем активным, для устанавления соседства с BR-R и обмена внутренними маршрутами
> no ip ospf passive - перевод интерфейса tun1 в активный режим
> do write - сохраняем текущую конфигурацию

> OSPFv3 - ipv6
> 
> router ospf6 - переход в режим конфигурации OSPFv3
> ospf6 router-id - назначение номера router-id
> сети интерфейсов tun1 и enp0s3 добавляем в конфигурацию OSPFv3
> do write - сохраняем текущую конфигурацию

Перезапускаем frr

    # systemctl restart frr
Посмотреть текущую конфигурацию можно с помощью следующих команд

    #  vtysh
    
    # show running-config

#### Настройка на BR-R
Настройки ```OSPFv2``` и ```OSPFv3``` на BR-R аналогичны HQ-R

Необходимо изменить

- объявляемые сети в OSPFv2;
- router-id в OSPFv3
Настройки OSPFv2 и OSPFv3 на BR-R

![2-5](https://github.com/be2glaz/ssa2004demo/assets/89695370/e3c1ea13-8cac-4769-af4b-4a6e6e3454bd)

Посмотреть текущую конфигурацию можно с помощью следующих команд

    #  vtysh
    
    # show running-config


### Проверка

<details>
<summary>ТЫКНИ</summary>

Получить информацию о соседях и установленных отношениях соседства.

    // для IPv4
    # show ip ospf neighbor

    // для IP6
    # show ipv6 ospf6 neighbor
Показать маршруты, полученные от процесса OSPF.

    // для IPv4
    # show ip route ospf
    
    // для IPv6
    # show ipv6 route ospf6
HQ-R

![2-6](https://github.com/be2glaz/ssa2004demo/assets/89695370/737aa0d8-12be-454a-83e8-a13ab4da85c2)

BR-R

![2-7](https://github.com/be2glaz/ssa2004demo/assets/89695370/6364c9e1-0848-4384-8aaf-3fd781452253)


</details>

#### Топология L3

![l3_topologiya_(2)](https://github.com/be2glaz/ssa2004demo/assets/89695370/48d7c679-dafd-45fe-8bd7-9f8c3b7e2df0)

</details>


## 1.3.Настройте автоматическое распределение IP-адресов на роутере HQ-R

<details>
<summary>ТЫКНИ</summary>


### Задание
#### Настройте автоматическое распределение IP-адресов на роутере HQ-R.
- a. Учтите, что у сервера должен быть зарезервирован адрес.

<details>
<summary>ТЫКНИ</summary>

### Решение
#### Настройка DHCP на HQ-R для IPv4

Установка DHCP

    # dnf install  dhcp-server
> Настройки для диапазона адресов IPv4 производятся в файле /etc/dhcp/dhcpd.conf. Пример данного файла можно посмотреть в файле /usr/share/doc/dhcp-server/dhcpd.conf.example.

Открываем файл конфигурации

    # nano /etc/dhcp/dhcpd.conf
Подсети обозначаются блоками, пример такого блока представлен ниже:

    subnet 172.16.100.0 netmask 255.255.255.192 {
      range 172.16.100.2 172.16.100.62;
      option routers 172.16.100.1;
      default-lease-time 600;
      max-lease-time 7200;
    }
где

- ```subnet``` - обозначает сеть, в области которой будет работать данная группа настроек;
- ```range``` — диапазон, из которого будут браться IP-адреса;
- ```option routers``` — шлюз по умолчанию;
- ```default-lease-time```, ```max-lease-time``` — время и максимальное время в секундах, на которое клиент получит адрес, по его истечению будет выполнено продление срока.

**Резервирование ip-адреса за клиентом**
Хосту с именем ```HQ-SRV``` , у которого сетевая карта имеет MAC ```ff:ff:ff:ff:ff:ff``` зарезервируем адрес ```172.16.100.2```.

    host HQ-SRV {
            hardware ethernet ff:ff:ff:ff:ff:ff;
            fixed-address 172.16.100.2;
    }
```ff:ff:ff:ff:ff:ff``` - mac адрес интерфейса которому будет выдан статический ip-адрес

Выбираем интерфейс, для которого будет работать DHCP сервер

Открываем файл конфигурации

    # nano /etc/sysconfig/dhcpd
Добавляем в него следующее:

    DHCPDARGS=ens19
где

```ens19``` - интерфейс смотрящий в сторону HQ-SRV
Запускаем и добавляем в автозагрузку службу dhcpd (для IPv4):

    # systemctl enable --now dhcpd

#### Проверка на HQ-SRV
Открываем на HQ-SRV настройку сетевых интерфейсов

    # nmtui
Настраиваем интерфейс на автоматическое получение адресов

![2-8](https://github.com/be2glaz/ssa2004demo/assets/89695370/7ccbe25e-0aef-4292-a184-cabddc8c81f0)

Перезагружаем интерфейс и убеждаемся в работоспособности DHCP сервера

![2-9](https://github.com/be2glaz/ssa2004demo/assets/89695370/4d0118db-3ddb-4c93-ae1d-aad6c427fa86)


#### Настройка DHCP на HQ-R для IPv6
> Настройки для диапазона адресов IPv6 производятся в файле ```/etc/dhcp/dhcpd6.conf```. Пример данного файла можно посмотреть в файле ```/usr/share/doc/dhcp-server/dhcpd6.conf.example```.

Для облегчения создания конфигурационного файла для DHCPv6

- Создаем резервную копию файла ```/etc/dhcp/dhcpd6.conf``` переименовав его
- Копируем файл ```/usr/share/doc/dhcp-server/dhcpd6.conf.example``` в директорию ```/etc/dhcp/``` с именем ```dhcpd6.conf```

![2-10](https://github.com/be2glaz/ssa2004demo/assets/89695370/c4c8e5d6-b4d4-47f7-b718-5f3e29b7babf)

Открываем на редактирование файл конфигурации DHCPv6

    # nano /etc/dhcp/dhcpd6.conf
Приводим файл к следующему виду удалив строки

> Вы можете использовать клавиши Ctrl + K, которые вырезают всю строку

![2-11(2)](https://github.com/be2glaz/ssa2004demo/assets/89695370/7c4d8d9d-1640-4be8-a776-43e21752c314)

> Блок host комментируем. Для резервирования IPv6 требуется получить dhcp6.client-id.

> dhcp6.client-id можно получить после запуска и получения клиентом (HQ-SRV) адреса.

Запускаем и добавляем в автозагрузку службу dhcpd6

    # systemctl enable --now dhcpd6
> Перезагружаем сетевой интерфейс на HQ-SRV

Просматриваем журнал и ищем необходимый ```"DUID"``` для того, чтобы зарезервировать IPv6 адрес

![2-12](https://github.com/be2glaz/ssa2004demo/assets/89695370/0c92441d-7f1a-470c-ad3e-6aaae618f441)

Этот ```DUID``` добавляем в host-identifier option при настройке HQ-R как DHCP сервера для IPv6. Снимаем коментарии с блока host.

![2-13](https://github.com/be2glaz/ssa2004demo/assets/89695370/1739f035-93d5-49c3-b7a6-91cb32bb5cd3)

Перезагружаем службу dhcpd6

    # systemctl restart dhcpd6
Отключаем и включаем сетевой интерфейс на HQ-R и HQ-SRV и проверяем:

![2-14](https://github.com/be2glaz/ssa2004demo/assets/89695370/6f7e60fe-8214-43ef-b405-d5d39d1cf472)

#### Установка и настройка RA (Router Advertisement)
> Шлюз на HQ-SRV для IPv4 раздается автоматически , за это отвечает параметр option routers в настройках dhcpd.conf
> 
> Для IPv6 такого параметра нет, шлюзы IPv6 выдаются маршрутизаторами средствами RA (Router Advertisement)

> Установку и настройку RA производим на HQ-R

Установливаем пакет ```radvd```:

    # dnf install -y radvd
Заходим в файл ```/etc/sysctl.conf```

    # nano /etc/sysctl.conf
Добавляем строку

    net.ipv6.conf.enp0s8.accept_ra=2
Открываем файл конфигурации ```radvd```. По умолчанию находится в ```/etc/radvd.conf```:

    nano /etc/radvd.conf
Приводим его к следующему виду:

![2-15(2)](https://github.com/be2glaz/ssa2004demo/assets/89695370/395d8332-e709-4379-8eba-f5f8871e044e)

Параметр ```prefix``` – прописываем свои параметры

Перезапускаем ```dhcpd6.service```

    systemctl restart dhcpd6
Запускаем и добавляем в автозагрузку ```radvd```:

    systemctl enable --now radvd

#### Настройка на HQ-SRV
> Через nmtui изменяем режим КОНФИГУРАЦИЯ IPv6 с <Автоматически (только DHCP)> на <Автоматически>

> Отключаем и включаем сетевой интерфейс на HQ-R и HQ-SRV и проверяем

</details>

#### Проверка


<details>
<summary>ТЫКНИ</summary>
##### IPv4
    
![2-16](https://github.com/be2glaz/ssa2004demo/assets/89695370/b76901e6-b5e2-4e43-b21d-91919912a1c2)

##### IPv6

![2-17](https://github.com/be2glaz/ssa2004demo/assets/89695370/d9dfe595-03a0-494d-ae21-c74e4dfc7c7f)


</details>
</details>


## 1.4.Настроить локальные учётные записи на всех устройствах в соответствии с таблицей 2

<details>
<summary>ТЫКНИ</summary>

### Задание
#### Настройте локальные учётные записи на всех устройствах в соответствии с таблицей 2.

![tab_2](https://github.com/be2glaz/ssa2004demo/assets/89695370/7abeabe5-f454-4c82-897f-dea9b34ec90d)

### Решение
> Для добавления нового пользователя используйте команды useradd и passwd.

> Параметр ```-c``` позволяет добавлять пользовательские комментарии, такие как полное имя пользователя, номер телефона и т. д. в файл /etc/passwd. Комментарий может быть добавлен одной строкой без пробелов.

Команда добавит пользователя «admin» и вставит его полное имя, Administrator, в поле комментария.

    # useradd -c "Admin" admin -U
    # passwd admin
> - ```admin``` - имя пользователя
> - ```-c``` Admin любая текстовая строка. Используется как поле для имени и фамилии пользователя
> - ```-U``` - cоздание группы с тем же именем, что и у пользователя, и добавление пользователь в эту группу
> - ```passwd admin``` - задать пароль пользователю

> Если имя пользователя состоит из двух слов – пишется через ```тире``` или ```подчеркивание```

##### Добавление пользователей
**HQ-R**

    # useradd -c "Admin" admin -U
    # passwd admin
    < вводим пароль пользователя >
    < повторяем ввод паря >
Создание пользователя ```Network admin```

    # useradd -c "Network admin" network_admin -U
    # passwd network_admin
    < вводим пароль пользователя >
    < повторяем ввод паря >
**HQ-SRV**

Создание пользователя ```Admin```

    # useradd -c "Admin" admin -U
    # passwd admin
    < вводим пароль пользователя >
    < повторяем ввод паря >
**BR-R**

Создание пользователя ```Branch admin```

    # useradd -c "Branch admin" branch_admin -U
    # passwd branch_admin
    < вводим пароль пользователя >
    < повторяем ввод паря >
Создание пользователя ```Network admin```

    # useradd -c "Network admin" network_admin -U
    # passwd network_admin
    < вводим пароль пользователя >
    < повторяем ввод паря >
**BR-SRV**

Создание пользователя ```Branch admin```

    # useradd -c "Branch admin" branch_admin -U
    # passwd branch_admin
    < вводим пароль пользователя >
    < повторяем ввод паря >
Создание пользователя ```Network admin```

    # useradd -c "Network admin" network_admin -U
    # passwd network_admin
    < вводим пароль пользователя >
    < повторяем ввод паря >
**CLI**

```Вариант -1```
Создание пользователя ```Admin```

    # useradd -c "Admin" admin -U
    # passwd admin
    < вводим пароль пользователя >
    < повторяем ввод паря >
```Вариант -2```
![useradd_-_gif](https://github.com/be2glaz/ssa2004demo/assets/89695370/fbd4ee89-25e9-41b3-8e0c-971087d5f737)


</details>

## 1.5.Измерить пропускную способность сети между двумя узлами HQ-R - ISP по средствам утилиты iperf 3. Предоставить описание пропускной способности канала со скриншотами

<details>
<summary>ТЫКНИ</summary>

### Задание
#### Измерьте пропускную способность сети между двумя узлами HQ-R-ISP по средствам утилиты iperf 3. Предоставьте описание пропускной способности канала со скриншотами.

### Решение
> Установка утилиты происходит на 2 машинах ```HQ-R``` и ```ISP```: одна выступает в роли сервера, другая в роли клиента.

> Вывод подсказки о том, как использовать iperf3:
>
> iperf3 -h

Устновка:

    # dnf install iperf3 -y
> При тестирование пропускной способности одна машина выступает в роли сервера, другая в роли клиента.

Запуск на стороне сервера с ключом ```-s``` (машина ISP)

    # iperf3 -s
Запуск на стороне клиента с ключом ```-c``` (машина HQ-R)

    # iperf3 -c IP_address_ISP
В ходе выполнения команд выполняется 10 секундная передача данных, на основе которых выдается скорость сети.

![5-1](https://github.com/be2glaz/ssa2004demo/assets/89695370/a63d9662-eb86-463a-8d2e-e352e6242df4)

![5-2](https://github.com/be2glaz/ssa2004demo/assets/89695370/5a0b1212-3c90-49ae-97a4-38ae7c858211)


</details>

## 1.6.Составить backup скрипты для сохранения конфигурации сетевых устройств, HQ-R BR-R. Продемонстрируйте их работу.

<details>
<summary>ТЫКНИ</summary>

### Задание
#### Составить backup скрипты для сохранения конфигурации сетевых устройств, HQ-R BR-R. Продемонстрируйте их работу.

### Решение
#### HQ-R
> Создадим простой ```bash-скрипт``` резервного копирования конфигурационных файлов ```FRR, GRE, Nftables, DHCP``` и ```настроек сетевых интерфейсов```.

Создадим директорию для хранения скрипта резервного копирования ```backup-script``` и директорию для хранения архивов резервных копий ```backup```

    # mkdir /var/{backup,backup-script}
Создадим файл скрипта

    # nano /var/backup-script/backup.sh
Пример скрипта резервного копирования:

![6-1](https://github.com/be2glaz/ssa2004demo/assets/89695370/a24bff6c-4689-4bb1-a014-beb0aeb9989d)


    #!/bin/bash

    data=$(date +%d.%m.%Y-%H:%M:%S)
    mkdir /var/backyup/$data
    cp -r/etc/frr /var/backup/$data
    cp -r/etc/nftables /var/backup/$data
    cp -r/etc/NetworkManager/system-connections /var/backup/$data
    cp -r/etc/dhcp /var/backup/$data
    cd /var/backup
    tar czfv "./$data.tar.gz" ./$data
    rm -r /var/backup/$data

Задаем права скрипту на выполнение:

    # chmod +x /var/backup-script/backup.sh
Запускаем скрипт

    # /var/backup-script/backup.sh

![6-2](https://github.com/be2glaz/ssa2004demo/assets/89695370/eca38a15-9cb0-4100-bb73-0441280a405a)

#### BR-R
> Копируем скрипт с HQ-R на BR-R

> Переходим на ВМ ```BR-R```

Создадим директорию для хранения скрипта резервного копирования ```backup-script``` и директорию для хранения архивов резервных копий ```backup```

    # mkdir /var/{backup,backup-script}
Забираем с HQ-R ```backup.sh```. Используем IP-адресацию GRE туннеля

    scp admin@10.10.10.1:/var/backup-script/backup.sh /var/backup-script/
При необходимости задаем права скрипту на выполнение:

    # chmod +x /var/backup-script/backup.sh
Запускаем скрипт

    # /var/backup-script/backup.sh

</details>  

## 1.7.Настройте подключение по SSH для удалённого конфигурирования устройства HQ-SRV по порту 2222. Учтите, что вам необходимо перенаправить трафик на этот порт по средствам контролирования трафика.

<details>
<summary>ТЫКНИ</summary>

### Задание
#### Настройте подключение по SSH для удалённого конфигурирования устройства HQ-SRV по порту 2222. Учтите, что вам необходимо перенаправить трафик на этот порт по средствам контролирования трафика.


### Решение
#### Настройка подключения
Необходимо изменить порт подключения по ```SSH``` с ```22``` на ```2222```

В конфигурационном файле ```/etc/ssh/sshd_config``` необходимо изменить номер порта

Открываем файл

    # nano /etc/ssh/sshd_config
Находим строчку Port 22 снимаем комментарий со сроки и изменяем номер порта

![7-1](https://github.com/be2glaz/ssa2004demo/assets/89695370/37f0f1af-b10c-45dc-ae07-4ca2a480bf54)

> Если включен SELinux, то необходимо внести изменения в его политики - разрешить этот порт для работы по нему SSH следующей комавндой: ```semanage port -a -t ssh_port_t -p tcp 2222```

Перезапускаем службу ```sshd```

    # systemctl restart sshd
Проверить на каком порту работает SSH:

    # ss -tlpn | grep ssh
Тестируем подключение. C ```HQ-R``` подключаемся к ```HQ-SRV``` нв порту ```2222```

![7-2](https://github.com/be2glaz/ssa2004demo/assets/89695370/61ecb5bf-e305-453c-89dd-0d531d74c1ea)

Перенаправление
> Создаем правило ```nftables``` на ```HQ-R```, которое будет перенаправлять внешние подключения к ```HQ-R``` на порту ```22``` -> на порт ```2222``` сервера ```HQ-SRV```.

Добавим цепочку ```prerouting``` в таблицу ```my_nat``` в ранее созданный файл с правилами nftables ```/etc/nftable/hq-r.nft```

> Примечание
> 
> PREROUTING — предназначена для первичной обработки входящих пакетов, адресованных как непосредственно серверу, так и другим узлам сети. Сюда попадает абсолютно весь входящий трафик для дальнейшего анализа.

Открываем файл

    # nano etc/nftable/hq-r.nft
И дописываем правила (выделено красным)

![7-3](https://github.com/be2glaz/ssa2004demo/assets/89695370/43be53ae-95f9-49d5-bc40-2c4c687f5fde)

Перезапускаем nftables

    # systemctl restart nftables

#### Проверка

<details>
<summary>ТЫКНИ</summary>
    
Подключаемся по SSH с BR-R к HQ-SRV используя внешний IPv4 и IPv6 адрес HQ-R
![7-4](https://github.com/be2glaz/ssa2004demo/assets/89695370/459a3943-54f4-4e80-8aaa-21d33298eb3d)

![7-5](https://github.com/be2glaz/ssa2004demo/assets/89695370/19f56a37-8541-424b-9714-a6a6cc52e8c6)

</details>  
</details>  

## 1.8.Настройте контроль доступа до HQ-SRV по SSH со всех устройств, кроме CLI.

<details>
<summary>ТЫКНИ</summary>

### Задание
#### Настройте контроль доступа до HQ-SRV по SSH со всех устройств, кроме CLI.

### Решение
#### Настройка nftables на HQ-SRV
Установка nftables

    # dnf install -y nftables
Создаем и открываем файл

    # nano /etc/nftables/hq-srv.nft
> Запрещаем подключение CLI к HQ-SRV по IPv4 и IPv6

Прописываем следующие строки

    table inet filter {
                chain input {
                type filter hook input priority filter; policy accept;
                ip saddr 3.3.3.2 tcp dport 2222 counter reject
                ip saddr 4.4.4.0/30 tcp dport 2222 counter reject
                ip6 saddr 2024:ab:cd:3::/64 tcp dport 2222 counter reject
                ip6 saddr 2024:ab:cd:4::/64 tcp dport 2222 counter reject
                }
    }
Включаем использование данного файла в ```sysconfig```

    # nano /etc/sysconfig/nftables.conf
Ниже строки начинающейся на ```include```, прописываем строку

    include "/etc/nftables/hq-srv.nft"
Запуск и добавление в автозагрузку сервиса nftables

    # systemctl enable --now nftables

### Проверка подключение к HQ-SRV по SSH

<details>
<summary>ТЫКНИ</summary>

#### Подключение с HQ-R

![8-1](https://github.com/be2glaz/ssa2004demo/assets/89695370/1eb28601-9ac7-441a-849a-9e7de9460772)

#### Подключение с BR-R

![8-2](https://github.com/be2glaz/ssa2004demo/assets/89695370/661a13e0-84c6-4aff-b7a0-93a029e3d6b7)

#### Подключение с BR-SRV

![8-3](https://github.com/be2glaz/ssa2004demo/assets/89695370/cd25bbae-06fb-43cd-b602-c9340497f4c9)

#### Подключение с CLI

![8-4](https://github.com/be2glaz/ssa2004demo/assets/89695370/53bff2a7-7d11-4f20-9373-5d77f60e8033)


</details>
</details>
