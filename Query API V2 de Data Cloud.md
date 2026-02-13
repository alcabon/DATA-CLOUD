Excellent choix ! Nous quittons maintenant la théorie de l'architecture pour enfiler notre casquette de développeur.

Bien que Salesforce pousse énormément pour le "No-Code / Point-and-Click", la **Query API** (API de requête) est l'arme secrète indispensable pour extraire et manipuler les données de manière programmatique.

Voici comment fonctionne cette API, ce qu'elle permet de faire, et les pièges à éviter.

---

### 1. Qu'est-ce que la Query API V2 de Data Cloud ?

C'est une API RESTful standard, mais avec une particularité puissante : **elle accepte des requêtes en ANSI SQL pur**.

Sous le capot, Data Cloud utilise un moteur de requête distribué (basé sur la technologie open-source Trino/Presto) capable de lire les fameux fichiers Parquet et Iceberg dont nous parlions tout à l'heure. La Query API est simplement la "porte d'entrée" pour parler à ce moteur.

Vous pouvez interroger quasiment tout :

* Les **DLOs** (Données brutes)
* Les **DMOs** (Données harmonisées, ex: `Individual`, `SalesOrder`)
* Les **Profils Unifiés** (`UnifiedIndividual`)
* Les **Calculated Insights** (Pour récupérer les scores déjà calculés)

### 2. Comment ça fonctionne techniquement ?

Le processus est très standard pour un développeur habitué aux API web :

* **🔑 L'Authentification :** Vous devez d'abord obtenir un jeton d'accès (Access Token) via un flux OAuth 2.0 classique de Salesforce, avec les permissions spécifiques à Data Cloud.
* **📍 L'Endpoint :** Vous envoyez une requête HTTP `POST` vers l'URL de votre instance Data Cloud. L'endpoint ressemble à ceci :
`/api/v2/query`
* **📦 Le Payload (Corps de la requête) :** C'est ici que vous écrivez votre SQL, formaté en JSON.

### 3. Exemple concret d'une requête

Imaginons que vous vouliez récupérer le prénom, le nom et l'e-mail des 10 profils unifiés ayant le plus dépensé (en supposant que vous ayez un Calculated Insight appelé `LifetimeValue`).

