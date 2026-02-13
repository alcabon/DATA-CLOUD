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
