# WealthSimpleBank — interface de simulation de portefeuille

Cette application est une interface de simulation : elle ne fournit aucun compte bancaire, titre financier, carte, RIB, virement ou retrait réel. Les cours, positions, coordonnées et mouvements affichés sont des données de simulation sans valeur monétaire. Aucune fonction de carte n’est disponible. La marque et l’interface ne doivent pas être présentées comme un établissement financier réel.

## Fonctionnement et limites financières

L’application fonctionne uniquement en `FINANCIAL_SERVICES_MODE=test`. Les soldes, cours, positions, crédits et virements sont des données de simulation sans valeur monétaire. Les demandes de RIB ou de carte sont stockées localement pour revue interne ; leur acceptation n’émet aucun document et ne contacte aucun prestataire. Ces limites sont indiquées dans les sections concernées ; aucun bandeau global n’est ajouté en haut des pages.

Le contrôle `/health` indique aussi le mode actif et les intégrations financières absentes. Le démarrage échoue si `FINANCIAL_SERVICES_MODE` est défini à une autre valeur que `test` : il n’existe actuellement aucun adaptateur bancaire, de paiement ou de données de marché, et cette variable ne permet pas d’activer un mode réel.

Une future intégration doit être conçue séparément, après sélection d’un fournisseur autorisé et validation de ses exigences : environnement sandbox et identifiants dédiés, adaptateur côté serveur, gestion des erreurs et rapprochements, tests d’intégration, revue de conformité/sécurité et activation de production explicite. Ne stocke aucun secret fournisseur dans le code ou dans ce dépôt. Tant que ces étapes ne sont pas terminées et vérifiées, conserver le mode test et ne pas présenter les données existantes comme réelles.

## S’y retrouver dans le projet

Ouvre le dossier du projet dans VS Code. Les fichiers utiles sont regroupés à la racine et les vues HTML séparées se trouvent dans `templates/`. Garde cette structure : Flask et Render cherchent `app.py`, `templates/`, `requirements.txt`, `Procfile` et `render.yaml` à la racine.

| Ce que tu veux modifier | Fichier / endroit |
| --- | --- |
| Inscription, texte et formulaire | `templates/register.html` |
| Portefeuille d’investissement pédagogique, graphique et boutons | `templates/demo_investments.html` |
| Codes de crédits dans l’administration | `templates/admin_demo_investment_codes.html` |
| Codes d’inscription dans l’administration | `templates/registration_codes.html` |
| Liste d’attente de l’administration | `templates/waiting_list.html` |
| Page d’accueil, tableau de bord et panneau admin | `app.py` : sections « PAGES HTML INTÉGRÉES » et « Affichage des pages ». Les grandes pages sont stockées encodées (`_INDEX_B64`, `_DASHBOARD_B64`, `_ADMIN_B64`) ; leurs ajustements sont dans `INDEX_HTML`, `DASHBOARD_HTML` et `ADMIN_HTML`. |
| Pages du site et vues admin | `app.py` : section « AFFICHAGE DES PAGES DU SITE ET DES VUES D’ADMINISTRATION » |
| Inscription, code de vérification et connexion | `app.py` : section « API — INSCRIPTION AVEC CODE, CONNEXION, DÉCONNEXION ET JETON CSRF » |
| Profil, portefeuille de simulation et virements | `app.py` : section « API — PROFIL CLIENT, PORTEFEUILLE FICTIF ET VIREMENTS » |
| Fonctions d’administration des comptes | `app.py` : section « API — ADMINISTRATION DES COMPTES, SOLDES, NOTIFICATIONS ET CODES » |
| Connexion à la base et création/mise à jour des tables | `app.py` : sections « CONNEXION À LA BASE DE DONNÉES » et « CRÉATION DES TABLES » |
| Dépendances Python | `requirements.txt` |
| Variables de configuration à copier comme modèle | `.env.example`; les vrais secrets locaux vont dans `.env`, qui ne doit pas être publié |
| Démarrage local | `app.py`; commande de lancement indiquée plus bas |
| Déploiement et base Render | `render.yaml` et `Procfile` |

**Pour retrouver rapidement un élément :** dans VS Code, fais `Ctrl+Shift+F` et cherche un mot exact. Cela fonctionne pour les fichiers HTML séparés, les routes et les remplacements existants dans `app.py`. Attention : le texte d’origine des pages d’accueil, tableau de bord et panneau admin est encodé, donc le chercher directement ne le retrouvera pas toujours. Ne modifie jamais les longues chaînes `_INDEX_B64`, `_DASHBOARD_B64` ou `_ADMIN_B64` manuellement. Pour changer un texte déjà ajusté par le code, repère la chaîne `.replace(...)` correspondante dans le bloc `INDEX_HTML`, `DASHBOARD_HTML` ou `ADMIN_HTML`. Pour modifier la mise en page ou un texte d’origine absent de ces remplacements, il faut d’abord extraire la page encodée dans un vrai fichier HTML; cette extraction n’a pas encore été faite. Les formulaires et le portefeuille sont déjà dans les fichiers HTML séparés indiqués ci-dessus.

