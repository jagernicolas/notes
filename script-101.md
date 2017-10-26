# Les scripts shell

## Table des matières.

[TOC]



## Écrire votre premier script.

##### Qu’est-ce qu’un script Shell ?

une défnition basique est de dire qu’un script est un fchier contenant une série de commandes.

##### Format de fichier d’un script :

```shell
# !/bin/bash
# ce fichier est votre premier script
echo ´Hello World’
```

La dernière ligne est une commande déjà discutée. Le symbole `#` désigne les commentaires, excepté pour la toute première ligne, qui s’appelle le "*shebang*". La ligne "*shebang*" indique au système d’exploitation qu’il ne s’agit pas d’un binaire mais d’un script et précise l’interprétateur, ici `/bin/bash`.

Ouvrez un éditeur de texte et recopier le dans un fichier hello_world.

##### Permission d’exécution :

La prochaine chose à faire est d’ajoute la permission d’exécution au fichier,

```shell
username@hostname:~$ ls -l
-rw-r--r-- 1 user user 73 Feb  4 20:08 hello_world
username@hostname:~$ chmod 755 hello_world
username@hostname:~$ ls -l hello_world 
-rwxr-xr-x 1 user user 73 Feb  4 20:08 hello_world
```

##### Emplacement du script :

maintenant que nous avons un script executable, faites,

```shell
username@hostname:~$ hello_world
bash: hello_world: command not found
```

On voit que le script est introuvable. Ceci s’explique par la localisation du script. Lorsque l’on tappe une commande, le shell cherche la commande dans un nombre limité d’emplacements. Pour savoir de quels emplacements on parle,

```shell
username@hostname:~$ echo $PATH
(…) 
```

Nous pourrions copier le script dans un de ses emplacements, à condition d´avoir les permissions nécessaires. Sinon on peut ajouter à la variable `PATH` le dossier dans lequel se trouve le script,

```shell
username@hostname:~$ export PATH=~:”$PATH” # n’oubliez pas :”$PATH” sinon PATH=~ …
```

cette modification ne sera valide que dans les nouvelles sessions Shell, ou bien on peut mettre à jour la session en (res)sourçant,

```shell
username@hostname:~$ . .bashrc
```

Sinon, il existe une troisième méthode, déterminer l’emplacement du script au moment de son invoquation, 

```shell
username@hostname:~$ ./hello_world 
Hello World
```

##### Note concernant les emplacements :

`~/bin/` est un bon endroit pour les scripts que seul l’utilisateur peut lancer. Si tous les utilisateurs doivent pouvoir lancer le script, `/usr/local/bin/` est l’emplacement traditionel. Si le script doit être reservés aux administrateurs, l’emplacement usuel est `/usr/local/sbin`.

## Les tests.

les tests permettent de créer des structures de type ‘si-alors-sinon’. Nous allons voir leurs fonctionnements dans les trois cas suivants :

- Avec des nombres entiers

- Avec des chaines de caractères 
- Avec des fichiers

##### Tester qu’une variable est définie :

```shell
# !/usr/bin/bash
VAR1=1

if [ -z “$VAR1” ]; then
	echo "VAR1 n’est pas initialisée" >&2
    exit 1 # exit [1,255] indique failure
fi
 
if [ -z “$VAR2” ]; then
    echo "VAR2 n’est pas initialisée" >&2
    exit 1
fi

exit 0 # indique success
```

##### Tester des nombres entiers :

```shell
# !/usr/bin/bash

ENTIER_1=-3
ENTIER_2=3
ENTIER_3=3

if [ $ENTIER_1 -eq $ENTIER_2 ]; then
    echo "etrange" >&2
    exit 1
fi

if [ $ENTIER_2 -eq $ENTIER_3 ]; then
    echo "normal"
elif [ $ENTIER_2 -gt $ENTIER_1 ]; then        # remarquez le elif (else if)
    ENTIER_1=6
    echo 'ENTIER_1 vaut '$ENTIER_1' maintenant.'
fi

if [ $ENTIER_3 -gt $ENTIER_1 ]; then
    echo 'ENTIER_3 > ENTIER_1'
else
    echo 'ENTIER_1 > ENTIER_3'
fi

exit 0 # indique success
```

| Expression | Vraie si... |
| ---------- | ----------- |
| A -eq B    | A = B       |
| A -ne B    | A ≠ B       |
| A -le B    | A ≤ B       |
| A -lt B    | A < B       |
| A -ge B    | A ≥ B       |
| A -gt B    | A > B       |

##### Tester des nombres décimaux :

le shell n’est pas capable d’efectuer des opérations sur les nombres décimaux. Pour tester des nombres décimaux, il faut utiliser un programme `bc` (basic calculator).

```shell
# !/usr/bin/bash

NOMBRE=1.64

if (( $(bc <<< "$NOMBRE == 0.2") ));then
	echo "$NOMBRE est egal a 0.2"
elif (( $(bc <<< "$NOMBRE < 0.2") ));then
	echo "$NOMBRE est inferieur a 0.2"
else
	echo "$NOMBRE est superieur a 0.2"
fi
```

