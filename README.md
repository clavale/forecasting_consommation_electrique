### Prévision de la consommation d'électricité Française du 31/12/2022

### Auteurs: Adjoua HOUNDONOUGBO and Morel MBEDI

### Ce projet  a été réalisé dans le cadre du cours de série temporelle  de notre formation "Master 2  Machine learning pour l'Intelligence Artificielle" à l'université Lumière Lyon 2


### L'objectif est de faire la prévision  de la consommation d'électricité Française (Nationale et régionales) du 31 décembre 2022 avec de données de résolution de 30 minutes

### Démarches et résultats (voir "Workflow_preprocessing_prévision_conso_electrique_France.pdf"):

- récupération de données  de consommation d'électricité(df_conso) sur le site RTE (01/01/2021 au 31/12/2022)
https://www.rte-france.com/eco2mix/telecharger-les-indicateurs
- récupération de données météo sur le site météo France(df_meteo) (01/01/2021 au 31/12/2022)
(https://public.opendatasoft.com/explore/dataset/donnees-synop-essentielles-omm/table/?sort=date) <br>
https://donneespubliques.meteofrance.fr/?fond=produit&id_produit=90&id_rubrique=32

- préprocessing en python:
- concatenation de 13 fichiers csv (1 par région + nationale). Retrait de données collectées pendant  15 et 45 minutes 
- ajout de la colonne region pour faire correspondre  chaque site de relevé météo dans sa région;Agregation de la température  moyenne (kelvin) par  region et datetime, interpolation linéaire pour compléter les temp des  datetimes manquantes rajoutées.Car les données météo sont collectées chaque 3 heures. l'idée c'est de collecter  les températures après chaque 30 mn au lieu  de 3h après.
- merge  de df_conso et df_meteo
- ajout de colonnes calendriers:weekday,isHoliday France,hour,isweekEnd,year,month
- prévision en utilisant le modèle prophet de chaqque série (13: 12 régions et 1 natinale) en python
- prévision hiérarchique(Nationale:régions): réconciliation ou rapprochement(révisions) des prévisions avec la librairie "sklearn-hts"
- prévision par experts: methode d'agrégation de poids des experts(randomforest,auto-arima, prophet,tslm) avec la librairie "opera" en R (utilisation du code R vu en TP)

- commentaire résultat, model prophet vs  methode reconciliation:
l'utilisation de la méthode de réconciliation a permis d'améliorer les prévisions de chaque série,car leurs RMSE(erreur moyenne quadratique) sont inférieures après la réconciliation des prévisions obtenues avec la méthode "prophet" (voir  "RMSE_prophet_and_reconciliation.png" ou le notebook "forecast_methode_reconciliation")

- commentaire résultat, méthode d'agregations des poids des experts ou prédicteurs (voir le code R "methode_experts.Rmd"):
le modèle tslm contribue plus à la prédiction de la consommation d'électriccité nationale  que les autres experts : prophet, arima,randomforest (voir "methode_experts.html" ou "methode_experts.Rmd")
