# DATA-CLOUD  (GEMINI 3)

L'écosystème Salesforce a beaucoup évolué ces dernières années, et il est vrai que la terminologie peut parfois sembler un peu dense.

En tant qu'expert, je vais vous démystifier **Salesforce Customer 360** et **Salesforce Data Cloud**, car bien qu'ils soient intimement liés, ils représentent deux concepts distincts : la *vision* et le *moteur*.

Voici une présentation claire et structurée de ces solutions.

---

### 1. Salesforce Customer 360 : La Vision

**Customer 360** n'est pas un produit unique que vous pouvez acheter et installer. C'est le nom donné à **l'ensemble du portefeuille de produits Salesforce** (Sales Cloud, Service Cloud, Marketing Cloud, Commerce Cloud, etc.) lorsqu'ils fonctionnent ensemble.

* **L'objectif :** Briser les silos entre vos départements.
* **Le concept :** Créer une "vue unique du client". Si un client appelle le service client parce que son colis est en retard, l'équipe commerciale et l'équipe marketing doivent le savoir instantanément pour éviter de lui envoyer un e-mail promotionnel inapproprié au même moment.

### 2. Salesforce Data Cloud : Le Moteur (Anciennement Genie / CDP)

Si Customer 360 est la voiture, **Data Cloud** est son moteur surpuissant. C'est la plateforme de données client (CDP - Customer Data Platform) hyperscale et en temps réel de Salesforce.

Historiquement, les données vivaient dans des bases séparées. Data Cloud résout ce problème en ingérant des volumes massifs de données (structurées et non structurées) provenant de *n'importe où* et en les rendant exploitables *en temps réel* par les applications Salesforce.

### 3. Comment fonctionne Data Cloud ? (Les 5 piliers)

Pour bien comprendre la puissance de Data Cloud, il faut regarder son cycle de traitement des données :

* **🔗 1. Connecter (Ingestion) :** Data Cloud se connecte à toutes vos sources de données. Cela inclut vos applications Salesforce, mais aussi des sources externes via des connecteurs natifs (Amazon Sagemaker, Snowflake, Google BigQuery, sites web, applications mobiles, systèmes ERP legacy).
* **🧩 2. Harmoniser (Modélisation) :** Toutes ces données arrivent dans des formats différents. Data Cloud les mappe vers un modèle de données standardisé (le *Customer Information Model*), pour qu'elles parlent toutes le même langage.
* **👤 3. Unifier (Résolution d'identité) :** C'est la magie du système. Il relie les points entre "l'adresse IP 123", "l'adresse e-mail jean.dupont@email.com" et "le profil Twitter @JeanD", pour créer un **Profil Client Unifié**.
* **🧠 4. Analyser et Prédire :** Une fois le profil unifié, l'intelligence artificielle de Salesforce (Einstein) entre en jeu. Elle peut calculer la valeur à vie du client (CLV), prédire le risque de désabonnement (churn), ou recommander la prochaine meilleure action (Next Best Action).
* **🚀 5. Agir (Activation) :** Les données ne servent à rien si elles ne sont pas actionnables. Vous pouvez envoyer ces profils ou des segments d'audience vers Marketing Cloud pour une campagne ciblée, vers Service Cloud pour alerter un agent, ou vers Tableau pour du reporting avancé.

---

### 4. La valeur ajoutée pour l'entreprise

Pourquoi les entreprises investissent-elles massivement dans Data Cloud pour réaliser leur Customer 360 ?

> **L'hyper-personnalisation en temps réel :** Vous ne réagissez plus à ce que le client a fait hier, mais à ce qu'il est en train de faire *maintenant* sur votre site web.
> **La réduction des coûts d'intégration :** Le "Zero Copy" avec des partenaires comme Snowflake ou Google Cloud permet de lire les données là où elles se trouvent, sans avoir à les déplacer ou à les dupliquer coûteusement.
> **Une IA plus intelligente :** L'intelligence artificielle n'est aussi bonne que les données qu'elle consomme. En nourrissant Salesforce Einstein (ou votre propre IA) avec des données harmonisées et unifiées, vos prédictions deviennent redoutables d'efficacité.

---

Souhaitez-vous que nous explorions **un cas d'usage spécifique** pour illustrer cela (par exemple, comment le Marketing et le Service Client collaborent grâce à Data Cloud) ou préférez-vous aborder **les aspects plus techniques de l'intégration des données** ?