##### Tester des chaînes de caractères  :

```shell
# !/usr/bin/bash
S1=" "
S2="B"
S3="A"

if [ "$S1" ]; then
    S1=”A”
fi

if [ "$S2" == "$S3" ]; then
    echo "normal"
fi

if [ "$S2" \> "$S1" ]; then # notez le \
    echo 'S1 est trié avant S2'
    S2="C"
fi

if [ "$S3" = "A" ]; then
    echo 'S3 vaut A'
else
    echo 'S3 vaut'$S3
fi
exit 0
```

| Expression | Vraie si...                   |
| ---------- | ----------------------------- |
| S1         | S1 est vide.                  |
| -n S1      | La longueur de S1 > 0         |
| -z S1      | La longueur de S1 = 0         |
| S1 != S2   | S1 ≠ S2                       |
| S1 \\> S2  | Trie S1 et S2. S1 est second  |
| S1 \\< S2  | Trie S1 et S2. S1 est premier |

##### Tester des fichiers :

```shell
# !/bin/bash
FILE=~/.bashrc

if [ -e "$FILE" ]; then
	if [ -f "$FILE" ]; then
 		echo "$FILE est un fichier."
 	fi
 	if [ -d "$FILE" ]; then
 		echo "$FILE est un dossier."
 	fi
 	if [ -r "$FILE" ]; then
 		echo "$FILE est lisible."
 	fi
 	if [ -w "$FILE" ]; then
 		echo "$FILE est éditable."
 	fi
 	if [ -x "$FILE" ]; then
 		echo "$FILE est exécutable."
 	fi
else
	echo "$FILE n’existe pas"
	exit 1
fi
exit
```

| Expression | Vraie si...                              |
| ---------- | ---------------------------------------- |
| -L file    | Lien symbolique.                         |
| -o file    | Existe et appartient à l’utilisateur.    |
| -r file    | Existe et est lisible.                   |
| -s file    | Existe et a une taille > 0.              |
| -S file    | Existe et est un socket réseau.          |
| -t fd      | ‘fd’ est un descripteur de fichier dirigé vers ou provenant d’un terminal. Util pour savoir si stdin/stout/stderr est redirigé. |
| -w file    | Existe et est éditable.                  |
| -x file    | Existe et est exécutable.                |

##### Utilisation de case :

Parfois la structure d’un script peut contenir une succession de ‘if-else-if’. Il existe une alternative rendant l’écriture du script plus lisible. Cette alternative est ‘case’,

```shell
# !/bin/bash

couleur="rouge"

case $couleur in
   "rouge" ) echo "comme une tomate";;
   "bleu"  ) echo "comme le ciel";;
   "jaune" ) echo "comme la banane";;
   "vert"  ) echo "comme l'herbe";;
esac
```

##### La boucle while :

```shell
while [[ condition ]]; do
	#Operations
done
```

Elle effectue une série d’opérations tant que la condition est vraie.

##### La boucle until :

```shell
Until [[ condition ]]; do
	#Operations
done
```

Elle effectue une série d’opérations jusqu’à ce que la condition soit vraie.

##### Stoper une boucle :

- `break`, stope la boucle.
- `continue`, passe à l’itération suivante.

Exemple d’utilisation :

```shell
# !/usr/bin/bash
VAR1=1

echo "boucle while :"
while [[ $VAR1 -le 50 ]];do
	echo $VAR1
    VAR1=$((VAR1 + 1))
    if [ $VAR1 -eq 5 ]; then
    	echo "on stop la boucle"
        break; # stop la boucle
    fi
    if [ $VAR1 -ne 4 ]; then
    	continue # passe à l'itération suivante
    fi
	echo "bingo, VAR1 vaut quatre"
done

echo "boucle until :"
until [[ $VAR1 -eq 1 ]];do
	echo $VAR1
    VAR1=$((VAR1 - 1))
done
```

## Functions et paramètres :

Un paramètre peut être passé, à un script, depuis la ligne de commande, de la même manière que l’on procède avec les arguments aux fonctions que nous avons déjà vues,

```shell
 username@hostname:~$ ./un_script parametre1 parametre2
```

Les scripts identifient les paramètres par `$1`,`$2`, … 

`$0` est particulier puisqu’il désigne le nom du script. La variable `$#` définit le nombre de paramètres.


Les fonctions s’écrivent de la façon suivante,

```shell
nom_fonction () {
	# operations
}
```

##### Exemple de script avec fonction et paramètres :

```shell
# !/usr/bin/bash

Bonjour () {
  if [ -z "$1" ];then
      echo "il manque le nom"
  else
      echo "bonjour "$1
  fi
}

echo "nom du script "$0", nombre de parametres :" $#

for var in "$@";do
  echo "la longueur de l'argument "$var" est : ${#var}"
done

Bonjour jo
Bonjour

if [ $# -gt 0 ];then
    Bonjour $1
fi
```



