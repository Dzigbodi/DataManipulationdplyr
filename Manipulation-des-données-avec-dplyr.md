Manipulation des données dans R avec dplyr
================
Richard Amegble
2023-12-15

# Description du projet

Dans cet exercice nous apprenons comment nous pouvons importer les
données dans `R` et les manipuler avec la librairie `dplyr`.

## Importation des données csv

- Importer les indices des prix du fichier : Prix.csv, en utilisant
  `read.csv()`

``` r
Prix<-read.csv("Prix.csv")
```

- Importer le fichier du dictionnaire des données: Dictionnaire.csv, en
  utilisant égalemet `read.csv()`

``` r
Dictionnaire<-read.csv("Dictionnaire.csv")
```

- Analyser le contenu des données des prix en utilisant la fonction:
  `str()`

<!-- -->

    ## 'data.frame':    28600 obs. of  8 variables:
    ##  $ CODE               : chr  "SSSSR3021M0BP" "SSSSR3021M0BP" "SSSSR3021M0BP" "SSSSR3021M0BP" ...
    ##  $ UNITE.DE.MESURE    : chr  "Aucune" "Aucune" "Aucune" "Aucune" ...
    ##  $ MAGNITUDE          : chr  "aucune" "aucune" "aucune" "aucune" ...
    ##  $ SOURCE             : chr  "Institut National de la Statistique, Base 100=2014" "Institut National de la Statistique, Base 100=2014" "Institut National de la Statistique, Base 100=2014" "Institut National de la Statistique, Base 100=2014" ...
    ##  $ TYPE_SERIE         : chr  "Indice" "Indice" "Indice" "Indice" ...
    ##  $ METHODE_OBSERVATION: chr  "Moyenne sur la periode" "Moyenne sur la periode" "Moyenne sur la periode" "Moyenne sur la periode" ...
    ##  $ Date               : chr  "2/1/2000" "3/1/2000" "4/1/2000" "5/1/2000" ...
    ##  $ Prix               : num  0 0 0 0 0 0 0 0 0 0 ...

    ## 'data.frame':    104 obs. of  3 variables:
    ##  $ SERIE    : chr  "AAASR3021M0BP" "AAASR3050M0BP" "AAASR3022M0BP" "AAASR3024M0BP" ...
    ##  $ PAYS     : chr  "COTE D'IVOIRE" "COTE D'IVOIRE" "COTE D'IVOIRE" "COTE D'IVOIRE" ...
    ##  $ COMMODITE: chr  "01--Alimentation" "02--Boissons alcoolisees, Tabac et stupefiants" "03--Habillement" "04--Ameublement" ...

- Selectionner les variables CODE, Date, Prix

``` r
Prix_df<-Prix|>
         select(CODE, Date, Prix)
str(Prix_df)
```

    ## 'data.frame':    28600 obs. of  3 variables:
    ##  $ CODE: chr  "SSSSR3021M0BP" "SSSSR3021M0BP" "SSSSR3021M0BP" "SSSSR3021M0BP" ...
    ##  $ Date: chr  "2/1/2000" "3/1/2000" "4/1/2000" "5/1/2000" ...
    ##  $ Prix: num  0 0 0 0 0 0 0 0 0 0 ...

- Fusionner les bases Prix et Dictionnaire en utilisant la fonction
  `inner_join()` en utilisant SERIE dans la base de Dictionnaire et CODE
  dans la base Prix comme clés de fusion.

``` r
Prix_Dict<-inner_join(Prix_df,Dictionnaire, by=c("CODE"="SERIE") )|>
           relocate(PAYS,CODE,COMMODITE,Date,Prix)
str(Prix_Dict)
```

    ## 'data.frame':    28600 obs. of  5 variables:
    ##  $ PAYS     : chr  "GUINEE BISSAU" "GUINEE BISSAU" "GUINEE BISSAU" "GUINEE BISSAU" ...
    ##  $ CODE     : chr  "SSSSR3021M0BP" "SSSSR3021M0BP" "SSSSR3021M0BP" "SSSSR3021M0BP" ...
    ##  $ COMMODITE: chr  "01--Alimentation" "01--Alimentation" "01--Alimentation" "01--Alimentation" ...
    ##  $ Date     : chr  "2/1/2000" "3/1/2000" "4/1/2000" "5/1/2000" ...
    ##  $ Prix     : num  0 0 0 0 0 0 0 0 0 0 ...

``` r
Prix_Dict<-inner_join(Prix_df,
                      Dictionnaire|>rename(CODE=SERIE), 
                      by=c("CODE") )|>
           relocate(PAYS,CODE,COMMODITE,Date,Prix)
str(Prix_Dict)
```

    ## 'data.frame':    28600 obs. of  5 variables:
    ##  $ PAYS     : chr  "GUINEE BISSAU" "GUINEE BISSAU" "GUINEE BISSAU" "GUINEE BISSAU" ...
    ##  $ CODE     : chr  "SSSSR3021M0BP" "SSSSR3021M0BP" "SSSSR3021M0BP" "SSSSR3021M0BP" ...
    ##  $ COMMODITE: chr  "01--Alimentation" "01--Alimentation" "01--Alimentation" "01--Alimentation" ...
    ##  $ Date     : chr  "2/1/2000" "3/1/2000" "4/1/2000" "5/1/2000" ...
    ##  $ Prix     : num  0 0 0 0 0 0 0 0 0 0 ...

