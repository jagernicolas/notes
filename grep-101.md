# grep

## table des matières

[TOC]



## Introduction à grep

Nous allons nous interesser à des outils pour manipuler les chaînes de caractères et après avoir introduit ‘les expressions régulières’.

##### Qu’est-ce donc ?

- Une réponse courte est de dire qu’il s’agit de notations symboliques utilisées pour identifier des motifs dans du texte. Sous certains aspects, les expressions régulières sont semblables aux ‘jokers’.
- ‘grep’ recherche dans du text les occurences correspondantes à une expression régulière et affiche le résultat.

Nous avons déjà fait un usage basique, en voici un autre,

```shell
username@hostname:~$ ls
Desktop Documents Downloads Music Pictures Public Templates Videos
username@hostname:~$ ls | grep es
Desktop
Pictures
Templates
```

‘grep’ a retourné tous les éléments contenant ‘es’, 

##### Options de ‘grep’ :

| Option                      | Description                              |
| --------------------------- | ---------------------------------------- |
| -i ou --ignore-case         | Ignore la casse.                         |
| -v ou --invert-match        | Inverse la correspondance.               |
| -c ou --count               | Affiche le nombre de correspondances, au lieu des correspondances. |
| -l ou -files-with-matches   | Affiche le nom des fichiers qui ont des correspondances, au lieu des correspondances. |
| -L ou --files-without-match | Affiche le nom des fichiers qui n’ont aucune correspondance. |
| -n ou --line-number         | Ajoute le numéro de la ligne à laquelle la correspondance a été trouvée. |
| -h ou -no-filename          | Lors des recherches dans plusieurs fichiers, n’affiche pas le nom des fichiers |

Créons d’abord des fichiers que nous utiliserons par la suite, 

```shell
username@hostname:~$ ls /bin > dirlist-bin.txt
username@hostname:~$ ls /usr/bin > dirlist-usr-bin.txt
username@hostname:~$ ls /sbin > dirlist-sbin.txt
username@hostname:~$ ls /usr/sbin > dirlist-usr-sbin.txt
username@hostname:~$ ls dirlist*
dirlist-bin.txt  dirlist-sbin.txt  dirlist-usr-bin.txt  dirlist-usr-sbin.txt
```

Effectuons une simple recherche,

```shell
username@hostname:~$ grep bzip dirlist-*
dirlist-bin.txt:bzip2
dirlist-bin.txt:bzip2recover
dirlist-sbin.txt:bzip2
dirlist-sbin.txt:bzip2recover
dirlist-usr-bin.txt:bzip2
dirlist-usr-bin.txt:bzip2recover
dirlist-usr-sbin.txt:bzip2
dirlist-usr-sbin.txt:bzip2recover
username@hostname:~$ grep -l bzip dirlist-*
dirlist-bin.txt
dirlist-sbin.txt
dirlist-usr-bin.txt
dirlist-usr-sbin.txt
username@hostname:~$ grep -L bzip dirlist-*
```

##### Métacaractères et litérales :

Il existe un autre type de  caractères, només métacaractères, utilisés pour spécifier des motifs plus complexes :

```
^ $ . [ ] { } - ? * + ( ) | \
```

##### Le n’importe quel caractère, ‘.’ :

Si un ‘.’ se retrouve inclut dans une expression régulière, n’importe quel caractère à la position qu’il occupe sera considéré validant la correspondance.

```shell
username@hostname:~$ echo "toto.bz.p" >> dirlist-bin.txt
username@hostname:~$ grep  'bz.p'  dirlist-*
dirlist-bin.txt:bzip2
dirlist-bin.txt:bzip2recover
dirlist-bin.txt:toto.bz.p
dirlist-sbin.txt:bzip2
dirlist-sbin.txt:bzip2recover
dirlist-usr-bin.txt:bzip2
dirlist-usr-bin.txt:bzip2recover
dirlist-usr-sbin.txt:bzip2
dirlist-usr-sbin.txt:bzip2recover
```

Remarquez que si une ligne contient ‘bz.p’ elle génère une correspondance.

##### Les ancres, ‘^’ and ‘$’ :

les ancres, permettent de limiter la correspondance au débuts de lignes, avec ‘^’, et aux fins de lignes, avec ‘$’,

```shell
username@hostname:~$ echo "zip" >> dirlist-bin.txt
username@hostname:~$ grep  '^zip$'  dirlist-bin.txt
zip
username@hostname:~$ grep  '^zip'  dirlist-bin.txt
zipgrep
zipinfo
zip
username@hostname:~$ grep  'zip$'  dirlist-bin.txt
funzip
gpg-zip
gunzip
gzip
hunzip
hzip
preunzip
prezip
unzip
zip
```

##### Les expressions entre crochets et les classes de caractères :

Nous avons vu comment obtenir une correspondance à une position donnée. Maintenant nous allons voir comment obtenir une correspondance pour un jeu de caractères.

```shell
username@hostname:~$ grep  '[bg]zip'  dirlist-bin.txt
bzip2
bzip2recover
gzip

```

