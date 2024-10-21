![pyenv](https://img.shields.io/badge/pyenv-white?style=for-the-badge)
![Poetry](https://img.shields.io/badge/Poetry-%233B82F6.svg?style=for-the-badge&logo=poetry&logoColor=0B3D8D)
![ipykernel](https://img.shields.io/badge/ipykernel-3670A0?style=for-the-badge)
![newsapi](https://img.shields.io/badge/newsapi-1a73e8?style=for-the-badge)
![NumPy](https://img.shields.io/badge/numpy-%23013243.svg?style=for-the-badge&logo=numpy&logoColor=white)
![Pandas](https://img.shields.io/badge/pandas-%23150458.svg?style=for-the-badge&logo=pandas&logoColor=white)
![pyarrow](https://img.shields.io/badge/pyarrow-222832?style=for-the-badge)
![Python](https://img.shields.io/badge/python-3670A0?style=for-the-badge&logo=python&logoColor=ffdd54)
![poo](https://img.shields.io/badge/poo-black?style=for-the-badge)
![Apache Spark](https://img.shields.io/badge/Apache%20Spark-FDEE21?style=for-the-badge&logo=apachespark&logoColor=black)
![Apache Kafka](https://img.shields.io/badge/Apache%20Kafka-000?style=for-the-badge&logo=apachekafka)
![Jupyter Notebook](https://img.shields.io/badge/jupyter-%23FA0F00.svg?style=for-the-badge&logo=jupyter&logoColor=white)
![Visual Studio Code](https://img.shields.io/badge/Visual%20Studio%20Code-0078d7.svg?style=for-the-badge&logo=visual-studio-code&logoColor=white)
![GitHub](https://img.shields.io/badge/github-%23121011.svg?style=for-the-badge&logo=github&logoColor=white)
![Databricks](https://img.shields.io/badge/databricks-222832?style=for-the-badge&logo=databricks)


# Enunciado Projeto Proposto

O grupo trabalha no time de engenharia de dados na HealthGen, uma empresa especializada em genômica e pesquisa de medicina personalizada. A genômica é o estudo do conjunto completo de genes de um organismo, desempenha um papel fundamental na medicina personalizada e na pesquisa biomédica. Permite a análise do DNA para identificar variantes genéticas e mutações associadas a doenças e facilita a personalização de tratamentos com base nas características genéticas individuais dos pacientes.

A empresa precisa se manter atualizada sobre os avanços mais recentes na genômica, identificar oportunidades para pesquisa e desenvolvimento de tratamentos personalizados e acompanhar as tendências em genômica que podem influenciar estratégias de pesquisa e desenvolvimento. Pensando nisso, o time de dados apresentou uma proposta de desenvolvimento de um sistema que coleta, analisa e apresenta as últimas notícias relacionadas à genômica e à medicina personalizada, e também estuda o avanço do campo nos últimos anos. 

O time de engenharia de dados tem como objetivo desenvolver e garantir um pipeline de dados confiável e estável. As principais atividades são:

## 1. Consumo de dados com a News API: 
Implementar um mecanismo para consumir dados de notícias de fontes confiáveis e especializadas em genômica e medicina personalizada, a partir da News API: 
https://newsapi.org/

## 2. Definir Critérios de Relevância:
* Desenvolver critérios precisos de relevância para filtrar as notícias. Por exemplo, o time pode se concentrar em notícias que mencionem avanços em sequenciamento de DNA, terapias genéticas personalizadas ou descobertas relacionadas a doenças genéticas específicas.

## 3. Cargas em Batches:
* Armazenar as notícias relevantes em um formato estruturado e facilmente acessível para consultas e análises posteriores. Essa carga deve acontecer 1 vez por hora. Se as notícias extraídas já tiverem sidos armazenadas na carga anterior, o processo deve ignorar e não armazenar as notícias novamente, os dados carregados não podem ficar duplicados.

## 4. Dados transformados para consulta do público final
* Quantidade de notícias por ano, mês e dia de publicação;
* Quantidade de notícias por fonte e autor;
* Quantidade de aparições de 3 palavras chaves por ano, mês e dia de publicação

# Estrutura do Projeto

## Variáveis de Ambiente
Na raiz do projeto, deve ser criada uma pasta chamada env, onde será armazenado o arquivo env_variables.py. Neste arquivo, será configurada a seguinte variável de ambiente:

*apikey*: Chave deve ser criada no portal [News API](https://newsapi.org)

Chave de acesso à API, que deve ser gerada individualmente no portal News API. Cada desenvolvedor precisa criar sua própria chave de acesso no portal e inseri-la nesta variável para utilizar os serviços da API.

## Ambiente de Desenvolvimento
O projeto é modularizado, com cada módulo armazenando as funções específicas da aplicação. Para testes individuais de funções, utilizamos notebooks localizados no diretório tests.

Arquivos Ignorados
O arquivo .gitignore está configurado para ignorar os seguintes diretórios e arquivos:

* env/env_variables.py: Arquivo contendo as variáveis de ambiente e chaves de acesso.
* pycache: Diretórios de cache gerados automaticamente, geralmente criados dentro dos módulos, e utilizados apenas no ambiente de testes.

## Integração com a API - News API
Utilizamos a biblioteca oficial da News API para facilitar as requisições, a qual pode ser instalada com o seguinte comando:

pip install newsapi-python

Limitações da API (Plano Gratuito):
* A busca por artigos está restrita aos últimos 30 dias.
* Limite de 50 requisições a cada 12 horas.
* O retorno da API é limitado a 100 artigos por página.
* A última página pode conter mais de 100 artigos, mas não foi implementada uma estratégia para adquirir todos os artigos adicionais, devido à limitação de requisições e à falta de garantia de que todos os artigos seriam obtidos.

Essas limitações foram consideradas no desenvolvimento do projeto, e como o foco principal não é a aquisição de todos os artigos possíveis, essas restrições não comprometem o objetivo final.

## Adequação para Execução no Databricks
Para garantir que o projeto funcione corretamente dentro do ambiente Databricks, foi criada uma pasta chamada Databricks, que contém cinco arquivos necessários para rodar o programa no cluster. Abaixo estão os detalhes de cada arquivo:

1 - setup_kafka_zookeeper

Neste arquivo, o Apache Kafka é baixado, descompactado e instalado no cluster do Databricks. Além disso, o funcionamento do Zookeeper, que é responsável pela coordenação dos serviços distribuídos, é iniciado.

2 - setup_kafka_server

Este arquivo contém o comando necessário para iniciar o servidor Kafka, permitindo a comunicação entre o cluster e os consumidores/produtores de mensagens.

3 - setup_kafka_topic

Aqui, o tópico Kafka utilizado no projeto é criado, onde as mensagens da API serão publicadas e consumidas.

4 - Pipeline

Este arquivo contém a implementação de toda a pipeline de consumo e processamento de dados. Para garantir o funcionamento correto, é necessário instalar quatro bibliotecas no ambiente Databricks, através do terminal:

pip install --upgrade pip
pip install kafka-python
pip install newsapi-python
pip install schedule

5 - insertion

Neste arquivo, são definidos os critérios de busca de artigos na API NewsAPI, o produtor de mensagens Kafka (producer), e a rotina de carga em lotes (batch) que insere os dados no tópico previamente criado.

## Conclusão
Este projeto foi desenvolvido com o objetivo de integrar a News API ao Kafka e permitir o processamento de dados dentro de um ambiente distribuído como o Databricks. Seguindo as instruções fornecidas ao longo deste documento, é possível configurar o ambiente corretamente e garantir o funcionamento eficiente da pipeline de dados.

Apesar das limitações da API no plano gratuito, o projeto foi planejado para ser escalável e modular, facilitando a manutenção e a adição de novas funcionalidades no futuro. O uso de tecnologias como Kafka e Databricks permite um processamento em tempo real e em lote, tornando o projeto uma base sólida para quem deseja explorar a integração de APIs e processamento de dados em larga escala.

Agradecemos o interesse no projeto e ficamos à disposição para sugestões ou melhorias. Sinta-se à vontade para contribuir, testar e personalizar conforme suas necessidades.





