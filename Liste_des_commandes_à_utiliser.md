
# Liste pour les commandes à utiliser

  Ici vous trouverez toutes les commandes utilisées pendant le TP, vous pouvez les copier et ensuite les coller dans le terminale qu'il faut. Pour plus d'explication consulté le fichier Guide d'installation.

## 1. INSTALLATION DES PREREQUIS  

- ### Installation de WSL sous Windows

  - #### Activer WSL
      ```bash
      dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
      ```

  - #### Mettre à jour WSL
      ```bash
      wsl --update
      ```

  - #### Activer la plateforme de machine virtuelle (pour WSL 2)
      ```bash
      dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
      ```

  - #### Définir WSL 2 comme version par défaut
      ```bash
      wsl --set-default-version 2
      ```

- ### Installation de Ubuntu ou autre distribution Linux sous WSL
  - #### Voir les distributions de linux qui peuvent être installer 
      ```bash
      wsl --list -–online
      ```
  
  - #### Installer Ubuntu 22.04 LTS
      ```bash
      wsl --install -d Ubuntu-22.04
      ```

  - #### Vérifier l'installation d'Ubuntu
      ```bash
      wsl --list -–verbose
      ```

- ### Installation de Java JDK (version 8 ou supérieure)
  - #### Mettre à jour la liste des paquets
      ```bash
      sudo apt update
      ```

  - #### Installer Java JDK
      ```bash
      sudo apt install openjdk-8-jdk
      ```

  - #### Vérifier la version de Java
      ```bash
      java -version
      ```

## 2. INSTALLATION DE HADOOP

- ### Téléchargement d'Hadoop
    ```bash
    wget https://downloads.apache.org/hadoop/common/hadoop-3.4.1/hadoop-3.4.1.tar.gz
    ```
  - #### Extraire les fichiers de l'archive hadoop-3.4.1.tar.gz dans un dossier nommé hadoop-3.4.1
      ```bash
      tar -xzvf hadoop-3.4.1.tar.gz
      ```

  - #### Déplacer le fichier extrait hadoop-3.4.1 vers le repertoire local
      ```bash
      sudo mv hadoop-3.4.1 /usr/local/hadoop
      ```

  - #### Modifier le fichier log4j.properties
      ```bash
      nano $HADOOP_HOME/etc/hadoop/log4j.properties
      ```
- ###  Configurer les variables d'environnement
  - #### Ouvrir  ~/.bashrc
      ```bash
      nano ~/.bashrc
      ```
  - #### Ajouter les lignes suivantes :
      ```bash
      export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64 
      export HADOOP_HOME=/usr/local/hadoop 
      export PATH=$PATH:$HADOOP_HOME/bin 
      export HADOOP_SBIN=$HADOOP_HOME/sbin 
      export PATH=$PATH:$HADOOP_SBIN 
      export HADOOP_MAPRED_HOME=$HADOOP_HOME 
      export HADOOP_CONF_DIR=$HADOOP_HOME/etc/hadoop 
      export HADOOP_COMMON_LIB_NATIVE_DIR=$HADOOP_HOME/lib/native 
      export HADOOP_OPTS="-Djava.library.path=$HADOOP_HOME/lib/native" 
      export HADOOP_STREAMING=$HADOOP_HOME/share/hadoop/tools/lib/hadoop-streaming-3.4.1.jar 
      export HADOOP_LOG_DIR=$HADOOP_HOME/logs 
      export PDSH_RCMD_TYPE=ssh   
      ```

  - #### Appliquer les modifications du fichier ~/.bashrc
      ```bash
      source ~/.bashrc
      ```

  - #### Ouvrir le fichier 'log4j.properties' :
      ```bash
      nano $HADOOP_HOME/etc/hadoop/log4j.properties 
      ```

  - #### Et ajouter :
      ```bash
      log4j.rootLogger=INFO, console
      log4j.appender.console=org.apache.log4j.ConsoleAppender
      log4j.appender.console.layout=org.apache.log4j.PatternLayout
      log4j.appender.console.layout.ConversionPattern=%d{yyyy-MM-dd HH:mm:ss} %p %c{1} - %m%n
      ```
- ### Configuration de Hadoop
  - #### Créer le fichier hadoop-env.sh
      ```bash
      nano $HADOOP_HOME/etc/hadoop/hadoop-env.sh
      ```
  - #### Ajouter la ligne suivante :
      ```bash
      export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64
      ```

  - #### Éditer le fichier core-site.xml
      ```bash
      sudo nano /usr/local/hadoop/etc/hadoop/core-site.xml
      ```
  - #### Ajoutez le code suivant :
      ```bash
      <configuration>
          <property>
              <name>fs.defaultFS</name>
              <value>hdfs://localhost:9000</value>
          </property>
      </configuration>
      ```
  - ####  Éditer le fichier hdfs-site.xml
      ```bash
      sudo nano /usr/local/hadoop/etc/hadoop/hdfs-site.xml
      ```
  - #### Ajoutez le code suivant :
      ```bash
      <configuration>
          <property>
              <name>dfs.replication</name>
              <value>1</value>
          </property>
      </configuration>
      ```

- ### Formatage du système de fichiers HDFS
    ```bash
    hdfs namenode -format
    ```