### Repères dans `app.py`

Les titres en majuscules dans `app.py` indiquent les principales parties du programme :

- **Paramètres de l’application** : clé secrète, cookies et configuration générale.
- **Pages HTML intégrées** : accueil, tableau de bord et interface d’administration.
- **E-mails de notification** : paramètres SMTP et fonctions d’envoi.
- **Base de données** : connexion SQLite en local ou PostgreSQL en production, création des tables et migrations.
- **Protections** : limitation des tentatives, protection CSRF, en-têtes HTTP et contrôle d’accès.
- **Générateurs** : coordonnées de simulation, validation des montants et cours simulés.
- **Affichage des pages** : routes qui servent les pages du site et les vues administrateur.
- **API d’authentification** : démarrage et validation de l’inscription, connexion, déconnexion et jeton CSRF.
- **API utilisateur** : données du profil, portefeuille de simulation, virements et notifications.
- **API d’administration** : gestion des comptes, soldes, notifications et codes de simulation.
- **Lancement** : démarrage local ou en production.

Pour retrouver une section, cherche son titre avec `Ctrl+Shift+F` dans VS Code. Les numéros de ligne ne sont pas indiqués ici, car ils changent à chaque modification.

### Repères dans `templates/`

- `register.html` : formulaire d’inscription et vérification du code à 4 chiffres.
- `demo_investments.html` : page portefeuille, affichage du graphique, historique et libellés de ses boutons.
- `admin_demo_investment_codes.html` : création et liste des codes de crédit CRD.
- `registration_codes.html` : visualisation des codes et demandes d’inscription côté admin.
- `waiting_list.html` : liste d’attente historique côté admin.

Les dossiers `.venv/` (bibliothèques Python), `__pycache__/` (cache), `uploads/` (fichiers locaux) et la base `neobank.db` ne sont pas des fichiers de code à modifier pour changer l’interface. Ne publie ni `.venv/`, ni `neobank.db`, ni `.env`, ni données stockées dans `uploads/`.

## Déploiement
1. Pousse le projet sur GitHub, sans inclure `.venv/`, `neobank.db` ni aucun fichier `.env`. Conserve aussi les templates HTML requis sous `templates/` (`register.html`, `demo_investments.html`, `admin_demo_investment_codes.html`, `registration_codes.html` et `waiting_list.html`) : Render renverra une erreur 500 si ce dossier manque.
2. Sur Render, crée un Blueprint à partir du dépôt et applique `render.yaml`.
3. Renseigne `MAIL_USERNAME` et `MAIL_PASSWORD` dans Render avec les identifiants SMTP Gmail. Pour Gmail, utilise un mot de passe d'application.
4. Dans les variables d'environnement Render, définis `ADMIN_USERNAME` (adresse de connexion à `/admin`) et `ADMIN_PASSWORD` (un mot de passe fort d'au moins 12 caractères). Render génère `SECRET_KEY` et fournit `DATABASE_URL` depuis la base PostgreSQL déclarée dans le Blueprint.
5. Si un site externe doit appeler un endpoint compatible CORS, définis `CORS_ALLOWED_ORIGINS` avec ses origines exactes, séparées par des virgules (par exemple `https://exemple.test`). Par défaut, aucun appel cross-origin n'est autorisé.
6. Conserve `FINANCIAL_SERVICES_MODE=test`. La configuration Render le fixe explicitement ; aucune valeur ne permet actuellement d’activer des opérations financières réelles.

## Configuration Gmail en local
1. Copie `.env.example` dans un fichier `.env` à la racine du projet.
2. Renseigne `MAIL_USERNAME` avec ton adresse Gmail et `MAIL_PASSWORD` avec un mot de passe d'application Google (pas le mot de passe normal de ton compte).
3. Redémarre Flask après avoir modifié `.env`. Ce fichier est ignoré par Git et ne doit jamais être envoyé sur GitHub.

