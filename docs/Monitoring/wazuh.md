# Installation de Wazuh SIEM


## Prérequis

- installer docker

```sudo apt install docker docker-compose```


## Installer Wazuh


On clone le repo git
```git clone https://github.com/wazuh/wazuh-docker.git -b v4.11.0```


Changer de rep vers le single node

```cd wazuh-docker/single-node```

Lancer l'installation du wazuh

```docker-compose up -d```


## Ajouter un hote sur le wazuh

suivre les étapes sur **https://10.10.240.50/app/endpoints-summary#/agents-preview/deploy**

### /!\ Attention sur windows avoir un shell en mode administrateur
### /!\ Attention sur linux installer wget et lsb-release

```sudo apt install lsb-release wget```


