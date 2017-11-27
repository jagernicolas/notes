# grep

##### 1) copiez, dans un fichier `data`, la liste des maires de la ville de Montréal :

```shell

    1833 - 1836 : Jacques Viger (1er maire)
    1836 - 1840 : Tutelle : Montréal est administrée par des juges de paix
    1840 - 1842 : Peter McGill
    1842 - 1844 : Joseph Bourret
    1844 - 1846 : James Ferrier
    1846 - 1847 : John Easton Mills
    1847 - 1850 : Joseph Bourret
    1850 - 1851 : Édouard-Raymond Fabre
    1851 - 1854 : Charles Wilson (1er maire élu)
    1854 - 1856 : Wolfred Nelson (Parti patriote)
    1856 - 1858 : Henry Starnes
    1858 - 1862 : Charles-Séraphin Rodier
    1862 - 1866 : Jean-Louis Beaudry
    1866 - 1868 : Henry Starnes
    1868 - 1871 : William Workman
    1871 - 1873 : Charles-Joseph Coursol
    1873 : Francis Cassidy
    1873 - 1875 : Aldis Bernard
    1875 - 1877 : William H. Hingston
    1877 - 1879 : Jean-Louis Beaudry
    1879 - 1881 : Sévère Rivard
    1881 - 1885 : Jean-Louis Beaudry
    1885 - 1887 : Honoré Beaugrand
    1887 - 1889 : John Joseph Caldwell Abbott (futur Premier ministre du Canada)
    1889 - 1891 : Jacques Grenier
    1891 - 1893 : James McShane
    1893 - 1894 : Alphonse Desjardins (sans rapport avec le fondateur du Mouvement Desjardins)
    1894 - 1896 : Joseph-Octave Villeneuve
    1896 - 1898 : Richard Wilson-Smith
    1898 - 1902 : Raymond Préfontaine
    1902 - 1904 : James Cochrane
    1904 - 1906 : Hormidas Laporte
    1906 - 1908 : Henry Archer Ekers
    1908 - 1910 : Louis Payette
    1910 - 1912 : James John Edmund Guerin
    1912 - 1914 : Louis-Arsène Lavallée
    1914 - 1924 : Médéric Martin
    1924 - 1926 : Charles Duquette
    1926 - 1928 : Médéric Martin
    1928 - 1932 : Camillien Houde (Parti conservateur)
    1932 - 1934 : Fernand Rinfret
    1934 - 1936 : Camillien Houde
    1936 - 1938 : Adhémar Raynault
    1938 - 1940 : Camillien Houde (arrêté en fonction le 5 août, enfermé au camp de concentration de Petawawa sans procès)
    1940 - 1944 : Adhémar Raynault
    1944 - 1954 : Camillien Houde
    1954 - 1957 : Jean Drapeau (Parti civique)
    1957 - 1960 : Sarto Fournier (Parti libéral du Canada)
    1960 - 1986 : Jean Drapeau (Parti civique)
    1986 - 1994 : Jean Doré (Rassemblement des citoyens de Montréal)
    1994 - 2001 : Pierre Bourque (Vision Montréal)
    2001 - 2012 : Gérald Tremblay (Équipe Tremblay - Union Montréal)
    2012 - 2013 : Michael Applebaum (Maire par intérim)
    2013 : Laurent Blanchard (Maire par intérim)
    2013 - 2017 : Denis Coderre (Équipe Denis Coderre pour Montréal)
    2017 - : Valérie Plante (Projet Montréal)

```

<u>source :</u> https://fr.wikipedia.org/wiki/Liste_des_maires_de_Montr%C3%A9al



##### 2) Utilisez `grep` pour afficher quand Jean Drapeau à été maire de montréal.

<u>Vous devriez observer :</u>

```shell
1954 - 1957 : Jean Drapeau (Parti civique)
1960 - 1986 : Jean Drapeau (Parti civique)
```


##### 3) on veut également connaître les numéros des lignes.

<u>Vous devriez observer :</u>

```shell
48:    1954 - 1957 : Jean Drapeau (Parti civique)
50:    1960 - 1986 : Jean Drapeau (Parti civique)
```


##### 4) on veut maintenant voir le nombre de fois que Jean Drapeau apparaît dans le fichier.

<u>Vous devriez observer :</u>

```shell
2
```


##### 5) on veut afficher toutes les lignes contenant `bb`.

<u>Vous devriez observer :</u> 

