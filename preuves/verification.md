# Verification du projet

Pour chaque cas, coller la commande, sa sortie, et votre explication.

1. Jeu initial de quatre articles.

PS C:\Users\HP\Downloads\r5.09_tp_stock-master\r5.09_tp_stock-master> docker run --rm `
>>   --mount "type=bind,source=$($PWD.Path)\entrees,target=/entrees" `
>>   --mount "type=bind,source=$($PWD.Path)\resultats,target=/resultats" `
>>   analyse-stock:v1
References : 4
Unites : 16
Valeur totale : 670.00 EUR
References en stock faible : 3
References en rupture : 1

Dans l'exemple initial il y a bien quatres articles ( Clavier, Souris, Ecran et Cable ),
Il y a 16 exemplaires totaux des articles ( 10 + 4 + 2 + 0 = 16 ),
Le prix total de tous les articles est de 670 euros ( 10 * 25 + 4 * 15 + 2 * 180 + 0 * 8 = 673 ),
Il t a trois articles en stocke faible ( j'ai décider de prendre 5 ou moins comme était faibles donc seul le clavier qui est à 10 exemplaires n'est pas en stock faible ),
Et un article est en rupture de stock ( les cable qui sont à 0 ).


2. CSV modifie sans reconstruction.

Après avoir passé le nombre de cable à 6 dans le csv :
PS C:\Users\HP\Downloads\r5.09_tp_stock-master\r5.09_tp_stock-master> docker run --rm `
>>   --mount "type=bind,source=$($PWD.Path)\entrees,target=/entrees" `
>>   --mount "type=bind,source=$($PWD.Path)\resultats,target=/resultats" `
>>   analyse-stock:v1
References : 4
Unites : 22
Valeur totale : 718.00 EUR
References en stock faible : 2
References en rupture : 0

Le conteneur à lue les nouvelle données, le résultat est donc différent.


3. Tentative d'ecriture dans entrees en lecture seule.

PS C:\Users\HP\Downloads\r5.09_tp_stock-master\r5.09_tp_stock-master> docker run --rm `
>>   --mount "type=bind,source=$($PWD.Path)\entrees,target=/entrees,readonly" `
>>   analyse-stock:v1 `
>>   python3 -c "from pathlib import Path; Path('/entrees/test.txt').write_text('test')"
Traceback (most recent call last):
  File "<string>", line 1, in <module>
  File "/usr/lib/python3.12/pathlib.py", line 1049, in write_text
    with self.open(mode='w', encoding=encoding, errors=errors, newline=newline) as f:
         ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/usr/lib/python3.12/pathlib.py", line 1015, in open
    return io.open(self, mode, buffering, encoding, errors, newline)
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
OSError: [Errno 30] Read-only file system: '/entrees/test.txt'

En lecture seul on ne peut pas écrire dans un fichier.


5. Entree absente, vide ou invalide.

PS C:\Users\HP\Downloads\r5.09_tp_stock-master\r5.09_tp_stock-master> docker run --rm --mount "type=bind,source=$($PWD.Path)\resultats,target=/resultats" analyse-stock:v1
Erreur : [Errno 2] No such file or directory: '/entrees/stock.csv'

Il manque le fichier d'entrée donc la commande renvoi une erreur.