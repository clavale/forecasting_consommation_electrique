### Prévision de la consommation d'électricité Française du 31/12/2022

### Auteurs: Adjoua HOUNDONOUGBO and Morel MBEDI

### Ce projet  a été réalisé dans le cadre du cours de série temporelle  de notre formation "Master 2  Machine learning pour l'Intelligence Artificielle" à l'université Lumière Lyon 2


### L'objectif était de prévoir la consommation d'électricité Française (régionale et nationale) du 31 décembre 2022 pour chaque 30 minutes

### Démarches et résultats  (voir le worflow => "Workflow_preprocessing_prévision_conso_electrique_France.pdf"):

- récupération de données  de consommation d'électricité(df_conso) sur le site RTE (01/01/2021 au 31/12/2022)
https://www.rte-france.com/eco2mix/telecharger-les-indicateurs
- récupération de données météo sur le site météo France(df_meteo) (01/01/2021 au 31/12/2022)
(https://public.opendatasoft.com/explore/dataset/donnees-synop-essentielles-omm/table/?sort=date) <br>
https://donneespubliques.meteofrance.fr/?fond=produit&id_produit=90&id_rubrique=32

- préprocessing en python:
- df_conso: concatenation de 13 fichiers csv (12 régionS + 1 national). Retrait de données collectées pendant  15 et 45 minutes 
-df_meteo:  ajout de la colonne region pour faire correspondre  chaque site de relevé météo dans sa région;Agrégation de la température  moyenne (kelvin) par  region et datetime (présence de plusieurs sites de relévé météo dans la région), interpolation linéaire pour compléter les températures  manquantes des datetimes rajoutées.Car les données météo sont collectées chaque 3 heures. l'idée c'est de collecter  les données de températures après chaque 30 mn comme pour les données de consommation d'électricité.
- merge  de df_conso et df_meteo
- ajout de colonnes calendriers:weekday,isHoliday France,hour,isweekEnd,year,month
- prévision en utilisant le modèle prophet de chaque série (12 régions et 1 natinale) en python
- prévision hiérarchique(avec une hiérarchie "nationale:régions"): réconciliation ou rapprochement(révisions) des prévisions avec la librairie "sklearn-hts" en python
- prévision par experts: methode d'agrégation de poids des experts(randomforest,auto-arima, prophet,tslm) avec la librairie "opera" en R (utilisation du code R vu en TP)

- commentaire des résultats, model prophet vs méthode de réconciliation:
l'utilisation de la méthode de réconciliation a permis d'améliorer les prévisions de chaque série,car leurs RMSE(erreur moyenne quadratique) sont inférieures à celles obtenues avec la méthode "prophet" (voir  "RMSE_prophet_and_reconciliation.png" ou le notebook "forecast_methode_reconciliation")

- commentaire des résultats, méthode d'agrégation des poids des experts ou prédicteurs (voir le code R "methode_experts.Rmd"):
le modèle tslm contribue plus à la prédiction de la consommation d'électriccité nationale  que les autres experts : prophet, arima,randomforest (voir "methode_experts.html" ou "methode_experts.Rmd")
