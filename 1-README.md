# Jeux de données publiés par ting.ad (producteur)

Trois fichiers d'**agrégats** sur le 15e arrondissement de Paris, prêts à être publiés comme jeux de données de ting.ad :

- sur **data.gouv.fr**, sous une organisation « ting.ad » à créer ;
- sur Kaggle et sur Hugging Face, plus tard, avec les mêmes fichiers et le même texte.

Ils sont générés par `scripts/studies/export_datasets_15e.py` à partir des fichiers de travail. Pour les régénérer :

```bash
python scripts/studies/export_datasets_15e.py <work> docs/backlinks/datasets
```

**Rien n'est publié.** La publication se fait à la main, depuis le compte data.gouv.fr d'Axel ou de son associé. Le pas à pas est à la fin de ce fichier.

Pourquoi publier nos propres jeux :

- un jeu de données producteur porte le nom et le lien de ting.ad sur sa page data.gouv ;
- il est moissonné par d'autres portails ;
- il est cité par ceux qui le réutilisent.

C'est le lien le plus durable et le plus légitime que l'open data puisse donner.

---

## Jeu 1 : Prix de vente médian au m² des appartements, par rue, Paris 15e, 2021-2025

- **Fichier** : `prix-median-m2-par-rue-paris-15e-2021-2025.csv`
  - 4 805 lignes : le total du 15e et 307 rues ;
  - UTF-8, séparateur virgule, point décimal.
- **Licence** : Licence Ouverte 2.0 (Etalab), la même que DVF.
- **Fréquence** : annuelle, après la mise à jour DVF de printemps.
- **Couverture** : commune 75115 (Paris 15e), ventes du 1er janvier 2021 au 31 décembre 2025, fichiers DVF mis à jour le 18 mai 2026.
- **Mots-clés** : dvf, prix-immobilier, prix-m2, paris, paris-15e, appartements, transactions

**Description**, à coller sur data.gouv.fr :

> Prix de vente médian au mètre carré des appartements dans chaque rue du 15e arrondissement de Paris, de 2021 à 2025. Il est donné pour toute la période, par période (2021-2022, 2023, 2024-2025) et par nombre de pièces (1, 2, 3, 4 et plus).
>
> Calculé par ting.ad à partir des Demandes de valeurs foncières géolocalisées (DGFiP, Etalab). Une vente compte si c'est une mutation « Vente » dont la partie bâtie est exactement un appartement (dépendances admises), d'au moins 9 m². Le prix au m² est la valeur foncière divisée par la surface réelle bâtie. Une vente est rattachée à la rue quand son nom de voie DVF correspond à celui de la Base Adresse Nationale. Les ventes sans correspondance (301 sur 15 650) ne comptent que dans le total du 15e.
>
> Règles de confidentialité, conformes aux conditions de réutilisation de DVF :
> - une médiane n'est publiée qu'à partir de 5 ventes, arrondie à 10 € ;
> - une surface médiane n'est publiée qu'à partir de 10 ventes ;
> - un groupe de 1 à 4 ventes est noté « 1-4 », sans aucun chiffre ;
> - le nombre de ventes est donné par tranche (5-9, 10-19, 20-49, 50-99, 100-199, 200-499, 500-999, 1000+) : un total exact permettrait de retrouver, par soustraction, le nombre exact d'un groupe de 1 à 4 ventes ;
> - aucune vente individuelle, aucune adresse précise, aucun lot.
>
> Méthode et code : scripts/studies/export_datasets_15e.py (ting.ad).

**Colonnes**

| Colonne | Type | Contenu |
|---|---|---|
| `code_commune` | texte | code INSEE : `75115` |
| `commune` | texte | `Paris 15e` |
| `voie` | texte | nom de la rue selon la Base Adresse Nationale, ou « Paris 15e arrondissement (toutes les rues) » pour le total |
| `voie_cle_dvf` | texte | nom de voie normalisé servant au rapprochement DVF / BAN (vide pour le total) |
| `periode` | texte | `2021-2025`, `2021-2022`, `2023`, `2024-2025` |
| `pieces` | texte | `toutes`, `1`, `2`, `3`, `4+` (nombre de pièces principales selon DVF) |
| `ventes` | tranche | nombre de ventes du groupe par tranche : `1-4`, `5-9`, `10-19`, `20-49`, `50-99`, `100-199`, `200-499`, `500-999`, `1000+` |
| `prix_median_eur_m2` | entier | prix médian au m², arrondi à 10 € ; vide sous 5 ventes |
| `surface_mediane_m2` | entier | surface médiane en m² ; vide sous 10 ventes |

**Obligations DVF à rappeler sur la page du jeu** :

- interdiction de réidentifier les personnes, même indirectement (art. R112 A-3 du livre des procédures fiscales) ;
- les données à caractère personnel ne doivent pas être indexées par les moteurs de recherche (décret 2018-1350).

Ce fichier ne contient que des agrégats d'au moins 5 ventes.

