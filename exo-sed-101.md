# EXERCICE SED

[TOC]



##### 1)  allez dans un nouveau répertoire de travail. Tapez la commande suivante :

```shell
$ ls /bin/ /sbin | grep -i zip > data-sed
```



##### 2) utilisez la commande `head` pour afficher les cinq premières lignes.

Vous devriez observer :

```shell
bunzip2
bzip2
bzip2recover
funzip
gpg-zip
```

##### 3) effacez la troisième ligne. Affichez le résultat. **Vous n'avez pas à appliquer les modifications sur le fichier**.

Vous devriez observer :

```shell
bunzip2
bzip2
funzip
gpg-zip
gunzip
```



##### 4) affichez les dix dernières lignes avec `cat` et `tail`. Aucun `sed` n 'est requis.

Vous devriez observer :

```shell
prezip
prezip-bin
unzip
unzipsfx
zip
zipcloak
zipgrep
zipinfo
zipnote
zipsplit
```



##### 5) remplacez les `zip` (par `ZIP`) qui apparaissent au début de chaque ligne. **Partez de la même commande écrite en 4)**, utilisez un tube est invoquez la bonne commande `sed`. 

Vous devriez observer :

```shell
prezip
prezip-bin
unzip
unzipsfx
ZIP
ZIPcloak
ZIPgrep
ZIPinfo
ZIPnote
ZIPsplit
```



##### 6) reprenez la commande précédente et ajoutez un `sed` qui retire les lignes qui ne contiennent pas `ZIP`. Complétez et utilisez le fragment suivant :

```shell
(...) | sed '/ZIP/😋d'
```

Vous devriez observer :

```shell
ZIP
ZIPcloak
ZIPgrep
ZIPinfo
ZIPnote
ZIPsplit
```



##### 7) affichez les dix premières lignes de `data-sed`.

Vous devriez observer :

```shell
bunzip2
bzip2
bzip2recover
funzip
gpg-zip
gunzip
gzip
hunzip
hzip
mwawZip
```



##### 8) en partant de la commande précédente, effacer les lignes commençantes par `gzip` et `bzip`. Vous aller avoir besoin d'un opérateur 'ou', représenté par `|`. Il faut échapper ce caractère sans quoi il sera interprété comme un tube.

Vous devriez observer :
```shell
bunzip2
funzip
gpg-zip
gunzip
hunzip
hzip
mwawZip
```

