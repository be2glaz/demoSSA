# ssa2004demo

### Общая информация

<details>
<summary>ТЫКНИ</summary>

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

![topology](https://github.com/be2glaz/ssa2004demo/assets/89695370/54472aa7-2573-4f55-b219-bf314e30f1ec)

![tab_1](https://github.com/be2glaz/ssa2004demo/assets/89695370/a48d854b-7284-4f67-8318-ce1c1a6ea22d)

![ip_adr_tabl(2)](https://github.com/be2glaz/ssa2004demo/assets/89695370/91c000bb-9a96-4017-8ee9-86e59870074c)

![l3_topologiya_(2)](https://github.com/be2glaz/ssa2004demo/assets/89695370/3a2e1161-db7c-4627-8191-98a602cd43ef)

</details>

## Базовая настройка

<details>
<summary>ТЫКНИ</summary>
    
![topology](https://github.com/be2glaz/ssa2004demo/assets/89695370/54472aa7-2573-4f55-b219-bf314e30f1ec)

1. Выполните базовую настройку всех устройств:
a. Присвоить имена в соответствии с топологией
b. Рассчитать IP-адресацию IPv4 и IPv6. Необходимо заполнить таблицу №1, чтобы эксперты могли проверить ваше рабочее место.
c. Пул адресов для сети офиса BRANCH - не более 16
d. Пул адресов для сети офиса HQ - не более 64

![tab_1](https://github.com/be2glaz/ssa2004demo/assets/89695370/a48d854b-7284-4f67-8318-ce1c1a6ea22d)


![ip_adr_tabl(2)](https://github.com/be2glaz/ssa2004demo/assets/89695370/91c000bb-9a96-4017-8ee9-86e59870074c)


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

![1-1](https://github.com/be2glaz/ssa2004demo/assets/89695370/cb447ca2-2e79-496b-8643-97fe1d349fe8)


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





