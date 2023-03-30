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

Перед установкой jenkins, установлю пакет java - а именно jdk (он включает в себя пакет jre, но более полный и необходим для компиляции)

sudo apt install default-jdk

![screen1](https://github.com/KorolkovDenis/8.2-jenkins-korolkov/blob/main/Screenshots/work1/screen1.png)

jdb -version

![screen2](https://github.com/KorolkovDenis/8.2-jenkins-korolkov/blob/main/Screenshots/work1/screen2.png)

Подготавливаем основу для Jenkins:

Добавляем репозиторий в файл jenkins.list

sudo sh -c 'echo deb https://pkg.jenkins.io/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list' 

Импортирую публичный ключ для подключения к репозиторию:
wget -q -O - https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo apt-key add -

Обновим список пактов – sudo apt update

Устанавливаем Jenkins: sudo apt install jenkins

![screen3](https://github.com/KorolkovDenis/8.2-jenkins-korolkov/blob/main/Screenshots/work1/screen3.png)

Разрешаем автозапуск сервиса: systemctl enable jenkins

Запускаем демона: systemctl start jenkins

Возьмем пароль для входа в файле: /var/lib/jenkins/secrets/initialAdminPassword

![screen4](https://github.com/KorolkovDenis/8.2-jenkins-korolkov/blob/main/Screenshots/work1/screen4.png)

Устанавливаем go:
1.	Скачиваем архив: go1.20.2.linux-amd64.tar.gz
wget https://go.dev/dl/go1.20.2.linux-amd64.tar.gz

![screen5](https://github.com/KorolkovDenis/8.2-jenkins-korolkov/blob/main/Screenshots/work1/screen5.png)

2.	Если есть, удаляем все предыдущие установки Go, удалив папку /usr/local/go (если она существует), затем извлеките архив, который вы только что загрузили, в /usr/local, создав новое дерево Go в /usr/local/go:: 

sudo rm -rf /usr/local/go && sudo tar -C /usr/local -xzf go1.20.2.linux-amd64.tar.gz

![screen6](https://github.com/KorolkovDenis/8.2-jenkins-korolkov/blob/main/Screenshots/work1/screen6.png)

![screen7](https://github.com/KorolkovDenis/8.2-jenkins-korolkov/blob/main/Screenshots/work1/screen7.png)

Добавим /usr/local/go/bin в переменную окружения PATH, добавив следующую строку в свой $HOME/.profile или /etc/profile (для общесистемной установки):

export PATH=$PATH:/usr/local/go/bin

go version 	- проверим какая у нас установлена версия go

![screen8](https://github.com/KorolkovDenis/8.2-jenkins-korolkov/blob/main/Screenshots/work1/screen8.png)

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

![screen9](https://github.com/KorolkovDenis/8.2-jenkins-korolkov/blob/main/Screenshots/work1/screen9.png)

Ломимся на вебку Jenkins: на момент первого входа IP: 192.168.43.111:8080

Берем пароль с файла /var/lib/jenkins/secrets/initialAdminPassword

![screen10](https://github.com/KorolkovDenis/8.2-jenkins-korolkov/blob/main/Screenshots/work1/screen10.png)
![screen11](https://github.com/KorolkovDenis/8.2-jenkins-korolkov/blob/main/Screenshots/work1/screen11.png)
![screen12](https://github.com/KorolkovDenis/8.2-jenkins-korolkov/blob/main/Screenshots/work1/screen12.png)
![screen13](https://github.com/KorolkovDenis/8.2-jenkins-korolkov/blob/main/Screenshots/work1/screen13.png)
![screen14](https://github.com/KorolkovDenis/8.2-jenkins-korolkov/blob/main/Screenshots/work1/screen14.png)
![screen15](https://github.com/KorolkovDenis/8.2-jenkins-korolkov/blob/main/Screenshots/work1/screen15.png)

Создаю в jenkins Freestyle Project, подключаю получившийся репозиторий к нему и произвожу запуск тестов и сборку проекта go test . и docker build ..

![screen16](https://github.com/KorolkovDenis/8.2-jenkins-korolkov/blob/main/Screenshots/work1/screen16.png)
![screen17](https://github.com/KorolkovDenis/8.2-jenkins-korolkov/blob/main/Screenshots/work1/screen17.png)

Шаги сборки - Выполнить команду shell (Execute shell)

/usr/local/go/bin/go test . – выполнить test всех файлов в go формате

Далее мы говорим, что надо поработать с docker, который уже был поставлен на наш хост с Jenkins.

docker build . -t ubuntu-bionic:8082/hello-world:v$BUILD_NUMBER

![screen18](https://github.com/KorolkovDenis/8.2-jenkins-korolkov/blob/main/Screenshots/work1/screen18.png)

Сохраняем конфиг и запускаем: Собрать сейчас

![screen19](https://github.com/KorolkovDenis/8.2-jenkins-korolkov/blob/main/Screenshots/work1/screen19.png)
![screen20](https://github.com/KorolkovDenis/8.2-jenkins-korolkov/blob/main/Screenshots/work1/screen20.png)
![screen21](https://github.com/KorolkovDenis/8.2-jenkins-korolkov/blob/main/Screenshots/work1/screen21.png)
![screen22](https://github.com/KorolkovDenis/8.2-jenkins-korolkov/blob/main/Screenshots/work1/screen22.png)


### Задание 2

**Что нужно сделать:**

1. Создайте новый проект pipeline.
2. Перепишите сборку из задания 1 на declarative в виде кода.

В качестве ответа пришлите скриншоты с настройками проекта и результатами выполнения сборки.

---

Ответ:

Последовательность выполнения:

![screen1](https://github.com/KorolkovDenis/8.2-jenkins-korolkov/blob/main/Screenshots/work2/screen1.png)
![screen2](https://github.com/KorolkovDenis/8.2-jenkins-korolkov/blob/main/Screenshots/work2/screen2.png)
![screen3](https://github.com/KorolkovDenis/8.2-jenkins-korolkov/blob/main/Screenshots/work2/screen3.png)
![screen4](https://github.com/KorolkovDenis/8.2-jenkins-korolkov/blob/main/Screenshots/work2/screen4.png)
![screen5](https://github.com/KorolkovDenis/8.2-jenkins-korolkov/blob/main/Screenshots/work2/screen5.png)
![screen6](https://github.com/KorolkovDenis/8.2-jenkins-korolkov/blob/main/Screenshots/work2/screen6.png)
![screen7](https://github.com/KorolkovDenis/8.2-jenkins-korolkov/blob/main/Screenshots/work2/screen7.png)
![screen8](https://github.com/KorolkovDenis/8.2-jenkins-korolkov/blob/main/Screenshots/work2/screen8.png)


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

Запускаем Nexus на нашей хостовой машины, с помощью docker:

docker run -d -p 192.168.43.234:8081:8081 -p 192.168.43.234:8082:8082 --name nexus -e INSTALL4J_ADD_VM_PARAMS="-Xms512m -Xmx512m -XX:MaxDirectMemorySize=273m" sonatype/nexus3

192.168.43.234 - IP моего хоста
192.168.43.234:8081 - по этому порту будем на nexus логиниться

Вывести пароль администратора для первого логина в Nexus:

docker exec -t nexus bash -c 'cat /nexus-data/admin.password && echo'

224a7868-30e8-44da-8904-1f59e6f96ce7

Теперь нам нужно создать репозиторий хранения go - файлов, для чего переходим в режим конфигурации и жмем create repository:

![screen1](https://github.com/KorolkovDenis/8.2-jenkins-korolkov/blob/main/Screenshots/work3/screen1.png)

Выбираем raw (hosted)

![screen2](https://github.com/KorolkovDenis/8.2-jenkins-korolkov/blob/main/Screenshots/work3/screen2.png)
![screen3](https://github.com/KorolkovDenis/8.2-jenkins-korolkov/blob/main/Screenshots/work3/screen3.png)

Далее в jenkins создаем новый pipeline: nexus_repo_go. Pipeline script будет такой:

![screen4](https://github.com/KorolkovDenis/8.2-jenkins-korolkov/blob/main/Screenshots/work3/screen4.png)

Запускаем сборку и получаем результат:

![screen5](https://github.com/KorolkovDenis/8.2-jenkins-korolkov/blob/main/Screenshots/work3/screen5.png)

## Дополнительные задания* (со звёздочкой)

Их выполнение необязательное и не влияет на получение зачёта по домашнему заданию. Можете их решить, если хотите лучше разобраться в материале.

---

### Задание 4*

Придумайте способ версионировать приложение, чтобы каждый следующий запуск сборки присваивал имени файла новую версию. Таким образом, в репозитории Nexus будет храниться история релизов.

Подсказка: используйте переменную BUILD_NUMBER.

В качестве ответа пришлите скриншоты с настройками проекта и результатами выполнения сборки.

Ответ:

Заметки от преподавателя:

Сейчас у вас достаточно сделано для зачета. Но, оставлю тут в качестве примера взаимодействие с переменными:

CGO_ENABLED=0 GOOS=linux /usr/local/go/bin/go build -o v$BUILD_NUMBER:$GIT_COMMIT:$GIT_BRANCH:outfile.go

curl -v -u admin:admin http://192.168.56.10:8081/repository/raw1/ --upload-file v$BUILD_NUMBER:$GIT_COMMIT:$GIT_BRANCH:outfile.go


## Более полная работа, со всеми неудачами по ходу выполнения в Google:

[Моя работа по Jenkins](https://docs.google.com/document/d/1S7XRWQkFn0lKEdqHfY3VVnEOXM-hrDsH/edit?usp=share_link&ouid=104113173630640462528&rtpof=true&sd=true)