**Jeux de données sources à associer** :

- <https://www.data.gouv.fr/datasets/demandes-de-valeurs-foncieres-geolocalisees>
- <https://www.data.gouv.fr/datasets/base-adresse-nationale>

**Réutilisation de référence** : <https://ting.ad/fr/prix-m2/paris-15e/>

## Jeu 2 : Exposition au bruit des transports, par immeuble, Paris 15e

- **Fichier** : `exposition-bruit-par-immeuble-paris-15e.csv`
  - 4 756 lignes, une par immeuble d'habitation ;
  - UTF-8, séparateur virgule.
- **Licence** : Licence Ouverte 2.0. Les sources (cartes de bruit stratégiques, Bruitparif, BDNB, BAN) sont toutes sous Licence Ouverte 2.0.
- **Fréquence** : à chaque nouvelle échéance des cartes de bruit. Les cartes actuelles (4e échéance) sont valides jusqu'au 30 juin 2027.
- **Mots-clés** : bruit, nuisance-bruit, cartes-de-bruit, bruitparif, immeuble, logement, paris-15e

**Description** :

> Exposition au bruit des transports devant chacun des 4 756 immeubles d'habitation du 15e arrondissement de Paris. Pour chaque immeuble, le fichier donne :
> - la bande de bruit routier Lden et Ln (par tranches de 5 dB(A), calculée à 4 m du sol) devant la façade sur rue, et le dépassement éventuel des valeurs limites réglementaires (route : Lden 68, Ln 62) ;
> - le bruit ferroviaire du réseau SNCF et du réseau RATP (avec la ligne) ;
> - l'indice de bruit des transports 2024 de Bruitparif (préservé, altéré, très dégradé).
>
> Sources :
> - cartes de bruit stratégiques de Paris, 4e échéance (Bruitparif, SNCF Réseau, RATP ; DRIEAT Île-de-France), Licence Ouverte 2.0 ;
> - Bruitparif (avec Airparif), cartographie air-bruit 2024, Licence Ouverte 2.0 ;
> - point adresse de la Base Adresse Nationale ;
> - nombre de logements de la BDNB (CSTB).
>
> L'exposition est mesurée au point adresse, qui est situé sur la façade sur rue. Elle ne décrit pas un logement sur cour. Calcul : ting.ad (scripts/immeubles/bruit_risques.py).

**Colonnes**

| Colonne | Contenu |
|---|---|
| `id_ban` | identifiant de l'adresse dans la Base Adresse Nationale (clé d'interopérabilité) |
| `adresse` | libellé de l'adresse |
| `latitude`, `longitude` | point adresse BAN (WGS 84) |
| `logements_bdnb` | nombre de logements du groupe de bâtiments selon la BDNB |
| `route_lden_db` | bande Lden du bruit routier devant la façade : `moins de 55`, `55-59` … `75 et plus` |
| `route_ln_db` | bande Ln (nuit) : `moins de 50`, `50-54` … `70 et plus` |
| `route_lden_valeur_limite_depassee` | 1 si l'adresse est dans la zone de dépassement Lden 68 dB(A) de la carte de type C, sinon 0 |
| `route_ln_valeur_limite_depassee` | 1 si dépassement de la valeur limite de nuit (Ln 62 dB(A)), sinon 0 |
| `fer_sncf_lden_db` | bande Lden du réseau SNCF dans un rayon de 12 m |
| `fer_sncf_valeur_limite_depassee` | 1 si dépassement d'une valeur limite ferroviaire SNCF (Lden 73, Ln 65), sinon 0 |
| `ratp_lden_db_max` | bande Lden la plus forte du réseau RATP |
| `ratp_lignes` | lignes RATP concernées (par exemple `L6`, `T3A`) |
| `ratp_valeur_limite_depassee` | 1 si dépassement d'une valeur limite pour une ligne RATP, sinon 0 |
| `bruitparif_indice` | 1, 2 ou 3 |
| `bruitparif_classe` | `préservé`, `altéré`, `très dégradé` |

**Jeux sources à associer** :

- <https://www.data.gouv.fr/datasets/lot-de-donnees-relatives-aux-cartes-de-bruit-strategiques-pour-paris>
- <https://www.data.gouv.fr/datasets/base-adresse-nationale>
- <https://www.data.gouv.fr/datasets/base-de-donnees-nationale-des-batiments>

## Jeu 3 : Anciennes carrières et zones inondables, par immeuble, Paris 15e

- **Fichier** : `risques-carrieres-inondation-par-immeuble-paris-15e.csv` (4 756 lignes).
- **Licence** : **ODbL**, obligatoire. Les couches sources de la Ville de Paris (PLU bioclimatique, caves inondées 1910) sont sous ODbL, et une base qui en dérive doit être partagée sous la même licence. **Ne pas le publier sous Licence Ouverte.**
- **Mots-clés** : risques, carrieres, inondation, ppri, plu, paris-15e

**Description** :

