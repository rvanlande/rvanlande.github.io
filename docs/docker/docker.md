# Docker Engine

[Docker](https://www.docker.com/)  
[Docker Overview](https://docs.docker.com/engine/understanding-docker/)  
[Docker Commands](https://docs.docker.com/engine/reference/commandline/)  

## Installation sur debian

[Installation](https://docs.docker.com/engine/installation/linux/debian/)

Le répertoire contenant tous les données docker (image, conteneur,...) se trouve dans le répertoire suivant :  

    /var/lib/docker

## Quelques commandes

### Images

    $ docker images    => Liste les images docker installées
    $ docker rmi -f <id_image>    => force la suppression d'une image par son identifiant
    $ docker build -t <nom_image>:<tag>  <path_dockerfile>  => crée une image basée sur le Dockerfile avec un nom et un tag
    $ docker rmi $(docker images -q -f dangling=true) => supprime des images temporaires non utilisées

NOTES :

* docker build : exécute toutes les commandes RUN du script dockerfile
* docker run : exécute toutes les commandes CMD du script dockerfile

### Conteneur

* **création et exécution d'un conteneur** :

        $ docker run --rm --name <mon_conteneur> --net="host" -v <absolute_path_host>:<absolute_path_conteneur> <image_name>

Cette commande créée un conteneur 'mon_conteneur' basé sur l'image 'image_name' et le démarre avec les options suivantes :  

--rm : supprime le conteneur à la fin de son exécution  
--name : définit un nom pour le conteneur  
--net="host" : définit l'interface réseau du conteneur avec celle de la machine hôte  
-v HOSTDIR:CONTAINERDIR : partage d'un répertoire entre la machine hôte et le conteneur  

* **Création et exécution d'un conteneur en ouvrant un terminal**

        $ docker run -it <image> bin/bash

* **Démarrer un conteneur existant** : 

        $ docker start <container_id>

* **Attacher un terminal à un conteneur en cours d'exécution** : 

        $ docker exec -it <container_id> bin/bash

* **Lister tous les conteneurs actifs et inactifs** :

        $ docker ps
        $ docker ps -a

* **Supprimer tous les conteneurs et leur volume associé** : 

        $ docker ps -aq | xargs docker rm -v

NOTES :

* **Image et conteneur** :

Une image peut être considéré comme immuable. Démarrer un ou plusieurs conteneurs à partir de cette image ne changera pas l'image. On peut comparer cela à une image virtuelle originale d'une machine virtuelle.  

Un conteneur démarre l'image et ajoute une couche au-dessus de celle-ci. Cette couche stocke tous les changements du conteneur (création/modification/supression de fichiers ...).

### Volume

Liste des volumes orphelins

    $ docker volume ls -qf dangling=true

Suppression des volumes orphelins

    $ docker volume rm $(docker volume ls -qf dangling=true)
