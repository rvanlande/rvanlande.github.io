* Annuler un merge/commit local

        $ git reflog => montre les références HEAD
        $ git reset --hard HEAD@{X} => repositionne le head de la branche sur le commit correspondant  
        /!\ Attention la commande annule toutes les modifications en cours de la branches stagged + commited

* Recherche des commit liés au texte

        [alias] :
        str-search = "!f() { git llg -S $1 --all --source $2; }; f"