## Inscription et portefeuille de simulation
- L'inscription demande uniquement un nom, un prénom, une adresse e-mail, un numéro de téléphone et un mot de passe pour le profil de simulation.
- Après « Suivant », la demande et son code sont conservés. Si le client quitte puis revient dans les 24 heures, il ressaisit les mêmes informations et reprend le même code ; un nouveau code n'est généré qu'après expiration. Dans `/admin`, ouvre « Codes d’inscription » pour communiquer le code affiché.
- Le code n'est consommé qu'une seule fois, lors de sa validation réussie. Il expire après 24 heures et la demande est verrouillée après 5 codes incorrects. Après validation, l'utilisateur arrive au tableau de bord de simulation.
- Aucune pièce d'identité, date de naissance, ville/pays de résidence, donnée de paiement ou justificatif n'est demandé. Les demandes de RIB ou de carte sont enregistrées pour revue interne : leur validation n'émet aucun numéro ni moyen de paiement. Les soldes affichés sont des valeurs de simulation et ne constituent pas un compte bancaire.
- L'ancienne route `/activation` redirige vers le tableau de bord et l'API d'envoi de dossiers est désactivée. Les dossiers/images éventuellement enregistrés auparavant restent dans la base historique, mais les images ne sont plus accessibles via l'application; leur suppression définitive nécessite une opération distincte sur la base.
- Le lien « Portefeuille de simulation » (`/investments`) donne accès à un portefeuille pédagogique lié au solde principal de simulation en EUR. Les actions représentatives et les cours sont simulés; les graphiques sont illustratifs et ne reprennent aucune donnée de marché réelle ni ne promettent de rendement. Les anciennes positions crypto sont conservées pour consultation et clôture.
- Dans le panneau `/admin`, l’action « Retour investissement » permet d’attribuer un crédit de simulation à un client. Le montant est ajouté au solde principal et s’affiche dans la section « Retour sur investissement » avec son historique; il ne représente pas un bénéfice ou rendement financier réel.
- Le panneau `/admin` comporte une session globale des dépôts et retours, désactivée par défaut. Un administrateur doit l’activer pour créditer les comptes, créer des codes de dépôt ou permettre leur utilisation; son état est conservé dans la base de données.
- Le bouton « Retour investissement » d’un client crédite immédiatement le montant saisi sur son solde de simulation et enregistre, dans la même transaction, l’opération dans `investment_returns` et dans l’historique des mouvements. L’historique est consultable depuis « Retours investissement » dans le menu admin (`/admin/investment-returns`).
- Une migration idempotente convertit une seule fois les anciens crédits CRD vers le solde EUR de simulation à raison de 1 CRD pour 1 EUR, sans supprimer l'historique des positions.
- L'administration peut générer un code à usage unique depuis « Codes portefeuille » (`/admin/demo-investment-codes`). Le code est présenté comme un crédit de simulation, ajouté au solde principal et journalisé comme tel. Ce n'est jamais un dépôt d'argent.
- Dans le tableau de bord, « Demandes simulées » permet d'envoyer une demande de RIB ou de carte et un commentaire non sensible. L'administration peut lire et valider/refuser ces demandes depuis `/admin/service-requests`; une validation reste une revue interne et ne génère aucun document ou numéro. Ne collecter aucun mot de passe, numéro de carte ou identifiant bancaire dans les précisions.
- Les virements de simulation sont adressés par e-mail à un autre compte client. Ils apparaissent « En attente — simulation » avec une animation, sans débit du solde, crédit du destinataire, envoi d'e-mail ni transfert réel; une demande peut être annulée.
- Aucun retrait réel ou frais/certification pour débloquer un retrait n'est proposé dans cette interface.

Les pages d’accueil, tableau de bord et panneau d’administration sont encore assemblés depuis des sources HTML encodées dans `app.py`; les interfaces d’inscription, d’investissement et d’administration du portefeuille sont dans `templates/`.

## Admin
- Ouvre `/admin` et utilise l'adresse définie par `ADMIN_USERNAME` ainsi que `ADMIN_PASSWORD` dans Render > Environment. Au démarrage, le mot de passe haché en base est synchronisé depuis cette variable.
- Pour créer ou réinitialiser l'administrateur en local, exécute `.\.venv\Scripts\python.exe -m flask --app app create-admin`. La commande demande l'adresse puis le mot de passe deux fois sans l'afficher. Dans le Shell Render, exécute `flask --app app create-admin`.
- Sur Render, `ADMIN_PASSWORD` est la source de vérité au démarrage. Si tu réinitialises le compte avec la commande CLI, mets aussi `ADMIN_PASSWORD` à la même valeur dans les variables du service, sinon le mot de passe défini dans Render reprendra effet au prochain redémarrage.
- `MAIL_USERNAME` et `MAIL_PASSWORD` servent uniquement à la connexion SMTP Gmail pour l'envoi d'e-mails. Ils ne servent pas à se connecter à `/admin`. N'utilise pas `Admin1234!` ni le mot de passe de Gmail comme mot de passe administrateur.
- `FLASK_DEBUG` n'active le mode debug local que s'il est explicitement défini à `1` ou `true`. En production, ne l'active jamais.
