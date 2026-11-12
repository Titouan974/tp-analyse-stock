# Analyse de stock avec Docker

## Auteurs
VIGOUROUX Titouan
BELLAICHE-LARUE Elliot

## Construction

docker build -t analyse-stock:v1
Cette commande permet de crée l'image qui a pour nom analyse-stock et comme tag v1.


## Execution avec montages

docker run --rm `
  --mount "type=bind,source=$($PWD.Path)\entrees,target=/entrees" `
  --mount "type=bind,source=$($PWD.Path)\resultats,target=/resultats" `
  analyse-stock:v1
Le premier chemin ( --mount "type=bind,source=$($PWD.Path)\entrees,target=/entrees" ) est le chemin d'entrée qui récupère le fichier stock.csv .
Le second chemin ( --mount "type=bind,source=$($PWD.Path)\resultats,target=/resultats" ) est le chemin de sortie qui est l'endroit ou l'on met le fichier de résultat.


## Tests et resultats
[`preuves/verification.md`](preuves/verification.md)

## Reproduction apres git clone
git clone <...>
cd ...
New-Item -ItemType Directory -Force entrees, resultats
docker build -t analyse-stock:v1
docker run --rm `
  --mount "type=bind,source=$($PWD.Path)\entrees,target=/entrees" `
  --mount "type=bind,source=$($PWD.Path)\resultats,target=/resultats" `
  analyse-stock:v1
Get-Content .\resultats\bilan-stock.txt