On peut utiliser ‘-’ pour définir une suite,

```shell
username@hostname:~$ grep  '[a-h]zip'  dirlist-bin.txt
bzip2
bzip2recover
gzip
hzip
prezip
prezip-bin
```

On peut utiliser ‘^’ entre les crochets pour faire la négation,

```shell
username@hostname:~$ grep  '[^a-z]zip'  dirlist-bin.txt 
gpg-zip
orcus-zip-dump
```

- Il est important de souligner que ‘^’ perd la signification de début de fichier lorsqu’il est mis entre ‘[]’.

- ‘^’ indique une négation que s’il occupe la première position.

##### Les classes de caractères POSIX :

| Classe     | Description                              |
| ---------- | ---------------------------------------- |
| [:alnum:]  | Caractères alphanumériques, équivalent à [A-Za-z0-9] |
| [:word:]   | Comme ‘:alnum:’ augmenté de ‘_’.         |
| [:alpha:]  | Caractères alphabétiques, équivalent à [A-Za-z]. |
| [:blank:]  | Inclut les espaces et les tabulations. Inclut les caractères ASCII entre 0 et 31 et 127. |
| [:digit:]  | Les chiffres de 0 à 9, équivalent à [0-9]. |
| [: graph:] | Les caractères ASCII visibles entre  33 et 126. |
| [:lower:]  | Les caractères alphabétiques en minuscules. |
| [:punct:]  | Les caractères de ponctuations, équivalent à [-!”#$%’()*+,./:;<=>?@[\\\]_`{\|}]. |
| [:space:]  | Les caractères vides, espace, tabulation, retour à la ligne, chariot retour, tabulation verticale, saut de page. Équivalent à [ \t\r\n\v\f]. |
| [:upper:]  | Les caractères alphabétiques en majuscules. |
| [:xdigit:] | Caracères utilisés pour exprimer les nombres hexadécimals, équivalent à [0-9A-Fa-f] |

Les expressions régulières que nous avons vues jusqu’à maintenant sont appellées ‘expressions régulières basiques’ (ERB). 

Il existe un autre type d’expressions régulières appellées ‘expressions régulières étendues’ (ERE). 

Ce qui distingues les deux types sont les métacaractères,

| ERB        | ERE            |
| ---------- | -------------- |
| ^$ . [ ] * | ( ) { } ? + \| |

Pour ajouter encore plus de confusion, les ‘( ) { }’ sont traités comme des métacaractères en ERB si ils sont échappés (avec ‘\’), tandis qu’en ERE n’importe quel métacaractère échappé sera traité comme un caractère litéral.

Historiquement ‘grep’ supporté ERB et ‘egrep’ supporté ERE. Aujourd’hui sur les systèmes GNU/Linux, il n’est plus nécessaire de faire de distinction, car grep supporte ERE en utilisant l’option ‘-E’.

## Les expressions régulières avancées :

##### Alternation :

L’alternation permet de chercher des correspondances entre plusieurs jeu d’expressions. Elle est symbolisé par la barre verticale ‘|’: 

```shell
username@hostname:~$ echo “AAA” | grep -E ‘AAA|BBB’
AAA
username@hostname:~$ echo “BBB” | grep -E ‘AAA|BBB’
BBB
username@hostname:~$ echo “CCC” | grep -E ‘AAA|BBB’
username@hostname:~$  grep -Eh '^(bz|gz|zip)' dirlist-bin.txt
bzcat
bzdiff
(…)
username@hostname:~$ grep -Eh '^bz|gz' dirlist-bin.txt
bzcat
Bzdiff
(…)
```

##### Les quantificateurs :

il est possible de spécifier le nombre de fois un élément est trouvé.

##### Trouver un élement zéro ou une fois avec ‘?’:

Imaginons que nous voulions trouver les numéros de téléphone dans un fichier qui sont dans une des deux formes suivantes :

(nnn) nnn-nnnn
nnn nnn-nnnn

Ceci pourrait être effectuer avec la commande suivante,

```shell
username@hostname:~$ grep -E ‘^\(?[0-9][0-9][0-9]\)? [0-9][0-9][0-9]-[0-9][0-9][0-9][0-9]$’ liste.txt
```

Remarquez les ‘\’.

##### Trouver un élement zéro ou plus avec ‘*’:

‘*’ est utilisé pour signifié un item optionnel, cependant, contrairement à ‘?’ l’item peut apparaître un nombre quelconque de fois, pas uniquement une fois. L’exemple typique est de vouloir vérifier qu’une chaîne de caractère est une phrase. Les phrases commencent par une majuscule, finissent par un point et on un nombre indeterminées de caractères,

```shell
username@hostname:~$ echo "ceci n'est pas une phrase" | grep -E '[[:upper:]][[:upper:][:lower:] ]*\.'
```

```shell
 username@hostname:~$ echo "Ceci est une phrase." | grep -E '[[:upper:]][[:upper:][:lower:] ]*\.'
 Ceci est une phrase.
```

