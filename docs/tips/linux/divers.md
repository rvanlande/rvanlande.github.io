## Divers

* recherche dans les fichiers du répertoire courant et de ses sous répertoire

```
$ grep -rn . -e 'chaine recherchée'
```

* zipper le contenu d'un répertoire

```
$ zip -r nom_du_fichier.zip *
```

## SSH

* démarrer l'agent ssh

```
$ eval `ssh-agent -s`
```

* ajouter une clé ssh à l'agent

```
$ ssh-add ~/.ssh/id_rsa
```

## SSL : création clé privée et certificat

* clef privée du serveur

```
$ cd /etc/ssl
$ sudo openssl genrsa -out server.key 2048
```

* Demande de signature du certificat

```
$ openssl req -text -noout -in server.csr
```

* Signature du certificat : auto-signé, valide un an

```
$ openssl x509 -req -days 365 -in server.csr -signkey server.key -out server.crt
```

## Réseaux

* voir les ports ouverts :

        $ sudo netstat -tulpn

## Systemd

* liste les services

        $ systemctl

* démarre un service

        $ systemctl start exim4

* arrête un service

        $ systemctl stop exim4

* affiche le statut d'un service

        $ systemctl status exim4

* active le service au démarrage

        $ systemctl enable exim4

* désactive le service au démarrage

        $ systemctl disable exim4

## gpg

* génération des clés

        $ gpg --gen-key

* afficher les clés

        $ gpg --list-keys
        $ gpg --list-secret-keys

* importer une clé

        $ gpg --import <key-file>

* exporter une clé publique

        $ gpg -a --export <id-cle>

* exporter une clé privée

        $ gpg -a --export-secret-key <id-cle>

* chiffrer un message

        $ gpg -a -r <email> -e <file>

* déchiffrer un message

        $ gpg -d <file>





