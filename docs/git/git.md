* Ajouter/mettre à jour l'url d'un dépôt sur origin

        $ git remote add origin <https://new_url/depot.git>
        $ git remote set-url origin <https://new_url/depot.git>

* Supprimer une branche

        $ git branch -d <branch_name>               # supprime la branche locale
        $ git push origin --delete <branch_name>    # supprime la branche distante

* Annuler un merge/commit local

        $ git reflog => montre les références HEAD
        $ git reset --hard HEAD@{X} => repositionne le head de la branche sur le commit correspondant  
        /!\ Attention la commande annule toutes les modifications en cours de la branches stagged + commited

* Recherche des commit liés au texte

        [alias] :
        str-search = "!f() { git llg -S $1 --all --source $2; }; f"
