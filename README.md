# Домашнее задание к занятию "`«Система мониторинга Zabbix»`" - `Копаческу Владимир`
В практике есть 2 основных и 1 дополнительное (со звездочкой) задания. Первые два нужно выполнять обязательно, третье - по желанию и его решение никак не повлияет на получение вами зачета по этому домашнему заданию, при этом вы сможете глубже и/или шире разобраться в материале.

Пожалуйста, присылайте на проверку всю задачу сразу. Любые вопросы по решению задач задавайте в чате учебной группы.

Цели задания
Научиться устанавливать Zabbix Server c веб-интерфейсом
Научиться устанавливать Zabbix Agent на хосты
Научиться устанавливать Zabbix Agent на компьютер и подключать его к серверу Zabbix
Чеклист готовности к домашнему заданию
 Просмотрите в личном кабинете занятие "Система мониторинга Zabbix"
Инструкция по выполнению домашнего задания
Сделайте fork репозитория c шаблоном решения к себе в Github и переименуйте его по названию или номеру занятия, например, https://github.com/имя-вашего-репозитория/gitlab-hw или https://github.com/имя-вашего-репозитория/8-03-hw).
Выполните клонирование этого репозитория к себе на ПК с помощью команды git clone.
Выполните домашнее задание и заполните у себя локально этот файл README.md:
впишите вверху название занятия и ваши фамилию и имя;
в каждом задании добавьте решение в требуемом виде: текст/код/скриншоты/ссылка;
для корректного добавления скриншотов воспользуйтесь инструкцией «Как вставить скриншот в шаблон с решением»;
при оформлении используйте возможности языка разметки md. Коротко об этом можно посмотреть в инструкции по MarkDown.
После завершения работы над домашним заданием сделайте коммит (git commit -m "comment") и отправьте его на Github (git push origin).
Для проверки домашнего задания преподавателем в личном кабинете прикрепите и отправьте ссылку на решение в виде md-файла в вашем Github.
Любые вопросы задавайте в чате учебной группы и/или в разделе «Вопросы по заданию» в личном кабинете.


---

### Задание 1
Установите Zabbix Server с веб-интерфейсом.

Процесс выполнения
1. `Выполняя ДЗ, сверяйтесь с процессом отражённым в записи лекции.`
2. `Установите PostgreSQL. Для установки достаточна та версия, что есть в системном репозитороии Debian 11.`
3. `Пользуясь конфигуратором команд с официального сайта, составьте набор команд для установки последней версии Zabbix с поддержкой PostgreSQL и Apache.`
4. `Выполните все необходимые команды для установки Zabbix Server и Zabbix Web Server.`
Требования к результаты
1. `Прикрепите в файл README.md скриншот авторизации в админке.`
2. `Приложите в файл README.md текст использованных команд в GitHub.`
### Решение 1

1. `Выполненые команды`
```
 sudo wget https://repo.zabbix.com/zabbix/6.4/debian/pool/main/z/zabbix-release/zabbix-release_6.4-1+debian12_all.deb
 sudo dpkg -i zabbix-release_6.4-1+debian12_all.deb
 sudo apt update
 sudo apt install zabbix-server-pgsql zabbix-frontend-php php8.2-pgsql zabbix-apache-conf zabbix-sql-scripts zabbix-agent
 sudo -u postgres createuser --pwprompt zabbix
 sudo -u postgres createdb -O zabbix zabbix
 sudo zcat /usr/share/zabbix-sql-scripts/postgresql/server.sql.gz | sudo -u zabbix psql zabbix
 sudo sed -i 's/# DBPassword=/DBPassword=123456789/g' /etc/zabbix/zabbix_server.conf
 sudo systemctl restart zabbix-server apache2 # zabbix-agent
 sudo systemctl status zabbix-server
 ip a

```

`Скрин 1 к заданию 1`                                    
![alt text](https://github.com/Replica63/hw-zabbix/blob/main/img/1.png)
![alt text](https://github.com/Replica63/hw-zabbix/blob/main/img/2.png)
![alt text](https://github.com/Replica63/hw-zabbix/blob/main/img/3.png)
![alt text](https://github.com/Replica63/hw-zabbix/blob/main/img/4.png)


---

### Задание 2



### Решение 2

1. ![alt text](https://github.com/Replica63/Jenkins-8-02.md/blob/main/img/4.png)
2. `Script`

```
Поле для вставки кода...
pipeline {
 agent any
 stages {
  stage('Git') {
   steps {git 'https://github.com/Replica63/sdvps-materials.git'}
  }
  stage('Test') {
   steps {
    sh '/usr/bin/go test .'
   }
  }
  stage('Build') {
   steps {
    sh 'docker build . -t ubuntu-bionic:8081/hello-world:v$BUILD_NUMBER'
   }
  }
 }
}
```
3. ![alt text](https://github.com/Replica63/Jenkins-8-02.md/blob/main/img/4.5.png)



---

### Задание 3
Что нужно сделать:

1. Установите на машину Nexus.
2. Создайте raw-hosted репозиторий.
3. Измените pipeline так, чтобы вместо Docker-образа собирался бинарный go-файл. Команду можно скопировать из Dockerfile.
4. Загрузите файл в репозиторий с помощью jenkins.
В качестве ответа пришлите скриншоты с настройками проекта и результатами выполнения сборки.
### Решение 3


1. ![alt text](https://github.com/Replica63/Jenkins-8-02.md/blob/main/img/n1.png)
2. ![alt text](https://github.com/Replica63/Jenkins-8-02.md/blob/main/img/n2.png)
3. `Измените pipeline так, чтобы вместо Docker-образа собирался бинарный go-файл. Команду можно скопировать из Dockerfile.`
```
pipeline {
    agent any
    stages {
        stage ('Git') {
         steps { git 'https://github.com/Replica63/sdvps-materials.git'}
        }
        stage ('Test') {
         steps {
             sh '/usr/bin/go test .'
         }
        }
        stage ('Build') {
            steps {
                sh '/usr/bin/go build -a -installsuffix nocgo -o main .'
            }
        }
        stage ('Push') {
            steps {
                sh 'curl -u admin:admin --upload-file ./main http://127.0.0.53:8081/repository/Nexus/main'
            }        
        }
    }
}


```
4.![alt text](https://github.com/Replica63/Jenkins-8-02.md/blob/main/img/n3.png)
  ![alt text](https://github.com/Replica63/Jenkins-8-02.md/blob/main/img/n4.png) 
  ![alt text](https://github.com/Replica63/Jenkins-8-02.md/blob/main/img/n5.png)
  ![alt text](https://github.com/Replica63/Jenkins-8-02.md/blob/main/img/n6.png)