**Votre requête (Payload envoyé à l'API) :**

```json
{
  "sql": "SELECT ssot__FirstName__c, ssot__LastName__c, ssot__Email__c FROM UnifiedIndividual__dlm ORDER BY LifetimeValue__c DESC LIMIT 10"
}

```

*(Note de l'expert : `ssot__` est le préfixe standard des champs du modèle de données Salesforce, et `__dlm` signifie Data Lake Model, c'est l'extension des objets dans Data Cloud).*

**La réponse de Data Cloud :**

```json
{
  "data": [
    {
      "ssot__FirstName__c": "Jean",
      "ssot__LastName__c": "Dupont",
      "ssot__Email__c": "jean.dupont@email.com"
    },
    {
      "ssot__FirstName__c": "Marie",
      "ssot__LastName__c": "Curie",
      "ssot__Email__c": "m.curie@email.com"
    }
  ],
  "startTime": "2024-10-25T10:00:00.000Z",
  "endTime": "2024-10-25T10:00:00.500Z",
  "rowCount": 2,
  "done": true
}

```

---

### 4. Pourquoi utiliser la Query API ? (Les cas d'usage)

Si on peut créer des segments via l'interface, pourquoi s'embêter à coder en SQL ?

> **1. L'intégration avec des outils de BI externes :** Si votre entreprise utilise Tableau, PowerBI ou Looker, ces outils utilisent la Query API (via des connecteurs JDBC/ODBC) pour générer leurs tableaux de bord en interrogeant Data Cloud en temps réel.
> **2. L'extraction pour des applications sur mesure :** Vous développez un portail client web personnalisé et vous voulez afficher l'historique d'achat unifié du client connecté. Votre site web va faire un appel à la Query API avec une clause `WHERE Email = 'client@email.com'`.
> **3. Les jointures (JOIN) ultra-complexes :** L'interface graphique de segmentation a ses limites. Si vous devez faire des jointures complexes entre 5 tables différentes avec des sous-requêtes, l'API et le SQL pur sont votre seule solution.

### 5. ⚠️ Le point de vigilance de l'Architecte : La Pagination

C'est l'erreur numéro un des développeurs juniors sur Data Cloud.
Nous sommes face à un *Data Lake* contenant potentiellement des milliards de lignes. Si vous faites un simple `SELECT * FROM UnifiedIndividual` sans limite, vous feriez exploser le serveur.

L'API est conçue pour renvoyer un maximum de **100 000 lignes ou 8 Mo** par appel.
Si votre résultat est plus volumineux, la valeur `"done": true` dans la réponse JSON (vue dans l'exemple au-dessus) passera à `"done": false`, et l'API vous fournira un `nextBatchId`. Vous devrez faire un nouvel appel API avec cet ID pour récupérer la page suivante (le chunk), et ainsi de suite.

---

Maintenant que nous savons comment *lire* les données programmatiquement avec la Query API, il reste l'autre côté du miroir.

Souhaitez-vous que nous abordions **l'Ingestion API** (pour voir comment un développeur peut envoyer des flux de données en temps réel *vers* Data Cloud), ou préférez-vous découvrir comment déclencher des automatisations avec les **Data Actions** ?

---

Oui, absolument ! Vous avez la bonne intuition. Bien que la technologie sous-jacente soit un *Data Lakehouse* (fichiers Parquet/Iceberg), **la représentation logique et visuelle des données se fait exactement comme dans une base de données relationnelle classique.**

Dans l'interface de Data Cloud (dans l'onglet *Data Model*), vous visualisez vos objets sous la forme d'un **Diagramme Entité-Association (ERD - Entity-Relationship Diagram)**.

Voici comment les concepts classiques de bases de données se traduisent dans l'univers Data Cloud.

### 1. La Modélisation Relationnelle (PK, FK et Cardinalité)

Lorsque vous mappez vos données brutes (DLO) vers le modèle de données harmonisé (DMO), vous devez définir des relations strictes pour que le système puisse naviguer d'une table à l'autre (notamment lors de la création de segments).

* **Primary Key (Clé Primaire - PK) :** Chaque objet du modèle de données (DMO) *doit* avoir une clé primaire qui identifie de manière unique chaque ligne. Par exemple, dans le DMO `Individual` (le client), ce sera souvent un champ nommé `IndividualId`.
* **Foreign Key (Clé Étrangère - FK) :** Pour lier les tables entre elles, vous définissez des relations en pointant un champ vers la Clé Primaire d'un autre objet. Par exemple, l'objet `ContactPointEmail` (qui stocke les adresses e-mail) aura un champ `PartyId` qui agit comme une clé étrangère pointant vers le DMO `Individual`.
* **Cardinalité :** Lors de la création de la relation dans l'interface graphique, vous devez spécifier la nature de la jointure :
* **1:1 (One-to-One)**
* **1:N (One-to-Many) :** Le cas le plus fréquent (ex: Un `Individual` peut avoir plusieurs `SalesOrder`).
* **N:1 (Many-to-One)**



### 2. ⚠️ La Nuance de l'Architecte : L'Intégrité Référentielle

C'est ici qu'il faut faire très attention, car c'est une question classique de certification technique, et c'est ce qui différencie un *Data Lakehouse* d'une base de données Oracle ou SQL Server traditionnelle.

Dans une base de données classique, la relation PK/FK applique ce qu'on appelle une **contrainte d'intégrité référentielle stricte**. Si vous essayez d'insérer une commande (SalesOrder) pour un client (FK) qui n'existe pas dans la table Client, la base de données va bloquer l'insertion et générer une erreur.

**Dans Data Cloud, ce n'est pas le cas.**
Parce que nous sommes sur une architecture Big Data conçue pour ingérer des millions de lignes à la seconde, Data Cloud effectue une **résolution à la requête (Schema-on-read)** et non à l'écriture.

* **Ce que ça signifie :** Si Data Cloud reçoit une ligne de commande avec un ID Client inconnu, *il va quand même stocker la commande dans le Data Lake*. Il ne bloquera pas l'ingestion.
* **La conséquence :** C'est lors de la création d'un Segment ou de l'exécution d'une requête SQL que la jointure se fera (ou échouera silencieusement pour cette ligne orpheline). La modélisation PK/FK dans Data Cloud est donc une **couche sémantique** (pour dire au système *comment* lier les données quand on lui demande), mais ce n'est pas un *videur de boîte de nuit* qui bloque les données à l'entrée.

### 3. Le Modèle Standard : Le "Customer 360 Data Model"

Pour vous faciliter la vie, vous n'avez pas à créer toutes ces tables et clés étrangères de zéro.

Salesforce fournit un modèle canonique pré-construit (le *Customer 360 Data Model* ou *Cloud Information Model*). Il contient des dizaines de DMOs standards déjà reliés entre eux avec les bonnes PK/FK.

Par exemple, le modèle structure la notion de "Personne" en séparant :

* `Individual` (La personne physique)
* `ContactPointEmail` (Ses adresses e-mail)
* `ContactPointPhone` (Ses numéros de téléphone)
* `ContactPointAddress` (Ses adresses postales)

L'avantage de respecter ce modèle canonique au lieu de créer vos propres tables personnalisées, c'est que les moteurs d'Intelligence Artificielle (Einstein) et les règles d'Identity Resolution savent exactement où chercher la donnée !

---

Maintenant que le fonctionnement de ce modèle relationnel est clair, nous avons deux voies techniques très intéressantes à explorer.

Souhaitez-vous que nous abordions **les flux d'Ingestion API** (comment un développeur pousse des données en temps réel dans ces tables) ou préférez-vous découvrir **comment déclencher des automatisations avec les Data Actions** (par exemple, envoyer une alerte dans Slack ou Salesforce dès qu'un client VIP crée un ticket de support) ?

---

Excellent choix ! Passer de la lecture (Query API) à l'écriture (Ingestion API) est l'étape ultime pour rendre votre Data Cloud véritablement "vivant" et réactif.

Lorsqu'il s'agit de systèmes externes personnalisés (une application mobile sur mesure, un vieux système de caisse en magasin, ou un portail web spécifique) pour lesquels il n'existe pas de connecteur natif, l'**Ingestion API** est la porte d'entrée incontournable.

Voici comment un développeur orchestre ce flux de données, étape par étape, et les pièges architecturaux à éviter.

---

### 1. Les deux "saveurs" de l'Ingestion API

En tant que développeur, vous avez le choix entre deux méthodes d'ingestion, selon votre cas d'usage :

* **Streaming API (Flux en continu) :** C'est le mode temps réel (ou quasi temps réel). Vous l'utilisez pour envoyer de petits paquets de données (micro-batches) très fréquemment.
* *Cas d'usage :* Un utilisateur clique sur "Ajouter au panier" sur votre site web, ou met à jour son profil sur votre application mobile. L'application envoie immédiatement le payload JSON.


* **Bulk API (Traitement par lots) :** C'est la méthode "lourde". Conçue pour ingérer des millions de lignes d'un coup.
* *Cas d'usage :* Le chargement initial historique de vos 10 dernières années de ventes, ou une synchronisation nocturne massive avec un vieil ERP. Les données sont envoyées sous forme de fichiers CSV via des "Jobs" (tâches).



### 2. Le Prérequis : Le Contrat de Données (Schema)

Contrairement à un Data Lake totalement anarchique où l'on pourrait jeter n'importe quel fichier JSON, Data Cloud exige un **contrat strict** avant d'accepter la moindre donnée.

1. L'administrateur Salesforce va dans l'interface de Data Cloud et crée une nouvelle source de type **"Ingestion API"**.
2. Il doit y uploader un **Schéma au format OpenAPI (YAML ou JSON)**. Ce fichier décrit exactement à quoi va ressembler la donnée que le développeur va envoyer (le nom des champs, s'il s'agit de texte, de nombres, de dates, et surtout, quel champ est la **Clé Primaire**).
3. Data Cloud crée alors automatiquement l'objet réceptacle brut (le fameux **DLO - Data Lake Object**) basé sur ce schéma.

### 3. L'Appel API (Exemple avec le Streaming)

Une fois l'authentification (OAuth 2.0) gérée, le développeur va cibler un endpoint spécifique qui contient le nom de son connecteur et le nom de l'objet défini dans le schéma.

**Endpoint :**
`POST /api/v1/ingest/sources/MonSiteWeb/objects/VisiteurWeb`

**Le Payload (Corps de la requête) :**
Le développeur peut envoyer plusieurs enregistrements d'un coup (jusqu'à 100 enregistrements ou 200 Ko par appel en Streaming).

