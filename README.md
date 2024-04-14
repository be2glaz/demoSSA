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

# КАРТИНКИИИ

<details>
<summary>ТЫКНИ</summary>

![topology](https://github.com/be2glaz/ssa2004demo/assets/89695370/54472aa7-2573-4f55-b219-bf314e30f1ec)

![tab_1](https://github.com/be2glaz/ssa2004demo/assets/89695370/a48d854b-7284-4f67-8318-ce1c1a6ea22d)

![ip_adr_tabl(2)](https://github.com/be2glaz/ssa2004demo/assets/89695370/91c000bb-9a96-4017-8ee9-86e59870074c)

![l3_topologiya_(2)](https://github.com/be2glaz/ssa2004demo/assets/89695370/3a2e1161-db7c-4627-8191-98a602cd43ef)

</details>

# Топология

    
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
> [!TIP]
>    NAME - имя устройства
> 
>    exec bash — перезапуск оболочки bash для отображения нового хостнейма
> 
>    Для устройств BR-SRV и CLI желательно сразу установить полное доменное имя. Потребуется для ввода этих машин в домен во второй части задания.
>    Например:
> 
>    ISP: isp
> 
>    CLI: cli.hq.work
> 
>    HQ-R: hq-r.hq.work
> 
>    HQ-SRV: hq-srv.hq.work
> 
>    BR-R: br-r.branch.work
> 
>    BR-SRV: br-srv.branch.wor

Пример:

![1-1](https://github.com/be2glaz/ssa2004demo/assets/89695370/cb447ca2-2e79-496b-8643-97fe1d349fe8)


b. Рассчитать IP-адресацию IPv4 и IPv6. Необходимо заполнить таблицу №1, чтобы эксперты могли проверить ваше рабочее место.
см Таблица IP-адресов

c. Пул адресов для сети офиса BRANCH - не более 16
Для пула адресов IPv4 не более 16 - маска подсети /28

Для пула адресов IPv6 не более 16 - длина префикса /124

d. Пул адресов для сети офиса HQ - не более 64
Для пула адресов IPv4 не более 64 - маска подсети /26

Для пула адресов IPv6 не более 64 - длина префикса /122


Настройка сетевых интерфейсов
ISP
Определяемся мена интерфейсов и какой интерфейс в какую сторону смотрит

Выводим информацию о сетевых интерфейсах:

# ip -c a

![1-2](https://github.com/be2glaz/ssa2004demo/assets/89695370/cf26d254-96d5-495e-9c99-ebf201d9a5c4)

Открываем настройки виртуальной машины

Выбираем необходимую виртуальную машину
Выбираем Оборудование
Смотрим MAC-адрес сетевых интерфейсов, и запоминаем их (лучше записать на черновик)

![1-3](https://github.com/be2glaz/ssa2004demo/assets/89695370/a52f6ddf-f930-466e-87cd-d0426988931a)













