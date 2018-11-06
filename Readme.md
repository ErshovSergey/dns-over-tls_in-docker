[![GitHub license](https://img.shields.io/badge/license-MIT-blue.svg)](https://raw.githubusercontent.com/ErshovSergey/dns-over-tls_in-docker/master/LICENSE) 

## DNS over TLS 
Образ docker с dns сервером для сокрытия содержимого dns запросов:
- построен на проксирующем dns сервере
[unbound](https://nlnetlabs.nl/projects/unbound/about/).
- запросы отправляются на [cloudflare-dns.com](https://cloudflare-dns.com)
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
- собрать образ и запустить
```
docker-compose up --build -d
```
### Полезные команды
Остановить и удалить контейнер
```
docker-compose down
```
### Дополнительная настройка
Для переадресации некоторых зон dns на определенные серверы необходимо создать конфигурационный файл (например add.conf) и поместить его в *$DoT_PATH*
 (путь определяется в .env). 
 
 Например для зоны *.local* файл будет следующего содержания
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