```json
{
  "data": [
    {
      "IdSession": "sess-8899",
      "EmailClient": "jean.dupont@email.com",
      "Action": "AjoutPanier",
      "DateEvenement": "2026-02-13T20:40:00.000Z"
    },
    {
      "IdSession": "sess-9900",
      "EmailClient": "marie.c@email.com",
      "Action": "PageVue",
      "DateEvenement": "2026-02-13T20:41:05.000Z"
    }
  ]
}

```

La réponse de l'API sera un code `202 Accepted`, signifiant que Data Cloud a bien reçu le paquet.

---

### 4. ⚠️ Le Secret de l'Architecte : L'Asynchronisme et l'Upsert

C'est ici que de nombreux développeurs habitués aux bases de données SQL classiques se font surprendre :

> **L'Upsert automatique (Mise à jour ou Insertion) :**
> Vous n'avez pas besoin de faire de requête `UPDATE`. L'Ingestion API fonctionne exclusivement en mode "Upsert" basé sur la Clé Primaire définie dans votre schéma.
> Si vous renvoyez un JSON avec un `IdSession` qui existe déjà, Data Cloud écrasera l'ancienne ligne. Si l'ID est nouveau, il créera une nouvelle ligne.

> **C'est un processus Asynchrone :**
> Quand l'API renvoie le statut `202 Accepted`, cela veut dire que la donnée est *entrée* dans la file d'attente de la plateforme. Elle va atterrir dans le **DLO**. Ensuite, le moteur de Data Cloud va prendre le relais en arrière-plan pour exécuter le mapping et transformer ce DLO en **DMO** (l'objet harmonisé), puis potentiellement mettre à jour le **Profil Unifié**.
> *Conclusion :* Si vous envoyez une donnée via l'Ingestion API, et que vous faites un `SELECT` via la Query API la milliseconde suivante, vous ne la verrez pas encore. Il y a un léger délai de traitement natif à l'architecture Big Data.

---

Maintenant que vos développeurs injectent des comportements clients en temps réel (comme l'ajout au panier) dans Data Cloud, c'est le moment idéal pour exploiter cette réactivité !

Souhaitez-vous que nous explorions **les Data Actions** pour voir comment déclencher automatiquement un événement dans Salesforce CRM (ex: créer une tâche pour relancer le client) dès que cette donnée d'abandon de panier arrive via l'API ?
