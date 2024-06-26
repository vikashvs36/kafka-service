# Setting Up Kafka along with Zookeeper Locally Using Docker

First we have to install docker and docker-compose on our local system and to verify that the version use this command as given below:

> docker --version

> docker compose version

After installing docker and docker compose create a docker-compose.yaml file. 
## Creating docker-compose.yaml file.

    version: "3"
        networks:
        myNetwork:
        
        services:
        
        zookeeper:
        image: 'bitnami/zookeeper:latest'
        ports:
        - '2181:2181'
        environment:
          - ALLOW_ANONYMOUS_LOGIN=yes
          networks:
          - myNetwork
        
        kafka:
        image: 'bitnami/kafka:latest'
        user: root
        ports:
        - '9092:9092'
        environment:
          - KAFKA_BROKER_ID=1
          - KAFKA_LISTENERS=PLAINTEXT://:9092
          - KAFKA_ADVERTISED_LISTENERS=PLAINTEXT://127.0.0.1:9092
          - KAFKA_ZOOKEEPER_CONNECT=zookeeper:2181
          - ALLOW_PLAINTEXT_LISTENER=yes
          volumes:
          - ./Kafka:/bitnami/kafka
          networks:
          - myNetwork
          depends_on:
          - zookeeper

After creating docker-compose.yaml file. use this below command to download the latest or specific given build and run your local machine

> docker compose up -d


Now Both kafka and zookeeper are available and running. so you can verify using this command

> docker images
> 
> docker ps
>
> docker logs <container-id>

After running kafka and zookeeper on local machine, we can create specific topic using this command 

> docker exec -it <your_container_id> kafka-topics.sh --create --bootstrap-server localhost:9092 --replication-factor 1 --partitions 3 --topic my-topic

If you have created multiple topics, and you want to verify that the created topics list, so using below command we can list all the topics.

> docker exec -it <your_container_id> kafka-topics.sh --bootstrap-server localhost:9092 --list

After creating topic we just need to create Producer, Also we can create multiple producers.

> docker exec -it <your_container_id> kafka-console-producer.sh --bootstrap-server localhost:9092 --topic my-topic

Now we can send any message to above created topic. that can be consumed from below command.

> docker exec -it <your_container_id> kafka-console-consumer.sh --from-beginning --bootstrap-server localhost:9092 --topic my-topic

This command will fetch messages from the beginning. same we can create multiple consumers. 

Happy coding :)
