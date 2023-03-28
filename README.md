# Домашнее задание к занятию "`8.2.  Что такое DevOps. СI/СD`" - `Корольков Денис`


### Инструкция по выполнению домашнего задания

   1. Сделайте `fork` [репозитория c шаблоном решения](https://github.com/netology-code/sys-pattern-homework) к себе в GitHub и переименуйте его по названию или номеру занятия, например, https://github.com/имя-вашего-репозитория/gitlab-hw или https://github.com/имя-вашего-репозитория/8-03-hw.
   2. Выполните клонирование этого репозитория к себе на ПК с помощью команды `git clone`.
   3. Выполните домашнее задание и заполните у себя локально этот файл README.md:
      - впишите сверху название занятия, ваши фамилию и имя;
      - в каждом задании добавьте решение в требуемом виде — текст, код, скриншоты, ссылка.
      - для корректного добавления скриншотов используйте [инструкцию «Как вставить скриншот в шаблон с решением»](https://github.com/netology-code/sys-pattern-homework/blob/main/screen-instruction.md);
      - при оформлении используйте возможности языка разметки md. Коротко об этом можно посмотреть в [инструкции  по MarkDown](https://github.com/netology-code/sys-pattern-homework/blob/main/md-instruction.md).
   4. После завершения работы над домашним заданием сделайте коммит `git commit -m "comment"` и отправьте его на GitHub `git push origin`.
   5. Для проверки домашнего задания в личном кабинете прикрепите и отправьте ссылку на решение в виде md-файла в вашем GitHub.
   6. Любые вопросы по выполнению заданий задавайте в чате учебной группы или в разделе «Вопросы по заданию» в личном кабинете.
   
Желаем успехов в выполнении домашнего задания!

---

### Задание 1

**Что нужно сделать:**

1. Установите себе jenkins по инструкции из лекции или любым другим способом из официальной документации. Использовать Docker в этом задании нежелательно.
2. Установите на машину с jenkins [golang](https://golang.org/doc/install).
3. Используя свой аккаунт на GitHub, сделайте себе форк [репозитория](https://github.com/netology-code/sdvps-materials.git). В этом же репозитории находится [дополнительный материал для выполнения ДЗ](https://github.com/netology-code/sdvps-materials/blob/main/CICD/8.2-hw.md).
3. Создайте в jenkins Freestyle Project, подключите получившийся репозиторий к нему и произведите запуск тестов и сборку проекта ```go test .``` и  ```docker build .```.

В качестве ответа пришлите скриншоты с настройками проекта и результатами выполнения сборки.

---

Ответ:

Последовательность выполнения:

Пеерд установкой jenkins, установлю пакет java - а именно jdk (он включает в себя пакет jre, но более полный и необходим для компиляции)

sudo apt install default-jdk

![screen1](https://github.com/KorolkovDenis/)

jdb -version

![screen2](https://github.com/KorolkovDenis/)

Подготавливаем основу для Jenkins:

Добавляем репозиторий в файл jenkins.list

sudo sh -c 'echo deb https://pkg.jenkins.io/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list' 

Импортирую публичный ключ для подключения к репозиторию:
wget -q -O - https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo apt-key add -

Обновим список пактов – sudo apt update

Устанавливаем Jenkins: sudo apt install jenkins

![screen3](https://github.com/KorolkovDenis/)

Разрешаем автозапуск сервиса: systemctl enable jenkins

Запускаем демона: systemctl start jenkins

Возьмем пароль для входа в файле: /var/lib/jenkins/secrets/initialAdminPassword

![screen4](https://github.com/KorolkovDenis/)

Устанавливаем go:
1.	Скачиваем архив: go1.20.2.linux-amd64.tar.gz
wget https://go.dev/dl/go1.20.2.linux-amd64.tar.gz

![screen5](https://github.com/KorolkovDenis/)

2.	Если есть, удаляем все предыдущие установки Go, удалив папку /usr/local/go (если она существует), затем извлеките архив, который вы только что загрузили, в /usr/local, создав новое дерево Go в /usr/local/go:: 

sudo rm -rf /usr/local/go && sudo tar -C /usr/local -xzf go1.20.2.linux-amd64.tar.gz

![screen6](https://github.com/KorolkovDenis/)

![screen7](https://github.com/KorolkovDenis/)

Добавим /usr/local/go/bin в переменную окружения PATH, добавив следующую строку в свой $HOME/.profile или /etc/profile (для общесистемной установки):

export PATH=$PATH:/usr/local/go/bin

go version 	- проверим какая у нас установлена версия go

![screen8](https://github.com/KorolkovDenis/)

Тут бы нам еще доставить docker:

Доставлю пакет с docker: Install Docker Engine on Ubuntu

sudo apt-get update

sudo apt-get install \
    ca-certificates \
    curl \
    gnupg \
    lsb-release

sudo mkdir -m 0755 -p /etc/apt/keyrings

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg

echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt-get update

sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin

docker-compose-plugin

Добавлю группу docker и нужных пользователей:

sudo groupadd docker

sudo usermod -aG docker $USER   - перезапуск хоста для применения изменений

sudo usermod –a –G docker jenkins

sudo usermod –a –G docker vagrant   - если использовали Vagrant

sudo systemctl restart docker.service

Fork репозитория на github:

https://github.com/KorolkovDenis/sdvps-materials.git

![screen9](https://github.com/KorolkovDenis/)

Ломимся на вебку Jenkins: на момент первого входа IP: 192.168.43.111:8080

Берем пароль с файла /var/lib/jenkins/secrets/initialAdminPassword

![screen10](https://github.com/KorolkovDenis/)
![screen11](https://github.com/KorolkovDenis/)
![screen12](https://github.com/KorolkovDenis/)
![screen13](https://github.com/KorolkovDenis/)
![screen14](https://github.com/KorolkovDenis/)
![screen15](https://github.com/KorolkovDenis/)

Создаю в jenkins Freestyle Project, подключаю получившийся репозиторий к нему и произвожу запуск тестов и сборку проекта go test . и docker build ..

![screen16](https://github.com/KorolkovDenis/)
![screen17](https://github.com/KorolkovDenis/)

Шаги сборки - Выполнить команду shell (Execute shell)

/usr/local/go/bin/go test . – выполнить test всех файлов в go формате

Далее мы говорим, что надо поработать с docker, который уже был поставлен на наш хост с Jenkins.

docker build . -t ubuntu-bionic:8082/hello-world:v$BUILD_NUMBER

![screen18](https://github.com/KorolkovDenis/)

Сохраняем конфиг и запускаем: Собрать сейчас

![screen19](https://github.com/KorolkovDenis/)
![screen20](https://github.com/KorolkovDenis/)
![screen21](https://github.com/KorolkovDenis/)
![screen22](https://github.com/KorolkovDenis/)


### Задание 2

**Что нужно сделать:**

1. Создайте новый проект pipeline.
2. Перепишите сборку из задания 1 на declarative в виде кода.

В качестве ответа пришлите скриншоты с настройками проекта и результатами выполнения сборки.

---

Ответ:

Последовательность выполнения:

![screen3](https://github.com/KorolkovDenis/)

![screen1](https://github.com/KorolkovDenis/)




### Задание 3

**Что нужно сделать:**

1. Установите на машину Nexus.
1. Создайте raw-hosted репозиторий.
1. Измените pipeline так, чтобы вместо Docker-образа собирался бинарный go-файл. Команду можно скопировать из Dockerfile.
1. Загрузите файл в репозиторий с помощью jenkins.

В качестве ответа пришлите скриншоты с настройками проекта и результатами выполнения сборки.

---

Ответ:

Последовательность выполнения:



## Дополнительные задания* (со звёздочкой)

Их выполнение необязательное и не влияет на получение зачёта по домашнему заданию. Можете их решить, если хотите лучше разобраться в материале.

---

### Задание 4*

Придумайте способ версионировать приложение, чтобы каждый следующий запуск сборки присваивал имени файла новую версию. Таким образом, в репозитории Nexus будет храниться история релизов.

Подсказка: используйте переменную BUILD_NUMBER.

В качестве ответа пришлите скриншоты с настройками проекта и результатами выполнения сборки.




## Ссылка на мою работу в Google:

[Моя работа по Jenkins](https://docs.google.com/document/)
