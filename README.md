#Инфрастуктура

- Server1 - компьютер разработчика с Ubuntu Descktop 18.04 x64
с него будет все устанавливаться и запускаться
- Server2 - виртуальная машина на компьютере разработчика. 
На ней будет висеть GitLab Community Edition
- Server3 - виртуальная машина на компьютере разработчика
На ней будут запускаться проекты

Все git репозитории лежат тут https://github.com/Veter9/office_devops


## Установка рабочей машины разработчика
- устанавливаем Ubuntu Desktop 18.04 x64
Установочный диск берём отсюда http://releases.ubuntu.com/18.04/ubuntu-18.04.1-desktop-amd64.iso
Имя пользователя, пароль - по желанию.
- После установки обновлем систему
apt update
apt upgrade

## установка VirtualBox на компьютер разработчика
wget https://download.virtualbox.org/virtualbox/5.2.18/virtualbox-5.2_5.2.18-124319~Ubuntu~bionic_amd64.deb
dpkg -i virtualbox-5.2_5.2.18-124319~Ubuntu~bionic_amd64.deb

## Установка GitLab сервера
- создаем новую виртуальную машину средствами графического интерфейса VirtualBox 
4096 RAM, 4 cores, 30GB HDD, network=bridge 
- устанавливаем Ubuntu 18.04 server x64
Я установил на виртуальную машину VirtualBox (5.2.10_Ubuntu r121806) на своем компьютере.
Образ взял отсюда http://releases.ubuntu.com/18.04/ubuntu-18.04.1-live-server-amd64.iso
Установка по-умолчанию, пользователь user, пароль 1, имя сервера gitlab
- после установки сервера необходимо сделать обновление сервера 
apt update
apt upgrade


