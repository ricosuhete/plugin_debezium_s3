# plugin_debezium_s3
Particionador customizado para Kafka Connect S3 Sink. Converte tópicos (ex: m01.db.tabela) em caminhos hierárquicos no S3 (db/tabela/), removendo o prefixo e trocando pontos por barras. Organiza pastas no S3 de forma limpa, sem violar as regras de nomenclatura do Kafka ou quebrar o mapeamento do Sink.
