# lab_mongo_replicaSet


## les étapes à suivre :
on lance docker et dans notre dossier on met :

on ouvre CMD et on met :
```
ipconfig
```
on cherche l'adresse IPv4 de la carte Ethernet vEthernet (WSL).

exemple du résultat :
```
Carte Ethernet vEthernet (WSL) :
   Adresse IPv4. . . . . . . . . . . . . .: 172.21.96.1
   ```
Ensuite on modifie le fichier "rsconf.json" : (adresse ip)
```
rsconf = {
   _id : "rsmongo",
   members: [
       {
           "_id": 0,
           "host": "172.21.96.1:27017",
           "priority": 4
       },
       {
           "_id": 1,
           "host": "172.21.96.1:27018",
           "priority": 2
       },
       {
           "_id": 2,
           "host": "172.21.96.1:27019",
           "priority": 1
       },
        {
           "_id": 3,
           "host": "172.21.96.1:27020",
           "priority": 0,
           "arbiterOnly" : true

       }
   ]
}
```
Et aprés :
```
docker-compose up -d
docker-compose exec mongodb1 mongo
```
Copier le contenu de "rsconf.json" et l'exécuter :

Aprés : 
```
rs.initiate(rsconf)
```

Quitter et exécuter : 
```
docker cp dblp.json mongodb1:/dblp.json

docker-compose exec mongodb1 mongo

> use dblp;

> db.publis.count();
```

Et la même chose avec mongodb2 et 3.

```
docker-compose exec mongodb2 mongo
rs.secondaryOk();
use dblp;
db.publis.count();
```

```
docker-compose exec mongodb3 mongo
rs.secondaryOk();
use dblp;
db.publis.count();
```

Dans la console de mongodb1 on ajoute un document :

```
db.publis.save({"test":1});
```

Et dans la console de mongodb2 et 3 on vérifier son existence :
```
rs.secondaryOk();
use dblp;
db.publis.count();
```

Et avec Compass :

Entrer :
```
mongodb://172.21.96.1:27017/?replicaSet=rsmongo
```

Et arrêter le conteneur Primary (mongodb1).

Exécuter sur compass  quelques  requêtes.
Et tout marche bien.




