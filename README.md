### forecasting_consommation_electrique

objectif est de faire la prévision  de la consommation d'életricité Française(Nationale et régionales) du 31 décembre 2022 avec de données de résolution de 30mn

- récupération de données  de consommation d'électricité(df_conso) sur le site RTE (01/01/2021 au 31/12/2022)
- récupération de données météo sur le site météo France(df_meteo) (01/01/2021 au 31/12/2022)
(https://public.opendatasoft.com/explore/dataset/donnees-synop-essentielles-omm/table/?sort=date)
- préprocessing en python:
- concatenation de 13 fichiers csv (1 par région + nationale). Retrait de données collectées pendant  15 et 45 minutes 
- ajout de la colonne region pour faire correspondre  chaque site de relevé météo dans sa région;Agregation de la température  moyenne (kelvin) par  region et datetime, interpolation linéaire pour compléter les temp des  datetimes manquantes rajoutées.Car les données météo sont collectées chaque 3 heures. l'idée est ce ça soit pour chaque 30 mn
- merge  de df_conso et df_meteo
- ajout de colonnes calendriers:weekday,isHoliday France,hour,isweekEnd,year,month
- prévision en utlisant le modèle prophet de chaqque série (13: 12 régions et 1 natinale) en python
- réconciliation ou rapprochement(revisions) des prévisions avec la librairie sklearn-hts
- methode d'agrégation de poids des experts(randomforest,auto-arima, prophet,tslm) avec la librairie "opera" en R 

commentaire résultat, model prophet vs  methodes reconciliations:
l'utilisation de la méthode de réconciliation a permis d'améliorer les prévisions de chaque série car leur RMSE(erreur moyenne quadratique) est inférieure avant la réconciliation (voir  "RMSE_prophet_and_reconciliation.png" ou le notebook "forecast_methode_reconciliation")

commentaire résultat, méthode d'agregations (experts):(voir  le le code R "forecast_methode_experts")
