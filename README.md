# lesson19

**Домашнее задание**

Настройка PXE сервера для автоматической установки

Цель:</br>
отрабатываем навыки установки и настройки DHCP, TFTP, PXE загрузчика и автоматической загрузки;

Описание/Пошаговая инструкция выполнения домашнего задания:</br>
Для выполнения домашнего задания используйте методичку

Что нужно сделать?

Следуя шагам из документа https://docs.centos.org/en-US/8-docs/advanced-install/assembly_preparing-for-a-network-install </br>
установить и настроить загрузку по сети для дистрибутива CentOS8.</br>
В качестве шаблона воспользуйтесь репозиторием https://github.com/nixuser/virtlab/tree/main/centos_pxe.</br>
Поменять установку из репозитория NFS на установку из репозитория HTTP.</br>

Для выполнения задания был использован box файл Centos7 2004.01</br>
В качестве OS для удаленной установки выбрана Centos 8 Stream, iso  образ которого весит >12Gb</br>

Vagranfile равернул две машины, как в методичке, предварительно скопировав в рабочую папку ./vagrant iso образ

Установил tftp-server, dhcp, syslinux, xinetd</br>

---
    sudo -i
    yum install tftp-server dhcp syslinux xinetd
---

Сделал настройку httpd

![image](https://github.com/movik242/lesson19/assets/143793993/80a4daf3-deab-483f-b2a8-5c67f3c07de6)

![image](https://github.com/movik242/lesson19/assets/143793993/16adc309-4231-43ee-ad8c-4f543df7c13f)

![image](https://github.com/movik242/lesson19/assets/143793993/e3fe2613-e01f-4422-a2a9-88c7e5b6baca)

Настроил файлы конфигурации

    cp -a /usr/share/syslinux/* /var/lib/tftpboot/
    mkdir /var/lib/tftpboot/centos8
    cp /mnt/images/pxeboot/vmlinuz  /var/lib/tftpboot/centos8
    cp /mnt/images/pxeboot/initrd.img  /var/lib/tftpboot/centos8

    mkdir /var/lib/tftpboot/pxelinux.cfg
    vi /var/lib/tftpboot/pxelinux.cfg/default

![image](https://github.com/movik242/lesson19/assets/143793993/344296c0-2ae5-4fcd-a962-abfe51137041)

    vi /etc/xinetd.d/tftp

![image](https://github.com/movik242/lesson19/assets/143793993/37d511c8-d9f4-4a3a-a3b5-997a03cdefe7)

    vi /etc/dhcp/dhcpd.conf

![image](https://github.com/movik242/lesson19/assets/143793993/fc03e7d9-8317-4db7-838b-f80754d7eefb)

    systemctl restart xinetd
    systemctl restart httpd
    systemctl restart dhcpd

Запускаю машину pxeclient

![image](https://github.com/movik242/lesson19/assets/143793993/9f63f5dd-a441-4aa8-8b87-386373090e23)

![image](https://github.com/movik242/lesson19/assets/143793993/a7eb358c-69a3-4693-8a5f-0f93b3f5eab4)


