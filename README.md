<p align="center">
<img src="./docs/images/logo.jpg" height="200">
</p>

L’API Partenaires permet à des logiciels partenaires ainsi que des cabinets membres de récupérer et d'envoyer de l'information depuis/vers MyUnisoft.

L’authentification du partenaire/cabinet est principalement basé sur:
- une clé **x-third-party** fournie par MyUnisoft (demande auprès de [c.mandrilly@myunisoft.fr](c.mandrilly@myunisoft.fr)).
- une clé [JWT](https://jwt.io/) (**API Token**) pour chaque cabinet et/ou société.

Ces deux clés sont nécessaires pour pouvoir utiliser les routes définies sur [https://docs.api.myunisoft.fr/](https://docs.api.myunisoft.fr/)

# Equipe 👥

| Prénom - Nom | Rôle(s) | Email |
| --- | --- | --- |
| Cyril Mandrilly | CTO | [c.mandrilly@myunisoft.fr](c.mandrilly@myunisoft.fr) |
| Thomas Gentilhomme | Lead Développeur API | [gentilhomme.thomas@gmail.com](gentilhomme.thomas@gmail.com) |
| Alexandre Malaj | Développeur API | [alexandre.malaj@gmail.com](alexandre.malaj@gmail.com) |
| Léon Souvannavong | Lead dev back-end (**a consulter pour la partie métier**) | [l.souvannavong@myunisoft.fr](l.souvannavong@myunisoft.fr) |

# Type d'accès 🔬
Notre API partenaires permet deux types distincts d'accès:

🔸 Un accès restreint a une **société** (dossier) d'un cabinet.

> L'accès limité par société est le modèle le plus courant car il permet d'interconnecter nos solutions de manière permanente par le biais d'un jeton n'ayant aucune date d'expiration (il peut être néanmoins révoqué par le gestionnaire du dossier ou par nos équipes techniques).
>
> C'est un modèle qui est aussi très flexible car nous n'avons pas à intervenir dans le processus de connexion. [Plus d'informations ici](./docs/connector.md).

🔹 Un accès à l'intégralité d'un **cabinet**.

> Un accès **cabinet** delivera un jeton ayant une durée de vie très courte pour garantir une meilleure sécurité des données appartenant au cabinet. 

# Prérequis 👀

Les éléments et informations que le partenaire (ou cabinet) doit nous fournir (mail a [c.mandrilly@myunisoft.fr](c.mandrilly@myunisoft.fr) ou slack si déjà invité.).

## 🔸 Accès par société

Ces éléments permettront de créer le connecteur sur l’application MyUnisoft et de vous envoyer les informations techniques: 

- nom partenaire.
- description courte partenaire (3 lignes 25 char maximum).
- description longue.
- logo partenaire (png, hauteur 50px).
- texte complémentaire (par exemple ou coller la clé sur votre interface ou lien vers une doc/vidéo d’utilisation avec myunisoft)
- nom, prénom, email pour un accès à myunisoft.
- nom, prénom, email pour une invitation slack.

## 🔹 Accès cabinet

> ⚠️ **PAS ENCORE DISPONIBLE**.

---

Les éléments que nous renvoyons au partenaire une fois les éléments ci-dessus en notre possession:

- Clé **x-third-party** (C’est une clé secrète unique entre vous et nous qui sera nécessaire pour requêter l’API).
- Un compte au sein d'un schéma dédié aux intégrations partenaires.
- Lien vers la documentation **postman** ([https://docs.api.myunisoft.fr/#intro](https://docs.api.myunisoft.fr/#intro)).

# Liens racine de nos API 🌍

- API Partenaires: [https://app.myunisoft.fr/api/v1](https://app.myunisoft.fr/api/v1)
- Service Auth: [https://app.myunisoft.fr/api/authenticate](https://app.myunisoft.fr/api/authenticate)

# Authentification 🔐

Les sous-documentations suivantes vous guideront dans le flow d'authentification nécessaire selon le type d'accès que vous avez souhaité.

[🔸 Accès par société](./docs/auth/societe.md)
> ⚠️ Dans le cadre **d'un accès société** l'authentification n'est nécessaire **que pour la phase de développement** du connecteur! Plus [d'informations ici](./docs/connector.md).


[🔹 Accès cabinet](./docs/auth/cabinet.md)

# Utilisation d’une route exposée par l’API 🚀

Lors de l’utilisation d’une route exposée il est nécessaire d’avoir l’**API Token** en [Bearer token](https://swagger.io/docs/specification/authentication/bearer-authentication/) dans l'en-tête **Authorization** (et surtout pas le jeton Utilisateur).

Il est aussi nécessaire d’ajouter une en-tête “**X-Third-Party-Secret**” contenant la clé secrète communiqué par notre équipe.

```bash
$ curl --location --request GET 'https://api.myunisoft.fr/api/v1/vat_param' \
--header 'X-Third-Party-Secret: nompartenaire-L8vlKfjJ5y7zwFj2J49xo53V' \
--header 'Authorization: Bearer {{API_TOKEN}}'
```

Pour plus d'informations nous vous invitons à consulter les sous documentations suivantes selon la nature de votre accès:

- [🔸 Accès par société](./docs/endpoints/societe.md)
- [🔹 Accès cabinet](./docs/endpoints/cabinet.md)

## Rate-limit des routes exposées

L'API limite le nombre de requêtes par API Token, quelques en-têtes supplémentaires sont envoyés dans la réponse HTTP:

- **X-Rate-Limit-Remaining** (le nombre de requêtes restantes dans la période).
- **X-Rate-Limit-Reset** (timestamp correspondant au moment où la période sera réinitialisée).
- **X-Rate-Limit-Total** (le nombre total de requêtes pour une période).

La limite par **défaut est de 100 requêtes par minute**.

# Guides supplémentaires 📌

Une liste de guides qui pourront certainement vous aider dans la réalisation de l'interconnexion.

- [Collection + Environment postman](./postman/README.md) (voir le dossier /postman à la racine du github pour les fichiers .JSON)
- [Création d'une entrée comptable avec le format JSON](./docs/entry_json.md)
- [Création d'une entrée comptable avec le format TRA+PJ](./docs/entry_tra.md)
- [Gestion des retours erreurs](./docs/erreurs.md)

Exemple d'implémentation sur différents langages de programmation:
- [Node.js](./exemples/nodejs/README.md)

> 💡 Votre language est manquant ? N'hésitez pas à ouvrir une pull-request pour l'ajouter!
