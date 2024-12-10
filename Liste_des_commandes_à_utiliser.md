
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
