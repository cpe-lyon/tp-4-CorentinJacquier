
# 👨‍💻 TP4 - Gestion des paquets

CPE Lyon - 4ETI, 3IRC & 3ICS - Année 2022/2023 Administration Système

Corentin JACQUIER ICS

## Exercice 1 - Commandes de base

On met à jour `sudo apt-get update && sudo apt-get upgrade`. 

On ajoute l'alias avec `alias maj="sudo apt-get update && sudo apt-get upgrade"`.

<img src="https://cdn.discordapp.com/attachments/1017478318934724638/1021672306977804308/unknown.png">

Pour que l'alias soit actif à chaque redémarage, on ajoute la commande dans le fichier `~/.bashrc`.

Avec la commande `head -5 dpkg.log`, on peut voir les 5 premiers packets installés.

<img src="https://media.discordapp.net/attachments/1017478318934724638/1021674909614743572/unknown.png"> 

On peut observer les derniers paquets installés explicitement avec la commande `apt list -i`. 

Et pour voir le total de paquets installés, on fait `dpkg -l | wc -l` ou `apt list -i | wc -l` (avec des lignes en plus ou moins).

<img src="https://media.discordapp.net/attachments/1017478318934724638/1021678849219633192/unknown.png">

Pour compter tous les paquets, on fait `apt list | wc -l`. 

On installe un paquet avec la commande `sudo apt install [nom_paquet]`. 

Le paquet `glances` permet d'afficher l'état des principales ressources d'un système, de sa charge et du fonctionnement des applications avec une application en caractère. 

Le paquet `tldr` permet d'afficher le résumé des pages du manuel de commandes linux.

Et la paquet `hollywood` simule les écrans de hackeurs, comme dans les film hollywodien. 

Le paquet `sudoku` permet je jouer à sudoku dans le terminal 

<img src="https://screenshots.debian.net/shrine/screenshot/16580/simage/small-eceac96c7a4b345a5bc9ac4ac284281f.png">

## Exercice 2

On trouve le paquet de la commande `ls` avec `dpkg -S /bin/ls`. On trouve donc le paquet `coreutills`. 

<img src="https://media.discordapp.net/attachments/1017478318934724638/1021686301566697533/unknown.png">

On trouve le nom du paquet d'une commande avec `dpkg -S [commande]` (-S pour --search soit rechercher). 

En une commande `sudo which -a ls | xargs dpkg -S | cut -d':' -f1`. 

  
On fait un fichier nommé «origine-commande» en tapant la commande suivante :  `nano origine-commande`  .

On reprend la commande précédente :

```bash
sudo which -a $1 | xargs dpkg -S | cut -d':' -f1
```
On donne les permissions au script avec `chmod u+x origine-commande` pour l'exécuter avec la commande `./origine-commande [commande]`. 

## Exercice 3

On utilise `dkg -l` pour afficher tous les paquets et on cherche si celui recherché est présent dans la liste :

```bash
if  dpkg -l | grep -q $1 ;  then
	echo  "INSTALLÉ"
else
	echo  "NON INSTALLÉ"
fi
```

<img src="https://cdn.discordapp.com/attachments/1017478318934724638/1021699583249756190/unknown.png">

Ou en version ligne de commande `dpkg -l [commande] 2>/dev/null | grep "^ii" && echo "INSTALLÉ" || echo "NON INSTALLÉ"  `


## Exercice 4

On liste les commandes du paquet `coreutils` avec la commande `dpkg -L coreutils`. 

<img src="https://media.discordapp.net/attachments/1017478318934724638/1021704484512083998/unknown.png">

## Exercice 5 - aptitude

On install l'interface graphique en terminal de `apt` avec `apt-get install aptitude`. 

Via l'interface on install `lynx` un navigateur utilisable dans le navigateur en mode texte et `emacs` qui est un editeur de texte extensible auto-documenté. 

## Exercice 6 - Installation d’un paquet par PPA

Après avoir fait les commandes nécéssaires à l'installation de Java. 

On ouvre le dossier `/etc/apt/sources.list.d`, et voici le contenu du noueavu fichier `linuxupridsing-ubuntu-java-jammy.list` :

<img src="https://media.discordapp.net/attachments/1017478318934724638/1021709278416994394/unknown.png">

Il contient les lien des paquets .deb.

## Exercice 7 - Installation d’un logiciel à partir du code source

On clone le github avec :

`git clone https://gitlab.com/jallbrit/cbonsai` 

Ensuite, on va dans le dossier `cd cbonsai`. Puis, avec `mdless`, on affiche le readme. 

On suit les instructions et on installe les paquets manquants. 

## Exercice 8 - Création de dépôt personnalisé

### Création d’un paquet Debian avec dpkg-deb

On fait l’arborescence demandée : 

```
scripts
└───origine-commande
│   └───DEBIAN
│   │   control
│
│	└───usr
│		└───local
│			└───bin
│			│	origine-commande
│
```
On build le paquet avec `dpkg-deb --build origine-commande`.

<img
src="https://cdn.discordapp.com/attachments/1017478318934724638/1021716452702695474/unknown.png">

### Création du dépôt personnel avec reprepro

On fait l’arborescence demandée avec `mkdir` puis `nano distributions` : 
```
repo-cpe
└───conf
│	│	distributions
│
└───packages
│   
```

On construit le paquet avec la commande `reprepro -b . includedeb origine-commande.deb`.

### Signature du dépôt avec GPG

On générère la clé avec `gpg --gen-key` et dans le fichier de config on ajoute `SignWith: yes`.

On ajoute la clé précédeament créeer dans le déport : `reprepro --ask-passphrase -b . export`.

Et pour finir, on met la clé dans celles connues par APT dans un fichier 'public.key' avec `sudo apt-key add public.key`. 
