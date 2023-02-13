### forecasting_consommation_electrique

objectif est de faire la prévision  de la consommation d'életricité Française(Nationale et régionales) du 31 décembre 2022 avec de données de résolution de 30mn

- recupération de données  de consommation d'électricité(df_conso) sur le site RTE (01/01/2021 au 31/12/2022)
- récupération de données météo sur le site météo France(df_meteo) (01/01/2021 au 31/12/2022)
(https://public.opendatasoft.com/explore/dataset/donnees-synop-essentielles-omm/table/?sort=date)
- préprocessing en python:
- concatenation de 13 fichiers csv (1 par région + nationale) et ajout de la colonne region sur le df_conso. Retrait de données collectées pendant  15 et 45 minutes 
- agregation de la temp  moyenne par  region et datetime de la température(kelvin), interpolation linéaire pour completer les dateyime mmanquantes : car les données météo sont collectées chaque 3 heure
- merge  de df_conso et df_meteo
- ajout de colonne calendrier:weekday,isHoliday France,hour,isweekEnd,year,month
- previosn avec le modele prophet de chaqque série (13)
- réconciliation des prévisions avec la librairie sklearn-hts
- methode d'experts des prévisions nationales en R
