# Análise de preços e vendas de combustíveis com Apache Spark

Trabalho desenvolvido como conclusão do módulo Processamento de Big Data com Apache Spark do **MBA em Engenharia de Dados: Big Data e IA**do instituto Infnet.

### Problema

O presente projeto integra dados da Agência Nacional do Petróleo (ANP) com informações geográficas e populacionais, obtidas através do Instituto Brasileiro de Geografia e Estatística (IBGE), com o intuito analisar métricas do mercado de combustível brasileiro.
A solução foi pensada para efetuar processos ETL, explorar funcionalidades spark, aplicar arquitetura Medallion e tabelas delta, sendo executada dentro de um ambiente DataBricks.

### Solução
Foi adotado um pipeline estruturado implementando a estrutura Medallion, em que:

- **Raw:** Arquivos de extração em seu formato original, mantidos em um Volume do Unity Catalog;
- **Bronze:** Ingestão dos arquivos .csv e .json em tabelas Delta, realizando pouca ou nenhuma alteração dos dados;
- **Silver:** Conversão de tipos, padronização, validação e integração dos dados das diferentes bases de dados;
- **Gold:** Geração de indicadores voltadas às métricas do negócio.

O projeto também demonstra a aplicação dos seguintes métodos: persistência em **_Parquet_**, _Spark SQL_, agregações, joins, pivotação, Window Functions.

Também é realizado o **Streaming** e comparação de **estratégias de otimização**.

### Métricas calculadas
As métricas de negógcio calculadas constam de: 
1. Preço médio anual por tipo de combustível;
2. Rankings estaduais;
    * 2.1. preço médio por combustível;
    * 2.2. Ranking estadual de volume vendido por combustível;
    * 2.3. Ranking estadual de volume vendido por habitante;
    * 2.4. Ranking estadual de volume vendido por km²;

### Estrutura do projeto

| Arquivo | Descrição |
|---|---|
| `sql/ddl.sql.ipynb` | Cria o catálogo, schemas, Volume e as tabelas Delta para as camadas Bronze, Silver e Gold. |
| `Notebooks/0-Stage.ipynb` | Baixa os arquivos da ANP e do IBGE, realizando a persistência no Volume data. Realiza extrações de arquivos .zip quando necessário. |
| `Notebooks/1-Bronze.ipynb` | Lê arquivos .csv e .json, adicionando metadados para persistência em tabela delta. Demonstra persistência em Parquet. |
| `Notebooks/2-Silver.ipynb` | Realizatramento dos dados. Converte tipos, padroniza textos, valida registros. |
| `Notebooks/3-Gold.ipynb` | Calcula os indicadores estaduais, realiza rankings através de windowfunctions, demonstra pivotação e consulta com Spark SQL. |
| `Notebooks/4-Streaming.ipynb` | Processa preços com Structured Streaming, watermark, janelas semanais, checkpoints e tabelas Delta. |
| `Notebooks/5-Optimization.ipynb` | (1) Compara função Python vs função nativa do Spark e (2) join convencional vs broadcast join, comparando os tempos de execução. |

### Execução

1. Criar as tabelas sql, através da execução de `sql/ddl.sql.ipynb`.
2. Executar notebooks em ordem crescente; de `0-Stage` a `5-Optimization`.
