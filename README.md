# Flume e Kafka

![Flafka](media/flafkaLogo.png 'Flafka')

## Introdução

Neste trabalho prático de Processamento de Fluxos Discretos e Contínuos de Dados, vamos explorar o uso integrado das ferramentas Flume e Kafka para processamento de dados em tempo real.

### Kafka

O Apache Kafka é uma plataforma de streaming de dados usada para processar e transmitir informações em tempo real. Ele é muito eficiente para lidar com grandes volumes de dados e é amplamente utilizado em sistemas que necessitam de alta velocidade e confiabilidade na troca de informações. Kafka organiza os dados em `tópicos` e os divide em `partições`, permitindo que várias aplicações possam processar os dados de maneira paralela. É usado em aplicações que vão desde análise de dados em tempo real até integração entre sistemas distribuídos.

### Flume

O Apache Flume é uma ferramenta que ajuda a mover dados de um lugar para outro de forma confiável. É útil para coletar informações de diferentes fontes, como arquivos de log ou sensores, e enviá-las para sistemas de armazenamento como Hadoop. É uma maneira eficiente de lidar com grandes quantidades de dados em ambientes de computação distribuída.

## Objetivos

Utilizar o `Flafka` para processamento de dados em tempo real, que combina as capacidades de ingestão de dados do Flume com a robustez de mensagens do Kafka. Nesta integração:

- `Flume` atua como um agente de coleta de dados que pode ser configurado para ler dados de várias fontes, como logs de arquivos, e enviá-los para diferentes destinos.
- `Kafka` atua como um sink para Flume. Flume pode enviar dados diretamente para tópicos Kafka.

## Experimentos

### Passo 1: Inicializando a Máquina Virtual

Para essa atividade, precisamos utilizar uma máquina virtual. Aqui podemos vê-la inicializada.
![Inicializando a VM](media/initVM.png 'Inicializando a VM')

### Passo 2: Inicializando Servidor Kafka

Primeiramente, precisamos inicializar o servidor do Kafka. Para isso, execute o seguinte comando no terminal:

```bash
sudo /home/puc/kafka_2.11-1.0.0/bin/kafka-server-start.sh /home/puc/kafka_2.11-1.0.0/config/server.properties
```

![Inicializando Kafka Server](media/initKafkaSrv.gif 'Inicializando Kafka Server')
Este passo é fundamental para garantir que a plataforma esteja pronta para receber e processar fluxos contínuos de dados em tempo real.

### Passo 3: Criar Tópico

Precisamos criar um tópico para receber as mensagens. Vamos criar o tópico `banana-test` utilizando o comando:

```bash
sudo /home/puc/kafka_2.11-1.0.0/bin/kafka-topics.sh --create --zookeeper localhost:2181 --replication-factor 1
--partitions 1 --topic banana-test
```

![Criando Tópico](media/createTopic.gif 'Criando Tópico')

### Passo 4: Listar Tópico

Para validar se o tópico foi criado corretamente, podemos listar todos os tópicos usando o comando:

```bash
sudo /home/puc/kafka_2.11-1.0.0/bin/kafka-topics.sh --list --zookeeper localhost:2181
```

![Listando Tópico](media/listTopic.gif 'Listando Tópico')

### Passo 5: Producer

Com o tópico criado, vamos inicializar o `producer` para que possamos inserir mensagens nele. Para executar o script do producer, use o seguinte comando:

```bash
sudo /home/puc/kafka_2.11-1.0.0/bin/kafka-console-producer.sh --broker-list localhost:9092 --topic banana-test
```

Quando o producer estiver `pronto`, forneça algumas mensagens para ele.
![Kafka Producer](media/initKafkaProducer.gif 'Kafka Producer')

### Passo 6: Consumer

Para validar se os dados foram inseridos corretamente no tópico, precisamos criar um `consumer` vinculado ao tópico `banana-test`. Utilize o comando:

```bash
sudo /home/puc/kafka_2.11-1.0.0/bin/kafka-console-consumer.sh --zookeeper localhost:2181 --topic banana-test
```

Ao inicializá-lo, percebemos que nenhuma mensagem é exibida inicialmente. Isso ocorre porque não utilizamos a flag `--from-beginning`. Sem essa flag, o consumidor exibirá apenas as mensagens a partir da última mensagem que foi processada. Use o comando:

```bash
sudo /home/puc/kafka_2.11-1.0.0/bin/kafka-console-consumer.sh --zookeeper localhost:2181 --topic banana-test
--from-beginning
```

Ao usar a flag `--from-beginning`, todas as mensagens previamente inseridas são exibidas:
![Kafka Consumer](media/iniKafkaConsumer.gif 'Kafka Consumer')

### Passo 7: Flume

Agora precisamos criar um agente Flume. O agente irá processar os arquivos da pasta `spool-test` e ao finalizar adicionará o sufixo `.BANANA`. Esse sufixo foi escolhido para diferenciar do padrão, indicando que o arquivo foi processado e encaminhado para o sink, que neste caso é o logger. Use o seguinte comando para inicializar o agente:

```bash
cd /apache-flume-1.8.0-bin/conf
flume-ng agent --conf-file spool-to-kafka.properties --name agent1 -Dflume.root.logger=WARN,console
```

![Agente Flume](media/initFlume.gif 'Agente Flume')

### Passo 8: Flafka

Finalmente chegamos na última etapa. Juntando todas as etapas anteriores, podemos executar o Flafka. Primeiramente, precisamos definir a propriedade do agente que vincula o tópico para `banana-test` e que irá processar os arquivos dentro da pasta `spool-to-kafka`. Após isso, podemos inicializar o novo agente.

```bash
cd /apache-flume-1.8.0-bin/conf
flume-ng agent --conf-file spool-to-kafka.properties --name agent2 -Dflume.root.logger=WARN,console
```

Também precisamos inicializar o consumidor para receber os textos que foram processados.

```bash
sudo /home/puc/kafka_2.11-1.0.0/bin/kafka-console-consumer.sh --zookeeper localhost:2181 --topic banana-test
```

Ao mover um arquivo para a pasta `spool-to-kafka`, o arquivo será processado e receberá um sufixo `.BANANA`. Após o processamento, ele será enviado para o tópico e o consumidor exibirá todo o texto.
![Flafka](media/flafka.gif 'Flafka')

## Conclusão

Podemos concluir que ao combinar essas duas ferramentas, é viável processar dados em tempo real de maneira eficiente, desde que as configurações dos sources e sinks estejam corretamente ajustadas. Embora seja um cenário relativamente simples para ambientes acadêmicos, essa integração oferece uma base sólida para aplicação em cenários profissionais mais complexos.

A utilização do Flafka proporciona uma abordagem robusta e escalável para a ingestão, processamento e distribuição de dados em tempo real. Isso se mostra crucial em ambientes onde a velocidade e a confiabilidade na manipulação de grandes volumes de dados são essenciais, como em análises de streaming, monitoramento em tempo real e processamento de logs.

Ao explorar e dominar essa integração, os profissionais podem não apenas implementar soluções eficazes de processamento de dados, mas também estar preparados para enfrentar desafios e demandas crescentes em ambientes corporativos e industriais que dependem cada vez mais de insights em tempo real para tomada de decisões estratégicas.
