
# Liste pour les commandes à utiliser

## 1. Installation des prérequis

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

## 2. Installation de Hadoop

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
    export HADOOP_OPTS="Djava.library.path=$HADOOP_HOME/lib/native" 
    export HADOOP_STREAMING=$HADOOP_HOME/share/hadoop/tools/lib/hadoop-streaming-3.4.1.jar 
    export HADOOP_LOG_DIR=$HADOOP_HOME/logs 
    export PDSH_RCMD_TYPE=ssh   
    ```

  - #### Appliquer les modifications du fichier ~/.bashrc
    ```bash
    source ~/.bashrc
    ```
    