``` r
Prix_Dict<-Prix_df|> inner_join(
                      Dictionnaire|>rename(CODE=SERIE), 
                      by=c("CODE") )|>
           relocate(PAYS,CODE,COMMODITE,Date,Prix)
str(Prix_Dict)
```

    ## 'data.frame':    28600 obs. of  5 variables:
    ##  $ PAYS     : chr  "GUINEE BISSAU" "GUINEE BISSAU" "GUINEE BISSAU" "GUINEE BISSAU" ...
    ##  $ CODE     : chr  "SSSSR3021M0BP" "SSSSR3021M0BP" "SSSSR3021M0BP" "SSSSR3021M0BP" ...
    ##  $ COMMODITE: chr  "01--Alimentation" "01--Alimentation" "01--Alimentation" "01--Alimentation" ...
    ##  $ Date     : chr  "2/1/2000" "3/1/2000" "4/1/2000" "5/1/2000" ...
    ##  $ Prix     : num  0 0 0 0 0 0 0 0 0 0 ...

- La fusion peut se faire aussi par la fonction `left_join()` dans ce
  cas on ramène les informations du dictionnaire des données dans la
  base données des prix.

``` r
Prix_Dict<-Prix_df|> left_join(
                      Dictionnaire|>rename(CODE=SERIE), 
                      by=c("CODE") )|>
           relocate(PAYS,CODE,COMMODITE,Date,Prix)
str(Prix_Dict)
```

    ## 'data.frame':    28600 obs. of  5 variables:
    ##  $ PAYS     : chr  "GUINEE BISSAU" "GUINEE BISSAU" "GUINEE BISSAU" "GUINEE BISSAU" ...
    ##  $ CODE     : chr  "SSSSR3021M0BP" "SSSSR3021M0BP" "SSSSR3021M0BP" "SSSSR3021M0BP" ...
    ##  $ COMMODITE: chr  "01--Alimentation" "01--Alimentation" "01--Alimentation" "01--Alimentation" ...
    ##  $ Date     : chr  "2/1/2000" "3/1/2000" "4/1/2000" "5/1/2000" ...
    ##  $ Prix     : num  0 0 0 0 0 0 0 0 0 0 ...

- La fusion peut se faire aussi par la fonction `right_join()` dans ce
  cas on ramène les informations des données des prix dans la base
  données du dictionnaire.

``` r
Prix_Dict<-Dictionnaire|>
          rename(CODE=SERIE)|> right_join(
                     Prix_df, 
                      by=c("CODE") )|>
           relocate(PAYS,CODE,COMMODITE,Date,Prix)
str(Prix_Dict)
```

    ## 'data.frame':    28600 obs. of  5 variables:
    ##  $ PAYS     : chr  "COTE D'IVOIRE" "COTE D'IVOIRE" "COTE D'IVOIRE" "COTE D'IVOIRE" ...
    ##  $ CODE     : chr  "AAASR3021M0BP" "AAASR3021M0BP" "AAASR3021M0BP" "AAASR3021M0BP" ...
    ##  $ COMMODITE: chr  "01--Alimentation" "01--Alimentation" "01--Alimentation" "01--Alimentation" ...
    ##  $ Date     : chr  "2/1/2000" "3/1/2000" "4/1/2000" "5/1/2000" ...
    ##  $ Prix     : num  57.4 60.2 61.4 61.9 63.9 63.9 62.4 60.5 59.8 58.4 ...

## Bonus: Manipulation avec les objets date

- Convertir la variable Date en format date en utilisant la fonction
  `as.Date()`

``` r
Prix_Dict<-Prix_Dict|>mutate(Date=as.Date(Date, format="%m/%d/%Y"))
str(Prix_Dict)
```

    ## 'data.frame':    28600 obs. of  5 variables:
    ##  $ PAYS     : chr  "COTE D'IVOIRE" "COTE D'IVOIRE" "COTE D'IVOIRE" "COTE D'IVOIRE" ...
    ##  $ CODE     : chr  "AAASR3021M0BP" "AAASR3021M0BP" "AAASR3021M0BP" "AAASR3021M0BP" ...
    ##  $ COMMODITE: chr  "01--Alimentation" "01--Alimentation" "01--Alimentation" "01--Alimentation" ...
    ##  $ Date     : Date, format: "2000-02-01" "2000-03-01" ...
    ##  $ Prix     : num  57.4 60.2 61.4 61.9 63.9 63.9 62.4 60.5 59.8 58.4 ...

- Extraire le Mois et l’année en utilisant `lubridate()`

``` r
library(lubridate)
Prix_Dict<-Prix_Dict|>mutate(Mois=month(Date), Annee=year(Date))
str(Prix_Dict)
```

    ## 'data.frame':    28600 obs. of  7 variables:
    ##  $ PAYS     : chr  "COTE D'IVOIRE" "COTE D'IVOIRE" "COTE D'IVOIRE" "COTE D'IVOIRE" ...
    ##  $ CODE     : chr  "AAASR3021M0BP" "AAASR3021M0BP" "AAASR3021M0BP" "AAASR3021M0BP" ...
    ##  $ COMMODITE: chr  "01--Alimentation" "01--Alimentation" "01--Alimentation" "01--Alimentation" ...
    ##  $ Date     : Date, format: "2000-02-01" "2000-03-01" ...
    ##  $ Prix     : num  57.4 60.2 61.4 61.9 63.9 63.9 62.4 60.5 59.8 58.4 ...
    ##  $ Mois     : num  2 3 4 5 6 7 8 9 10 11 ...
    ##  $ Annee    : num  2000 2000 2000 2000 2000 2000 2000 2000 2000 2000 ...

## Référence

- Wickham H, François R, Henry L, Müller K (2022). *dplyr: A Grammar of
  Data Manipulation*. R package version 1.0.9,
  <https://CRAN.R-project.org/package=dplyr>.
