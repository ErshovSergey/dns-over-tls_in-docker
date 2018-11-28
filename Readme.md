[![GitHub license](https://img.shields.io/badge/license-MIT-blue.svg)](https://raw.githubusercontent.com/ErshovSergey/dns-over-tls_in-docker/master/LICENSE) 

## DNS over TLS 
Образ docker с dns сервером для сокрытия содержимого dns запросов:
- используется проксирующий dns сервер [unbound](https://nlnetlabs.nl/projects/unbound/about/).
- запросы отправляются на сервера определенные в [unbound/unbound.conf](unbound/unbound.conf)
- подробнее можно почитать на [cloudflare.com](https://developers.cloudflare.com/1.1.1.1/dns-over-tls/)
- размер образа ~ 16 mb.
### Использование
- клонировать репозиторий
```
git clone https://github.com/ErshovSergey/dns-over-tls.git
cd dns-over-tls
```
- скопировать файл *.env-default* в *.env*
```
cp .env-default .env
```
- изменить необходимые параметры в файле *.env*
```
nano .env
```
- закоментировать/раскомментировать/добавить свой dns сервер в файле [unbound/unbound.conf](unbound/unbound.conf)
```
nano unbound/unbound.conf
```
- собрать образ и запустить
```
docker-compose up --build -d --remove-orphans --force-recreate
```
### Полезные команды
Остановить и удалить контейнер
```
docker-compose down
```
### Дополнительная настройка
Для переадресации некоторых зон dns на определенные серверы необходимо создать конфигурационный файл (например add.conf) и поместить его в *$DoT_PATH*
 (путь определяется в .env). 
 
 Например за зону *.local* будет отвечать сервер x.xx.y.yy@53  
 Файл *add.conf* будет следующего содержания
```console
private-domain: "local."
#если ip адреса из "серых" - необходима следующая строка
domain-insecure: "local"
forward-zone:
        name: "local."
        forward-addr: x.xx.y.yy@53
        forward-tls-upstream: no
        forward-first: yes
```

