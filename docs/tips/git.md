### Dépôt

* Ajouter/mettre à jour l'url d'un dépôt sur origin

        $ git remote add origin <https://new_url/depot.git>
        $ git remote set-url origin <https://new_url/depot.git>
        
### Branches        

* Créer et de placer sur une branche

        $ git checkout -b <branch_name>

* Supprimer une branche

        $ git branch -d <branch_name>               # supprime la branche locale
        $ git push origin --delete <branch_name>    # supprime la branche distante

### Commits

* Annuler un merge/commit local

        $ git reflog => montre les références HEAD
        $ git reset --hard HEAD@{X} => repositionne le head de la branche sur le commit correspondant  
        /!\ Attention la commande annule toutes les modifications en cours de la branches stagged + commited


### Log

* Afficher l'historique d'un fichier avec les modifications
 
        $ git log -p path/to/file
 

* Recherche des commit liés au texte

        [alias] :
        str-search = "!f() { git llg -S $1 --all --source $2; }; f"

### Stash

* sauvegarder le travail en cours de la branche en cours

        $ git stash save

* rétablir le travail suavegardé sur la branche en cours

        $ git stash pop

