# Инфрастуктура, общее описание

- Server1 - компьютер разработчика с Ubuntu Descktop 18.04 x64
с него будет все устанавливаться и запускаться всего для демонстрации ДевОпс инструментов и практик
- Server2 - виртуальная машина на компьютере разработчика. 
На ней будет висеть GitLab Community Edition, система непрерывной интеграции
- Server3 - виртуальная машина на компьютере разработчика
На ней будут запускаться проекты и виртуальные машины. Да, виртуальные машины в виртуальной машине.
- Server4 - реальный сервер в локальной сети компании, на нем располагается GIT репозиторий кода продукта
- Server5 - реальный сервер в локальной сети компании, на нем расположен NGINX сервер с готовыми артефактами 
Артефакт представляет собой архив формата *.tar.gz, из которого разворачивается продукт.

Артефакты с Server4 попадают на сервер Server5 посредством еженочных билдов(мы их не рассматриваем)

Все git репозитории инфраструктуры лежат тут https://github.com/Veter9/office_devops

## Установка рабочей машины разработчика Server1
- устанавливаем Ubuntu Desktop 18.04 x64
Установочный диск берём отсюда 
http://releases.ubuntu.com/18.04/ubuntu-18.04.1-desktop-amd64.iso
Имя пользователя, пароль - по желанию.
- После установки обновлем систему
`apt update`
`apt upgrade`

## установка VirtualBox на компьютер разработчика для запуска GitLab сервиса
`wget https://download.virtualbox.org/virtualbox/5.2.18/virtualbox-5.2_5.2.18-124319~Ubuntu~bionic_amd64.deb`
`dpkg -i virtualbox-5.2_5.2.18-124319~Ubuntu~bionic_amd64.deb`

## Установка GitLab сервиса на Server2
- создаем новую виртуальную машину средствами графического интерфейса VirtualBox 
4096 RAM, 4 cores, 30GB HDD DVI, network=bridge 
- устанавливаем Ubuntu 18.04 server x64
Образ брать отсюда http://releases.ubuntu.com/18.04/ubuntu-18.04.1-live-server-amd64.iso
Установка по-умолчанию, пользователь user, пароль 1, имя сервера gitlab
Раздел не шифровать.
- после установки сервера необходимо сделать обновление сервера 
`apt update`
`apt upgrade`
`apt install ssh`
`sudo apt-get install -y curl openssh-server ca-certificates`
`curl -sS https://packages.gitlab.com/install/repositories/gitlab/gitlab-ce/script.deb.sh | sudo bash`
`sudo EXTERNAL_URL="http://gitlab.example.com" apt-get install gitlab-ce` 
где gitlab.example.com - домен внутри локальной сети компании
После установки необходимо зайти на 




