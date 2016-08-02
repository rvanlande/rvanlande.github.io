## Introduction

Ces pages ont pour objectif de rassembler toutes les notes/codes/commandes de différents outils informatiques.

## Installation

* Pré-requis :

[Installer mkdocs](http://www.mkdocs.org/#installation)

* Installation

        $ git clone github.com/rvanlande/rvanlande.github.io
        $ git checkout mkdocs

## Mise à jour de la documentation

* La documentation se trouve dans le répertoire `rvanlande.github.io/docs`

* sauvegarde des mises à jour dans git

        $ git commit -am 'update docs'
        $ git push origin mkdocs

## Déploiement de la documentation

    $ mkdocs gh-deploy --remote-branch master
