# Проект внедрения девопс практик и инструментов
Общая цель - автоматизация.

## Общее описание сервисов и серверов
- Server1 - компьютер разработчика с Ubuntu Desktop 18.04 x64
с него будет все устанавливаться и запускаться всего для демонстрации ДевОпс инструментов и практик
На ней так же будут запускаться проекты и виртуальные машины. 
- Server2 - виртуальная машина на компьютере разработчика. 
На ней будет висеть GitLab Community Edition, система непрерывной интеграции
- Server3 - реальный сервер в локальной сети компании, на нем располагается GIT репозиторий кода продукта
Этот сервер мы напрямую пока не будем использовать.
- Server4 - реальный сервер в локальной сети компании, на нем расположен NGINX/PHP сервер с готовыми артефактами. 
Артефакт представляет собой архив формата *.tar.gz, из которого разворачивается продукт.
Мы можем подключаться по протоколу HTTP к адресу сервера и через WGET утилиту скачивать артефакты.
Артефакты с Server3 попадают на сервер Server4 посредством еженочных билдов(мы их не рассматриваем).
С этого сервера мы будем качать артефакты.

- Git репозиторий СОЗДАНИЯ инфраструктуры лежат тут: https://github.com/Veter9/office_devops
- Git репозиторий CI системы будут лежать тут: http://gitlab.example.com/root/nms-3.4.git (заменить на реальный домен)
- Вся сеть работает в подсети 192.168.1.0/24 с гейтвеем 192.168.1.1
- Все адреса, логины, пароли ниже представлены как пример.

# Установка сервисов
## 1. Установка рабочей машины разработчика Server1 192.168.1.41
- устанавливаем Ubuntu Desktop 18.04 x64
Установочный диск берём отсюда 
http://releases.ubuntu.com/18.04/ubuntu-18.04.1-desktop-amd64.iso
Имя пользователя, пароль - по желанию. Я использовал user/1.
- После установки обновляем систему
- `apt update`
- `apt upgrade`
- устанавливаем IDE для удобства, в моём примере это PyCharm, но может быть любой.
Подключаем к проекту репозиторий http://gitlab.example.com/root/nms-3.4.git
Ветка master всего одна, pull сразу в мастер. 
В дальнейшем IDE используется для правки кода всего проекта

## 2. Установка VirtualBox на компьютер разработчика Server1 для запуска GitLab сервиса Server2
- `wget https://download.virtualbox.org/virtualbox/5.2.18/virtualbox-5.2_5.2.18-124319~Ubuntu~bionic_amd64.deb`
- `dpkg -i virtualbox-5.2_5.2.18-124319~Ubuntu~bionic_amd64.deb`

## 3. Установка GitLab сервиса на Server2
- создаем новую виртуальную машину средствами графического интерфейса VirtualBox 
4096 RAM, 4 cores, 30GB HDD DVI, network=bridge 
- устанавливаем Ubuntu 18.04 server x64
Образ брать отсюда http://releases.ubuntu.com/18.04/ubuntu-18.04.1-live-server-amd64.iso
Установка по-умолчанию, пользователь user, пароль 1, имя сервера gitlab
Раздел не шифровать.
- после установки сервера необходимо сделать обновление сервера 
- `apt update`
- `apt upgrade`
- `apt install ssh`
- `sudo apt-get install -y curl openssh-server ca-certificates`
- `curl -sS https://packages.gitlab.com/install/repositories/gitlab/gitlab-ce/script.deb.sh | sudo bash`
- `sudo EXTERNAL_URL="http://gitlab.example.com" apt-get install gitlab-ce` 
где gitlab.example.com - домен внутри локальной сети компании
После установки необходимо зайти на веб-страницу GitLab 192.168.1.214 
При первом старте необходимо ввести пароль рута (Ww1228Tt).
После этого можно залогиниться по http://192.168.1.214 и ввести логин root вкупе с созданным паролем Ww1228Tt
- Создаем проект с именем "NMS_3.4". Access private.

## 4. Устаналиваем Vagrant на машине разработчика Server1
- `sudo apt install vagrant`

# Настройка сервисов 
## 1. Настройка GitLab сервиса
- Подключаем IDE разработчика к репозиторию CI системы http://gitlab.example.com/root/nms-3.4.git
- Создаем первый файл с имнем .gitlab-ci.yml
В этом файле будет раположен наш пайплайн.
- Добавляем файл в git реестр и делаем commit+push
- Конечно же у нас грохнулся пайплайн. Нам необходим раннер. Settings - CI/CD - Runners
- Устанавливаем раннер. Я выбрал SSH раннер. Делаем по инструкции
- https://docs.gitlab.com/runner/install/linux-repository.html 
- подключаемся к сервису по ssh 
- `ssh user@192.168.1.214` , соглашаемся с ключом и вводим пароль "1"
- переходим в консоль админа `sudo su`
- `curl -L https://packages.gitlab.com/install/repositories/runner/gitlab-runner/script.deb.sh | sudo bash`
- `sudo apt-get install gitlab-runner`
- `apt-cache madison gitlab-runner`
- `sudo apt-get install gitlab-runner=10.0.0`
- Далее необходимо зарегистрировать раннер
- https://docs.gitlab.com/runner/register/index.html
- `sudo gitlab-runner register`
- После установки его видно в списках раннеров
## Подключение GitLab сервиса к машине разработчика
- Генерируем ключи SSH для подключения на GitLab сервере
- `ssh-keygen`
- Далее копируем созданный клюяч на машину разработчика
- `ssh-copy-id user@192.168.1.41`
- После успешного добавления можно проверить введя команду
`ssh user@192.168.1.41`
- Должно запустить без пароля
##
