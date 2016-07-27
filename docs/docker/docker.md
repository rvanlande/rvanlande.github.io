# Docker

## Installation sur debian

[Installation](https://docs.docker.com/engine/installation/linux/debian/)

Le répertoire contenant tous les données docker (image, conteneur,...) se trouve dans le répertoire suivant :  

    /var/lib/docker

## Commandes

### Image

    $ docker images    => Liste les images docker installées
    $ docker rmi -f <id_image>    => force la suppression d'une image par son identifiant
    $ docker build -t <nom_image>:<tag>  <path_dockerfile>  => crée une image basée sur le Dockerfile avec un nom et un tag
    $ docker rmi $(docker images -q -f dangling=true) => supprime des images temporaires non utilisées

NOTES :

* docker build : exécute toutes les commandes RUN du script dockerfile
* docker run : exécute toutes les commandes CMD du script dockerfile

### Conteneur

    $ docker ps -a    => liste tous les conteneurs
    $ docker ps -aq | xargs docker rm -v    => supprime tous les conteneurs et leur volume associé

### Volume

Liste des volumes orphelins

    $ docker volume ls -qf dangling=true

Suppression des volumes orphelins

    $ docker volume rm $(docker volume ls -qf dangling=true)

## Image vs Container

Une image peut être considéré comme immuable. Démarrer un ou plusieurs conteneurs à partir de cette image ne changera pas l'image. On peut comparer cela à une image virtuelle originale d'une machine virtuelle.  

Un conteneur démarre l'image et ajoute une couche au-dessus de celle-ci. Cette couche stocke tous les changements du conteneur (création/modification/supression de fichiers ...).


## Création d'une image

Créer un répertoire qui contiendra le fichier Dockerfile de création l'image

    $ mkdir -P createimage/ovya_go

Créer le fichier Dockerfile

    $ touch createimage/ovya_go/Dockerfile

Editer le fichier et créer le script docker permettant de construire l'image, exemple création d'une image go

    FROM debian:latest
    MAINTAINER Renaud Vanlande <rvanlande@ovya.fr>
    RUN apt-get update && apt-get install -y wget
    RUN wget https://storage.googleapis.com/golang/go1.6.2.linux-amd64.tar.gz 
    RUN tar -C /usr/local -xzf go1.6.2.linux-amd64.tar.gz
    RUN rm -f go1.6.2.linux-amd64.tar.gz
    ENV PATH=$PATH:/usr/local/go/bin

Création de l'image

    $ cd createimage/ovya_go
    $ docker build -t ovya/go:1.6.2 .

## Création et exécution d'un conteneur

    $ docker run --rm --name ovya_clog --net="host" -v /var/www/clog/:/var/www/clog ovya/clog

Cette commande créée un contenur **ovya_clog** et le démarre avec les options suivantes :  

**--rm** supprime le conteneur à la fin de son exécution  

**--name** définit un nom pour le conteneur  
**--net="host"** définit l'interface réseau du conteneur avec celle de la machine hôte  
**-v HOSTDIR:CONTAINERDIR** partage d'un répertoire entre la machine hôte et le conteneur  

    $ docker run -it <image> bin/bash

Cette commande crée et exécute un conteneur en ouvrant un terminal

    $ docker start <container_id>

Démarre un conteneur existant.

    $ docker exec -it <container_id> bin/bash

Attache un terminal à un conteneur en cours d'exécution.

## Exemple clog + pqsql clog

* **Container : ovya/clog**

> construction de l'image ovya/clog

~/dev/docker/createimage/ovya_clog/DockerFile 

    $ se placer dans le répertoire
    $ docker build -t ovya/clog .

> création du conteneur

    $ docker create --name ovya_clog --net="host" -v /var/www/clog/:/var/www/clog -v /var/log/nginx:/var/log/nginx ovya/clog

> lancement du conteneur

    $ docker start -a <conteneur_id>

* **Container : ovya/pgsql**

> construction de l'image ovya/pgsql

~/dev/docker/createimage/ovya_pgsql/

- DockerFile
- init-db-user.sh : exécuter à la création de l'image pour ajouter le user rcv


    $ se placer dans le répertoire
    $ docker build -t ovya/pgsql .

> création du conteneur

    $ docker create --name ovya-pgsql -e POSTGRES_PASSWORD=rcv -p 5432:5432 ovya/pgsql

> lancement du conteneur

    $ docker start -a <conteneur_id>

> Exécution d'une migration (installation du schéma initial)

    $ se placer dans le répertoire /var/www/clog
    $ odbm up

