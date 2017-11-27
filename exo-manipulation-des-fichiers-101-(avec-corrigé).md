# manipulation des fichiers (avec corrigé)

##### 1) créez un dossier `exo1`.

<u>Réponse :</u>

```shell
$ mkdir exo1
```



##### 2) entrez dans le dossier `exo1`.

<u>Réponse :</u>

```shell
$ cd exo1
```



##### 3) copiez le fichier `etc/passwd` dans le répertoire courant et donnez lui le nom de `fic1`.

<u>Réponse :</u>

```shell
$ cp /etc/passwd fic1
```



##### 4) faites un lien symbolique `fic2` pointant sur `fic1`.

<u>Réponse :</u>

```shell
$ ln -s fic1 fic2
```



##### 5) utilisez la commande `ls` pour afficher les différences entre ces deux fichiers. Vous devez observer le `inode` ainsi que le pointeur (`->`).

<u>Réponse :</u>

```shell
$ ls -li
```



##### 6) faites un lien logique `fic3` pointant sur `fic1`.

<u>Réponse :</u>

```shell
$ ln fic1 fic3
```



##### 7) utilisez la même commande que en 5) et observez les différences.

<u>Réponse :</u>

```shell
$ ls -li
```



##### 8) effacez `fic1`.

<u>Réponse :</u>

```shell
$ rm fic1
```



##### 9)  utilisez la même commande que en 5) et observez les différences.

<u>Réponse :</u>

```shell
$ ls -li
```



##### 10) copiez le fichier `/etc/hostname` dans `fic1`.

<u>Réponse :</u>

```shell
$ cp /etc/hostname fic1
```



##### 11) affichez les informations et comparez ce que vous avez trouvé lors des questions 3) et 7).

<u>Réponse :</u>

```shell
$ ls -li
```



##### 12) créez les dossiers `dir1/dirA` (notez que `dirA` est contenu dans le dossier `dir1`) en UNE commande.

<u>Réponse :</u>

```shell
$ mkdir -p dir1/dirA
```



##### 13) copiez `fic1`, `fic2`, `fic3` dans le dossier `dirA` que vous venez de créer. Faites le en UNE seule commande.

<u>Réponse :</u>

```shell
$ cp fic1 fic2 fic3 dir1/dirA
```



##### 14) rendez-vous dans le dossier `dirA `. Faites le en UNE seule commande.

<u>Réponse :</u>

```shell
$ cd dir1/dirA
```



##### 15) affichez les informations sur les fichiers. Observez vous des différences entre maintenant et ce que vous avez pu observer à l'étape 11) ?

<u>Réponse :</u>

```shell
$ ls -li
```



##### 16) effacez les fichiers présents dans ce dossier.

<u>Réponse :</u>

```shell
$ rm *
```



##### 17) utilisez man page avec la commande `cp` pour trouver commande faire une copie recursive de fichiers `fic1`, `fic2` et `fic3` dans le dossier `dirA`. Faites le en une commande ET depuis le dossier `dirA`.

<u>Réponse :</u>

```shell
$ cp -r ../../fic* .
```



##### 18) affichez les informations sur les fichiers. Quels sont les différences observées avec l'étape 15).

<u>Réponse :</u>

```shell
$ ls -li
```



##### 19) depuis le dossier `dirA` ET en une commande, effacez le fichier `fic1` qui se trouve dans le dossier `exo1`.

<u>Réponse :</u>

```shell
$ rm ../../fic1
```



##### 20) toujours depuis le dossier `dirA`, affichez les informations sur le contenu du dossier `exo1`. Puis affichez les informations du dossier `dirA`. Comparez.

<u>Réponse :</u>

```shell
$ ls -li ../../
$ ls -li
```



##### 21) déplacer le fichier `fic1` vers le dossier `exo1`.

<u>Réponse :</u>

```shell
$ mv fic1 ../../
```



##### 22) affichez les informations sur le contenu des dossier `exo1` et `dirA`. Cette fois-çi faites le en UNE commande!

<u>Réponse :</u>

```shell
$ ls -li . ../../
```



##### 23) utilisez `man`  pour trouver comment faire une copie interactive.

<u>Réponse</u> :

```shell
$ man cp
```



##### 24) utilisez l'option de copie interactive en copiant les fichiers depuis le dossier `exo1` vers le dossier `dirA`.

<u>Réponse :</u>

```shell
$ cp -i ../../fic* .
```



##### 25) retournez à votre dossier personnel.

<u>Réponse :</u>

```
$ cd
```



##### 26) affichez les informations du dossier en oubliant pas de lister les fichiers cachés. Il existe deux façons de faire. Utilisez man pour les trouver. Quels sont les différences entre ces deux options ?

<u>Réponse :</u>

```shell
$ ls -a
$ ls -A
```



##### 27) que représente "." ".." ?



##### 28) copiez le dossier `.config` dans le dossier `exo1`.

<u>Réponse :</u>

```shell
$ cp .config exo1
```



##### 29) entrez la commande `ls`, vous ne devriez pas observer le dossier `.config`.



##### 30) renomez le dossier .config pour config.

<u>Réponse :</u>

```shell
$ mv .config config
```



##### 31) faites un dernier `ls` . Vous devriez observer le dossier `config.`

<u>Réponse :</u>

```shell
$ ls
```