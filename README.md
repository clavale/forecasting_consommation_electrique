### forecasting_consommation_electrique

objectif est de faire la prévision  de la consommation d'életricité Française(Nationale et régionales) du 31 décembre 2022 avec de données de résolution de 30mn

- recupération de données  de consommation d'électricité(df_conso) sur le site RTE (01/01/2021 au 31/12/2022)
- récupération de données météo sur le site météo France(df_meteo) (01/01/2021 au 31/12/2022)
(https://public.opendatasoft.com/explore/dataset/donnees-synop-essentielles-omm/table/?sort=date)
- préprocessing en python:
- concatenation de 13 fichiers csv (1 par région + nationale) et ajout de la colonne region sur le df_conso. Retrait de données collectées pendant  15 et 45 minutes 
- agregation de la température  moyenne (kelvin) par  region et datetime, interpolation linéaire pour compléter les datetimes mmanquantes.Car les données météo sont collectées chaque 3 heures
- merge  de df_conso et df_meteo
- ajout de colonnes calendriers:weekday,isHoliday France,hour,isweekEnd,year,month
- prévision en utlisant le modèle prophet de chaqque série (13: 12 régions et 1 natinale)
- réconciliation des prévisions avec la librairie sklearn-hts
- methode d'agrégation de poids des experts(randomforest,auto-arima, prophet,tslm) avec la librairie "opera" en R 
