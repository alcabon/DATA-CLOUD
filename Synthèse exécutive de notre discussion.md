Voici une synthèse exécutive de notre discussion, structurée pour être facilement lisible, mémorisable ou réutilisable pour une présentation.

### Synthèse Exécutive : Salesforce Data Cloud & Customer 360

---

### 1. Le Concept Fondamental : Vision vs Moteur

* **Customer 360 :** C'est la *vision* stratégique. L'objectif d'aligner toutes les applications Salesforce (Ventes, Service, Marketing) autour d'une vue unique du client pour briser les silos départementaux.
* **Data Cloud :** C'est le *moteur*. La plateforme de données (Customer Data Platform / Data Lakehouse) hyperscale en temps réel qui rend cette vision techniquement possible.

### 2. L'Architecture Technique (Le Data Lakehouse)

Data Cloud n'est pas une base de données relationnelle classique, c'est un **Data Lakehouse** hébergé sur le cloud public (Hyperforce / AWS).

* **Les Standards Ouverts :** Le stockage physique se fait en **Apache Parquet** (format colonne hyper-compressé) et la gestion des transactions est assurée par **Apache Iceberg**.
* **La Révolution "Zero Copy" :** Grâce à ces standards, Data Cloud peut lire les données d'entrepôts externes (comme **Snowflake**, **Databricks** ou **Google BigQuery**) directement là où elles se trouvent, sans jamais utiliser d'ETL ni dupliquer la donnée.
* **Data Spaces :** Des partitions *logiques* au sein de l'instance globale permettant d'isoler les données et les processus par région, marque ou modèle d'affaires (ex: séparation stricte B2B et B2C) pour garantir la sécurité et la gouvernance.

### 3. Le Cycle de Vie de la Donnée (Les 5 Piliers)

La valeur de Data Cloud se crée à travers ce pipeline de traitement :

1. **Connecter (Ingestion) :** Aspiration des données via connecteurs natifs ou via l'**Ingestion API** (Streaming/Bulk) pour créer les objets bruts (**DLO** - Data Lake Objects).
2. **Harmoniser (Modélisation) :** Mapping des DLO vers le modèle standard *Customer 360 Data Model* (**DMO**). Les jointures (PK/FK) se font à la lecture (*Schema-on-read*), sans bloquer l'ingestion en cas d'erreur.
3. **Unifier (Identity Resolution) :** Création du "Golden Record" (**Unified Individual**) grâce aux règles de Correspondance (qui est qui ?) et de Réconciliation (quelle donnée garder ?).
4. **Analyser (Insights & IA) :** Enrichissement du profil avec des scores calculés (CLTV, RFM) et des prédictions d'IA (via l'intégration **BYOM** avec Databricks ou SageMaker).
5. **Agir (Activation) :** Création de **Segments** (glisser-déposer ou via la **Query API** en SQL) envoyés vers des cibles externes (Marketing Cloud, Meta Ads), ou déclenchement de **Data Actions** (via des *Platform Events*) pour alerter le CRM en temps réel.

### 4. La Vraie Proposition de Valeur

> Data Cloud package des technologies open-source complexes (Iceberg, Trino) pour les rendre accessibles aux équipes métiers. Sa véritable force ne réside pas dans le stockage, mais dans **ses connecteurs natifs avec l'écosystème Salesforce** et sa capacité à transformer instantanément un calcul "Big Data" en une **action CRM concrète** (alerte commercial, modification de parcours client).

---

Avez-vous besoin que je transforme cette synthèse en **un plan de présentation diapositive par diapositive (Slide Deck)**, ou souhaitez-vous explorer un tout autre sujet technique aujourd'hui ?
