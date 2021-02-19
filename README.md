### Установка
```
pip install -r requirements.txt
```
### Пример запуска
```
ansible-playbook -i inventory create_topic.yml --extra-vars '{"topic_name": "test", "partitions_number": "2", "replication_factor":"1", "cleanup_policy": "delete", "retention_ms": "574930", "min_insync_replicas": "1" }'
```
### Входные переменные
- topic_name - имя топика
- partitions_number - количество партиций, по умолчнию из конфига брокера
- replication_factor - фактор репликации
- retention_ms - ьаксимальное время хранения журнала от 30 минут до 20160
- cleanup_policy - тип очистки, по умолчанию delete [compact, delete]
- min_insync_replicas - минимальное количество реплик с подтвержденной записью, от 1 до replication_factor
### Проверка работоспособности
На одной из нод кафки:
```
[root@kafka_3 ~]#echo "Helo World" | /app/kafka/bin/kafka-console-producer.sh --topic test --broker-list kafka_1:9092,kafka_2:9092,kafka_3:9092 
```
На другой ноде должны прочитать что отправили:
```
[root@kafka_1 ~]# /app/kafka/bin/kafka-console-consumer.sh --topic test --bootstrap-server kafka_1:9092,kafka_2:9092,kafka_3:9092 --from-beginning
Helo World
```
### Пример inventory
```
[kafka]
kafka_1.domain.ru
kafka_2.domain.ru
kafka_3.domain.ru

[zookeeper]
zookeeper_1.domain.ru
zookeeper_2.domain.ru
zookeeper_3.domain.ru
```