- ### Vérifier l'état de SSH
  - #### Vérifier si le client SSH est disponible :
      ```bash
      ssh -V
      ```

  - #### Vérifiez si le serveur SSH est en cours d'exécution :
      ```bash
      sudo service ssh status  
      ```

  - #### Démarrer le seveur SSH : 
      ```bash
      sudo service ssh start
      ```

  - #### Tester une connexion SSH locale 
      ```bash
      ssh localhost
      ```

- ### Installer et configurer SSH si nécessaire
  - #### Installer le serveur SSH :
      ```bash
      sudo apt update
      ```
    Ensuite exécutez la commande suivante :
      ```bash
      sudo apt install openssh-server -y
      ```

  - #### Démarrer le serveur SSH :
      ```bash
      sudo service ssh start  
      ```

  - #### Configurer SSH pour démarrer automatiquement :
      ```bash
      sudo systemctl enable ssh
      ```

  - #### Vérifiez si SSH écoute sur le port 22 :
      ```bash
      netstat -tlnp | grep :22
      ```

- ### Configurer l'authentification SSH sans mot de passe
  - #### Générer une clé SSH (si elle n'existe pas déjà) :
      ```bash
      ssh-keygen -t rsa -P ""
      ```

  - #### Ajouter la clé publique à la liste des clés autorisées :
      ```bash
      cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
      ```
    Ensuite exécutez la commande suivante :
      ```bash
      chmod 600 ~/.ssh/authorized_keys
      ```
    
  - #### Tester la connexion SSH sans mot de passe :
      ```bash
      ssh localhost
      ```

- ### Démarrer Hadoop :
    ```bash
    start-dfs.sh
    ```

- ### Ouvrir un navigateur et aller sur :
    ```bash
    http://localhost:9870
    ```

- ### Créer un répertoire  :
    ```bash
    hdfs dfs -mkdir /test 
    ```

- ### Lister le contenu du répertoire racine dans HDFS pour vérifier que le répertoire a bien été créé
    ```bash
    hdfs dfs -ls /
    ```

## 3- INSTALLATION DE SPARK

- ### Téléchargement de Spark
    ```bash
    wget https://downloads.apache.org/spark/spark-3.5.3/spark-3.5.3-bin-hadoop3.tgz
    ```
  - #### Extraire les fichiers de l'archive spark-3.5.3-bin-hadoop3.tgz
      ```bash
      tar xvf spark-3.5.3-bin-hadoop3.tgz
      ```

  - #### Déplacer le fichier extrait spark-3.5.3-bin-hadoop3 vers le repertoire local
      ```bash
      sudo mv spark-3.5.3-bin-hadoop3 /usr/local/spark
      ```

- ###  Configurer les variables d'environnement
  - #### Ouvrir  ~/.bashrc
      ```bash
      nano ~/.bashrc
      ```
  - #### Ajouter les lignes suivantes :
      ```bash
      export SPARK_HOME=/usr/local/spark 
      export PATH=$PATH:$SPARK_HOME/bin
      ```

  - #### Appliquer les modifications du fichier ~/.bashrc
      ```bash
      source ~/.bashrc
      ```

- ### Test d'un job simple : Créer un fichier Python (test_spark.py)
  - #### Créer un répertoire nommé spark_projects dans votre répertoire personnel 
      ```bash
      mkdir ~/spark_projects 
      ```
  - #### Se placer dans le répertoire spark_project
      ```bash
      cd ~/spark_projects/ 
      ```
  - #### Création du fichier python nommer test_spark.py
      ```bash
      touch test_spark.py 
      ```
  - #### Ouverture du fichier python test_spark.py
      ```bash
      nano test_spark.py
      ```
  - #### Ajout des lignes de codes suivantes :
      ```bash
      from pyspark import SparkContext 
      sc = SparkContext("local", "Simple App") 
      data = [1, 2, 3, 4, 5] 
      rdd = sc.parallelize(data) 
      total = rdd.reduce(lambda a, b: a + b) 
      print("Total :", total) 
      sc.stop()
      ```

  - #### Création du fichier python nommer test2_spark.py
      ```bash
      touch test2_spark.py 
      ```
  - #### Ouverture du fichier python test2_spark.py
      ```bash
      nano test2_spark.py
      ```
  - #### Ajout des lignes de codes suivantes :
      ```bash
      from pyspark.sql import SparkSession 
      # Créer une session Spark 
      spark = SparkSession.builder \ 
      .appName("Test Spark") \ 
      .getOrCreate() 
      # Créer un DataFrame 
      data = [("BAKAYOKO", 1500000), ("YAO", 278000), ("TIEKOURA", 150000)] 
      df = spark.createDataFrame(data, ["Nom", "Salaire"]) 
      # Afficher le DataFrame 
      df.show() 
      # Arrêter la session Spark 
      spark.stop()
      ```

- ### Exécuter le job Spark :
    ```bash
    spark-submit test_spark.py 
    ```

   - #### En cas d'erreur d'adresse éditer fichier '~/.bashrc' ajouter le script ci-dessous puis charger avec la commande 'source ~/.bashrc' 
      ```bash
      export SPARK_LOCAL_IP=10.255.255.254 # Remplacez cette adresse par une adresse IP de votre réseau 
      ```
