# Интеграция Usergate UTM c ELK-стеком.
Эти конфигурационные файлы позволяют получать логи с устройства UserGate UTM, парсить их и представлять в разобраном виде в интерфейс Kibana.

![ELK with UG logs](/images/ug_elk.png)

## Как пользоваться
Вы должны добавить файлы с правилами парсинга в ваши настройки Logstash и Elasticseach.

## Как протестировать в Docker?
Примерно так:  
`docker run -d -p 9200:9200/tcp -p 9300:9300/tcp -it -h elasticsearch --name elasticsearch elasticsearch:6.8.13 `  

`docker run -d -p 192.168.95.125:5601:5601 -h kibana --name kibana --link elasticsearch:elasticsearch kibana:6.8.13`  

`docker run -h logstash --name logstash --link elasticsearch:elasticsearch -it --rm -v /opt/logstash:/config-dir -p 192.168.95.125:5514:5514 logstash:6.8.13 -f /config-dir/mylogstash.conf`  

Что здесь важно. Нужно замапить каталог с конфигами в каталог `/config-dir` в контейнере. В случае команды выше, конфиги лежат в `/opt/logstash`

## Улучшения
При создании индекса для WEB-логов, можно уточнить, что requestUrl не просто string, a URL.

### TODO
[ ] Разобраться с полем  destinationTranslatedPort. Должно быть числовым.