```shell
    1887 - 1889 : John Joseph Caldwell Abbott (futur Premier ministre du Canada)
```


##### 6) on veut exatcement le même résultat mais en utilisant la notation suivante :

```shell
$ grep -E '[😋]{😋}' data
```

<u>Vous devriez observer :</u>

```shell
1887 - 1889 : John Joseph Caldwell Abbott (futur Premier ministre du Canada)
```


##### 7) remplacez `grep -E` par `egrep`. Observez vous une quelconque différence ? que veut dire `-E` dans le premier cas ? vérifiez si `egrep` veut dire la même chose (utilisez `man`).



##### 8) Affichez toutes les lignes qui finnisent par `rd`.

<u>Vous devriez observer :</u>

```shell
    1873 - 1875 : Aldis Bernard
    1879 - 1881 : Sévère Rivard
```


##### 9) affichez toutes les lignes qui finnissent soient par `r` soit par `d`.

<u>Vous devriez observer :</u>

```shell
    1844 - 1846 : James Ferrier
    1858 - 1862 : Charles-Séraphin Rodier
    1873 - 1875 : Aldis Bernard
    1879 - 1881 : Sévère Rivard
    1885 - 1887 : Honoré Beaugrand
    1889 - 1891 : Jacques Grenier
```


##### 10) affichez toutes les lignes qui ont soit `ll` soit `pp`.

<u>Vous devriez observer :</u>

```shell
1836 - 1840 : Tutelle : Montréal est administrée par des juges de paix
1840 - 1842 : Peter McGill
1846 - 1847 : John Easton Mills
1868 - 1871 : William Workman
1875 - 1877 : William H. Hingston
1887 - 1889 : John Joseph Caldwell Abbott (futur Premier ministre du Canada)
1893 - 1894 : Alphonse Desjardins (sans rapport avec le fondateur du Mouvement Desjardins)
1894 - 1896 : Joseph-Octave Villeneuve
1912 - 1914 : Louis-Arsène Lavallée
1928 - 1932 : Camillien Houde (Parti conservateur)
1934 - 1936 : Camillien Houde
1938 - 1940 : Camillien Houde (arrêté en fonction le 5 août, enfermé au camp de concentration de Petawawa sans procès)
1944 - 1954 : Camillien Houde
2012 - 2013 : Michael Applebaum (Maire par intérim)
```



##### 11) affichez toutes les lignes qui commencent par `18`.

<u>Vous devriez observer :</u>

```shell
    1833 - 1836 : Jacques Viger (1er maire)
    1836 - 1840 : Tutelle : Montréal est administrée par des juges de paix
    1840 - 1842 : Peter McGill
    1842 - 1844 : Joseph Bourret
    1844 - 1846 : James Ferrier
    1846 - 1847 : John Easton Mills
    1847 - 1850 : Joseph Bourret
    1850 - 1851 : Édouard-Raymond Fabre
    1851 - 1854 : Charles Wilson (1er maire élu)
    1854 - 1856 : Wolfred Nelson (Parti patriote)
    1856 - 1858 : Henry Starnes
    1858 - 1862 : Charles-Séraphin Rodier
    1862 - 1866 : Jean-Louis Beaudry
    1866 - 1868 : Henry Starnes
    1868 - 1871 : William Workman
    1871 - 1873 : Charles-Joseph Coursol
    1873 : Francis Cassidy
    1873 - 1875 : Aldis Bernard
    1875 - 1877 : William H. Hingston
    1877 - 1879 : Jean-Louis Beaudry
    1879 - 1881 : Sévère Rivard
    1881 - 1885 : Jean-Louis Beaudry
    1885 - 1887 : Honoré Beaugrand
    1887 - 1889 : John Joseph Caldwell Abbott (futur Premier ministre du Canada)
    1889 - 1891 : Jacques Grenier
    1891 - 1893 : James McShane
    1893 - 1894 : Alphonse Desjardins (sans rapport avec le fondateur du Mouvement Desjardins)
    1894 - 1896 : Joseph-Octave Villeneuve
    1896 - 1898 : Richard Wilson-Smith
    1898 - 1902 : Raymond Préfontaine
```

> ATTENTION : si vous avez des espaces au début de la lignes, vous devez en tenir compte dans votre commande



##### 12) afficher le nom de la nouvelle mairesse de Montréal, en complétant la commande suivante :

```shell
$ grep -E '2017 [😋:]' data
```


<u>Vous devriez observer :</u>

```shell
    2017 - : Valérie Plante (Projet Montréal)
```