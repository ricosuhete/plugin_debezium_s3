# Topic Path Partitioner for Kafka Connect S3 Sink

Um Particionador customizado para o [Confluent S3 Sink Connector](https://docs.confluent.io/kafka-connect-s3-sink/current/overview.html) que converte o nome do tópico Kafka em uma estrutura de diretórios hierárquica e limpa no Amazon S3.

## 🚀 Motivação

O Apache Kafka não permite o caractere `/` em nomes de tópicos. Quando usamos o Debezium, os tópicos são gerados no formato `prefixo.database.tabela`. 

Tentar usar transformações (SMT como `RegexRouter`) no Sink para colocar `/` no nome do tópico resulta no erro `No writer found for topic partition`, pois quebra o mapeamento interno do S3 Sink.

Este plugin resolve o problema atuando **apenas na camada de storage do S3**, preservando o tópico original no Kafka.

## ⚙️ O que ele faz

1. Remove o prefixo do tópico (o texto antes do primeiro ponto).
2. Substitui os demais pontos (`.`) por barras (`/`).

**Exemplo prático:**

| Tópico Kafka | Path gerado no S3 |
| :--- | :--- |
| `m03.inventory.customers` | `inventory/customers/partition=0/` |
| `m01.signals.heartbeat` | `signals/heartbeat/partition=0/` |

## 🛠️ Como compilar (Build)

Você pode compilar usando Maven nativo ou via Docker (sem precisar instalar o Java na sua máquina).

**Via Docker:**
```bash
docker run --rm \
  -v $(pwd):/app \
  -w /app \
  maven:3.9-eclipse-temurin-17 \
  mvn clean package
````

## ⚙️ Configuração
"partitioner.class": "br.com.dio.connect.partitioner.TopicPathPartitioner"

## Compatibilidade
Kafka Connect 3.x <br>
Confluent S3 Sink Connector <br>
Java 17+
