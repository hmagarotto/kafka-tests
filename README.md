# Kafka Tests

Testes realizados com o Kafka na versão utilizada em produção 1.1.0.

Esta versão é antiga e estamos planejando a migração para a versão 2 do Kafka em produção.

Os comandos abaixo podem mudar para a versão 2 do Kafka.

## Encontros:

### 1° encontro (2021-12-10)

Assuntos:

* Setup inicial
* Brokers
* Network

![Network](/diagrams/network.png)

Servers:

```
# inside docker network
KAFKA_SERVERS=kafka-0:9092,kafka-1:9092,kafka-2:9092
ZOOKEEPER_SERVERS=zookeeper:2181

# on host:
KAFKA_SERVERS=localhost:19090,localhost:19091,localhost:19092
ZOOKEEPER_SERVERS=localhost:2181
```

Comandos:

```
# start do cluster
docker-compose up -d zookeeper
docker-compose up -d
```

### 2° encontro (2022-02-04)

Assuntos:

* Criação de tópicos
* Réplicas + Leaders + InSync
* Rebalance

![Consumers vs Partitions](/diagrams/consumers-vs-partitions.png)




Comandos:

```
source env.sh

# verificar metadados do cluster
docker run --rm --net=host confluentinc/cp-kafkacat \
  kafkacat -L -b "${KAFKA_SERVERS}"

# criar tópico TESTE
kafka-topics.sh \
  --zookeeper "${ZOOKEEPER_SERVERS}" \
  --topic TESTE \
  --create \
  --replication-factor=2 \
  --partitions=16

# consumir tópico TESTE
kafka-console-consumer.sh \
  --bootstrap-server "${KAFKA_SERVERS}" \
  --topic TESTE \
  --group TEST_GROUP

# produzir no tópico TESTE
kafka-console-producer.sh \
  --broker-list "${KAFKA_SERVERS}" \
  --topic TESTE

# distribuição das partições entre consumidores
kafka-consumer-groups.sh \
  --bootstrap-server 127.0.0.1:19090 \
  --group TEST_GROUP \
  --describe |\
  awk '{print $1,$6}' | sort | uniq -c
```

### 3° encontro (2022-02-18)

Assuntos:

* partition size
* partition offset
* fetch
* session timeout
* heartbeat

![Partitions](/diagrams/partitions.png)

### 4° encontro (2022-02-25)

Assuntos:

* UIs
* metrics
* perf
* consumer and producer tests
