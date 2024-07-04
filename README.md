# Flume e Kafka

![Flafka](media/flafkaLogo.png 'Flafka')

## Introdução

Neste trabalho prático de Processamento de Fluxos Discretos e Contínuos de Dados, vamos explorar o uso integrado das ferramentas Flume e Kafka para processamento de dados em tempo real.

### Kafka

O Apache Kafka é uma plataforma de streaming de dados usada para processar e transmitir informações em tempo real. Ele é muito eficiente para lidar com grandes volumes de dados e é amplamente utilizado em sistemas que necessitam de alta velocidade e confiabilidade na troca de informações. Kafka organiza os dados em "tópicos" e os divide em "partições", permitindo que várias aplicações possam processar os dados de maneira paralela. É usado em aplicações que vão desde análise de dados em tempo real até integração entre sistemas distribuídos.

### Flume

O Apache Flume é uma ferramenta que ajuda a mover dados de um lugar para outro de forma confiável. É útil para coletar informações de diferentes fontes, como arquivos de log ou sensores, e enviá-las para sistemas de armazenamento como Hadoop. É uma maneira eficiente de lidar com grandes quantidades de dados em ambientes de computação distribuída.

## Objetivos

## Experimentos

1. Inicializando a Máquina Virtual
![Inicializando a VM](media/initVM.png 'Inicializando a VM')
Para essa atividade, precisamos utilizar uma máquina virtual. Aqui podemos vê-la inicializada.
2. Inicializando Servidor Kafka
![Inicializando Kafka Server](media/initKafkaSrv.gif 'Inicializando Kafka Server')
Aqui estamos iniciando o servidor Kafka. Este passo é fundamental para garantir que a plataforma esteja pronta para receber e processar fluxos contínuos de dados em tempo real.
3. Criar Tópico
![Criando Tópico](media/createTopic.gif 'Criando Tópico')
Estamos criando o tópico `banana-test` no Kafka para que possamos produzir mensagens.
4. Listar Tópico
![Listando Tópico](media/listTopic.gif 'Listando Tópico')
Ao listar os tópicos, podemos ver que o `banana-test` está entre eles. Sendo assim, podemos prosseguir para a próxima etapa e começar a produzir mensagens.
5. Producer
![Kafka Producer](media/initKafkaProducer.gif 'Kafka Producer')
Aqui inicializamos o producer vinculado ao tópico `banana-test`. Adicionamos algumas mensagens para o consumer.
6. Consumer
![Kafka Consumer](media/iniKafkaConsumer.gif 'Kafka Consumer')
Para validar se os dados foram inseridos corretamente no tópico, precisamos criar um consumidor vinculado ao tópico `banana-test`. Ao inicializá-lo, percebemos que nenhuma mensagem é exibida inicialmente. Isso ocorre porque não utilizamos a flag `--from-beginning`. Sem essa flag, o consumidor exibirá apenas as mensagens a partir da última mensagem que foi processada. Ao usar a flag `--from-beginning`, todas as mensagens previamente inseridas são exibidas.
7. Flume
8. Flafka

## Conclusão