> Pour chacun des 4 756 immeubles d'habitation du 15e arrondissement de Paris, le fichier indique :
> - si son point adresse est dans un secteur d'anciennes carrières souterraines ;
> - dans quelle zone du plan de prévention du risque d'inondation (PPRI) de Paris il se trouve ;
> - s'il est dans la zone des caves inondées par une crue de type 1910.
>
> Sources :
> - Ville de Paris, PLU bioclimatique approuvé le 20 novembre 2024, secteurs de risque « carrières » et « inondations », ODbL ;
> - Ville de Paris, zones des caves inondées en 1910, ODbL ;
> - Base Adresse Nationale.
>
> Base de données dérivée publiée sous ODbL. Calcul : ting.ad (scripts/immeubles/bruit_risques.py). Indication d'exposition, pas un état des risques réglementaire : pour une vente ou une location, l'état des risques officiel reste obligatoire.

**Colonnes** :

- `id_ban`, `adresse`, `latitude`, `longitude` : comme dans le jeu 2 ;
- `secteur_carrieres` : 1 ou 0 ;
- `ppri_zones` : codes de zone du PPRI, séparés par une espace (vide si hors zone) ;
- `caves_inondees_1910` : 1 ou 0.

**Jeux sources à associer** :

- <https://www.data.gouv.fr/datasets/plu-bioclimatique-secteurs-de-risque-carrieres>
- <https://www.data.gouv.fr/datasets/plu-bioclimatique-secteurs-de-risque-inondations>
- <https://www.data.gouv.fr/datasets/zones-des-caves-inondees-1910>

---

## Publier sur data.gouv.fr (pas à pas, à la main)

1. Créer l'organisation « ting.ad » depuis le compte data.gouv.fr : menu du profil → « Créer une organisation ». Y mettre le logo, une description et le site <https://ting.ad>. Une organisation se fait certifier en écrivant à l'équipe data.gouv, ce qui n'est utile que si l'on publie régulièrement.
2. « Publier un jeu de données » : titre, description et licence (ci-dessus), fréquence, couverture « Paris 15e arrondissement », période 2021-2025.
3. Ajouter le CSV comme ressource principale, au format `csv`. data.gouv génère un aperçu et une API tabulaire.
4. Ajouter le lien vers la page ting.ad qui le montre comme **réutilisation** du jeu (étape séparée).
5. Un jeu par semaine. Commencer par le jeu 1, qui a le plus de demande.

Si les données d'une source changent (une nouvelle échéance des cartes de bruit, une mise à jour DVF), régénérer le fichier avec le script et le **remplacer** dans la même ressource. Ne pas créer un nouveau jeu.

## Kaggle, Hugging Face, GitHub (préparé, non publié)

Mêmes fichiers, même texte. Sur chaque plateforme, la description commence par la source et par le lien vers la page ting.ad.

- **Kaggle** : <https://www.kaggle.com/datasets> → « New Dataset ».
  - Titre : « Paris 15e apartment prices per street (DVF, 2021-2025) ».
  - Licence : « Etalab Open License 2.0 » n'est pas dans la liste Kaggle. Choisir « Other (specified in description) » et écrire « Licence Ouverte 2.0 (Etalab) » dans la description.
  - Le jeu 3 se déclare en « ODbL ».
- **Hugging Face** : créer un dépôt de type dataset `ting-ad/paris-15e-open-data` et y placer les trois CSV et une fiche `README.md`. En-tête YAML à mettre en tête de cette fiche :

  ```yaml
  ---
  license: other
  license_name: licence-ouverte-2.0-et-odbl
  license_link: https://www.etalab.gouv.fr/licence-ouverte-open-licence/
  language: [fr]
  pretty_name: Paris 15e — prix par rue, bruit et risques par immeuble (agrégats open data)
  size_categories: [1K<n<10K]
  tags: [real-estate, paris, open-data, noise, dvf]
  configs:
    - config_name: prix_par_rue
      data_files: prix-median-m2-par-rue-paris-15e-2021-2025.csv
    - config_name: bruit_par_immeuble
      data_files: exposition-bruit-par-immeuble-paris-15e.csv
    - config_name: risques_par_immeuble
      data_files: risques-carrieres-inondation-par-immeuble-paris-15e.csv
  ---
  ```

  Le corps de la fiche reprend les trois descriptions ci-dessus. Préciser la licence propre à chaque fichier.
- **GitHub** : CLAUDE.md retire GitHub **pour le code du site**, car le miroir GitHub → GitLab ne passe plus. Un dépôt de données séparé n'a rien à voir avec ce miroir, mais pour éviter toute confusion, préférer un **projet public GitLab** `thedesigner/ting-ad-open-data`, avec le même contenu que le dépôt Hugging Face. N'ouvrir un dépôt GitHub que si Axel le décide explicitement.

**Ne jamais publier** :

- les fichiers de travail (`<work>/`) ;
- `ventes-immeubles.json` : il contient de petits effectifs exacts ;
- une vente DVF à l'unité.
