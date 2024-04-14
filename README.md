# ssa2004demo


калькулятор ipv4
https://ipmeter.ru/
калькулятор ipv6
[https://ipmeter.ru/](https://www.coderstool.com/ipv6-subnet-calculator)

drawio
https://app.diagrams.net/



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

Например:
ISP: isp
CLI: cli.hq.work
HQ-R: hq-r.hq.work
HQ-SRV: hq-srv.hq.work
BR-R: br-r.branch.work
BR-SRV: br-srv.branch.wor

Пример:
![1-1](https://github.com/be2glaz/ssa2004demo/assets/89695370/cb447ca2-2e79-496b-8643-97fe1d349fe8)








