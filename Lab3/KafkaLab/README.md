Lab3 Kafka - Projet Maven

Contenu créé:
- `pom.xml` : configuration Maven avec dependances Kafka, kafka-streams et assembly plugin
- `src/main/java/edu/ensias/kafka/EventProducer.java`
- `src/main/java/edu/ensias/kafka/EventConsumer.java`
- `src/main/java/edu/ensias/kafka/WordCountApp.java`

Instructions rapides :

1) Compiler et créer les jars (sur ta machine ou dans le conteneur hadoop-master si Maven est installé) :

```bash
cd /shared_volume/ # ou dans d:/EZ/Ensias/BigData Alami/Lab3/KafkaLab
mvn clean package
```

2) Copier le JAR généré vers le dossier partagé `hadoop_project/kafka` (si tu as buildé sur la machine hôte)
Le jar avec dépendances sera dans `target/*-jar-with-dependencies.jar`.

3) Dans le conteneur `hadoop-master`, créer les topics nécessaires :

```bash
kafka-topics.sh --create --bootstrap-server localhost:9092 --replication-factor 1 --partitions 1 --topic Hello-Kafka
kafka-topics.sh --create --bootstrap-server localhost:9092 --replication-factor 1 --partitions 1 --topic input-topic
kafka-topics.sh --create --bootstrap-server localhost:9092 --replication-factor 1 --partitions 1 --topic output-topic
```

4) Lancer le producer Java (exemple) :

```bash
java -jar /shared_volume/kafka/*-jar-with-dependencies.jar Hello-Kafka
```

5) Lancer le consumer Java (exemple) :

```bash
java -cp /shared_volume/kafka/*-jar-with-dependencies.jar edu.ensias.kafka.EventConsumer Hello-Kafka
```

6) Lancer l'application WordCount (exemple) :

```bash
java -jar /shared_volume/kafka/*-jar-with-dependencies.jar input-topic output-topic
```

Notes :
- Si tu utilises le même jar pour plusieurs classes, indique la classe principale via `-cp` et le nom complet de la classe (ex: `edu.ensias.kafka.EventProducer`).
- Après création des fichiers et compilation, exécute les commandes `kafka-console-producer.sh` et `kafka-console-consumer.sh` pour tester rapidement.

Résumé simple de ce que j'ai fait (sans rentrer dans les détails du code) :
- J'ai créé un projet Maven contenant un producer Kafka, un consumer Kafka et une application Kafka Streams (word count).
- J'ai ajouté un `pom.xml` prêt à produire un JAR avec dépendances.
- Je t'ai laissé dans le README les commandes pour compiler et exécuter les applications depuis le conteneur `hadoop-master`.

Si tu veux, je peux :
- T'expliquer comment build et exécuter les trois jars directement dans le conteneur `hadoop-master`.
- Ou modifier le `pom.xml` pour produire un JAR par application (producer/consumer/streams) automatiquement.
