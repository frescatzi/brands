---
type: raw
title: "POS-GENERIQUE_configuration-obsidian-ios-self-hosted"
source_url: "dropbox:/apps/lumina ai os/lumina vps/lumina ai/obsidian/lumina inbox/pos-generique_configuration-obsidian-ios-self-hosted.md"
captured: 2026-07-21
vault: brands
brand: obsidian
immutable: true
---

---
type: POS
title: "POS générique — Synchroniser Obsidian sur mobile via un serveur self-hosted (CouchDB + LiveSync)"
scope: générique
statut: actif
date: 2026-07-08
related:
  - "save-pos-md-to-drive"
---

# POS — Synchroniser Obsidian sur mobile via CouchDB self-hosted + plugin LiveSync

## Contexte

Objectif : synchroniser un coffre Obsidian entre plusieurs appareils (desktop, mobile) sans passer par Obsidian Sync (service officiel payant), en gardant le contrôle total de l'infrastructure (« self-hosted »).

Plusieurs approches ont été évaluées : synchronisation via Git (plugin Obsidian Git + app tierce comme Working Copy ou GitSync.md sur iOS), Syncthing + Möbius Sync (peer-to-peer), et **CouchDB self-hosted + plugin « Self-hosted LiveSync »**. Cette dernière est retenue comme solution de référence : c'est la seule pensée nativement pour ce cas d'usage (pas de contournement d'une app tierce), avec une vraie gestion de conflits multi-appareils, intégrée directement dans Obsidian (aucune app de sync séparée à ouvrir sur mobile).

| Solution | Fiabilité | Intégration | Effort |
|---|---|---|---|
| **CouchDB + Self-hosted LiveSync** | Élevée, gestion de conflits native | Dans Obsidian directement | Déploiement CouchDB + config initiale |
| Git + app tierce (Working Copy/GitSync.md) | Correcte mais plugin Git mobile instable | Nécessite une 2e app | Moyen à élevé |
| Syncthing + Möbius Sync | Bonne sauf en tâche de fond sur iOS | App tierce séparée | Moyen |

## Prérequis

- Un serveur/VPS/PaaS self-hosted pouvant exposer un conteneur Docker en HTTPS avec certificat valide (ex. Coolify, Dokploy, ou tout hébergeur avec reverse proxy + Let's Encrypt automatique).
- Obsidian installé sur chaque appareil à synchroniser.

## 1. Déployer CouchDB

Image officielle `couchdb:latest`, avec :
- Variables d'environnement `COUCHDB_USER` et `COUCHDB_PASSWORD` (créent l'administrateur — **uniquement pris en compte au tout premier démarrage sur un volume de données vide**, voir piège ci-dessous).
- Volumes persistants sur `/opt/couchdb/data` et `/opt/couchdb/etc/local.d`.
- Domaine HTTPS avec certificat valide (obligatoire pour qu'Obsidian iOS accepte la connexion).

⚠️ Sur certaines plateformes (Coolify notamment), une image de base de données connue peut être classée par défaut comme « database » plutôt que « application », ce qui masque le champ Domaine/HTTPS (seul un port TCP brut est proposé). Chercher une option du type **« Convert to Application »** si le champ Domaine n'apparaît pas.

## 2. Initialiser CouchDB (script officiel du plugin)

```bash
curl -s https://raw.githubusercontent.com/vrtmrz/obsidian-livesync/main/utils/couchdb/couchdb-init.sh \
  | hostname=https://<votre-domaine> username=<user> password='<password>' bash
```

Configure CORS, le mode single-node, la taille max des documents, et crée les bases système. Vérifier avec :
```bash
curl -u <user>:'<password>' https://<votre-domaine>/
```
→ doit répondre `{"couchdb":"Welcome",...}`.

## 3. Une base CouchDB par vault — règle absolue

CouchDB peut héberger plusieurs bases sur un seul serveur. **Chaque vault Obsidian doit avoir sa propre base, jamais partagée avec un autre vault** — sinon leurs contenus se mélangent côté serveur (et potentiellement en local à la synchronisation suivante). Nommer les bases en minuscules, sans espace ni caractère spécial (autorisé : lettres, chiffres, `-`, `_` sauf en première position).

Pour créer une base :
```bash
curl -u <user>:'<password>' -X PUT https://<votre-domaine>/<nom-de-base>
```

## 4. Connecter un appareil (plugin Self-hosted LiveSync)

1. Réglages → Plugins communautaires → installer **« Self-hosted LiveSync »** (auteur : vrtmrz).
2. Wizard de bienvenue :
   - **Premier appareil pour ce vault** → « I am setting this up for the first time ».
   - **Appareil suivant** → « I am adding a device to an existing synchronisation setup ».
3. Choisir **« Enter the server information manually »** (fiable partout ; la Setup URI/QR code peuvent échouer selon le contexte, voir pièges).
4. Renseigner URI (sans port si le reverse proxy termine en HTTPS standard), username, password, et le **nom de base dédié à ce vault**. Cocher **« Use Internal API »** en cas d'erreur CORS.
5. Cliquer **« Detect and Fix CouchDB Issues »** — doit afficher « All checks passed successfully! ».
6. À l'étape de récupération des données, pour un premier appareil ou un serveur neuf :
   - **« Compare time and take newer »** (jamais « Overwrite all with remote files » si le serveur est vide).
   - **« Keep local files even if deleted on remote »** (jamais « Delete local files if deleted on remote » dans le même cas).

## Pièges rencontrés

- **`COUCHDB_USER`/`COUCHDB_PASSWORD` ne s'appliquent qu'au tout premier démarrage** sur un volume vide. Changer ces variables plus tard et redéployer ne suffit pas : il faut vider les volumes persistants puis redéployer pour qu'un nouveau mot de passe soit pris en compte.
- CouchDB **verrouille temporairement le compte** après plusieurs échecs d'authentification rapprochés — attendre quelques minutes plutôt que retenter en boucle.
- **Caractères spéciaux dans le mot de passe** (`$`, etc.) : sans danger dans un champ de formulaire, mais risqués tels quels dans un terminal (interprétation shell) — toujours entourer de guillemets simples en ligne de commande.
- **`Access forbidden` persistant malgré des identifiants vérifiés bons** (ex. via curl direct) → cocher **« Use Internal API »** dans les réglages du plugin, qui contourne un blocage CORS propre au contexte navigateur/Electron.
- **Presse-papier isolé** dans certains environnements conteneurisés/navigateur (ex. bureau distant type noVNC) : la fonction « copier la Setup URI » ne se colle pas forcément sur un autre appareil physique — préférer la saisie manuelle, ou le QR code pour mobile (scan caméra, insensible à ce problème de presse-papier).
- **Vérifier qu'aucun autre vault ne partage déjà le nom de base** avant de valider la configuration — une confusion entre deux vaults distincts pointant sur la même base peut mélanger leurs contenus.

## Bonnes pratiques une fois configuré

- Un seul mécanisme de synchronisation actif par appareil à la fois (éviter de faire tourner LiveSync et un plugin Git en parallèle sur le même dossier sans discipline claire sur qui pousse quoi).
- Si le coffre est aussi un dépôt Git par ailleurs, le commit/push Git reste un geste séparé — LiveSync ne se substitue pas à Git comme source de vérité versionnée, il ne fait que garder les fichiers synchronisés en direct entre appareils.
- Activer le chiffrement de bout en bout (E2EE) du plugin comme étape contrôlée séparée, une fois la synchronisation de base confirmée stable.

## Sources consultées

- [GitHub — vrtmrz/obsidian-livesync](https://github.com/vrtmrz/obsidian-livesync)
- [obsidian-livesync — setup_own_server.md](https://github.com/vrtmrz/obsidian-livesync/blob/main/docs/setup_own_server.md)
- [Coolify Docs — Docker Compose](https://coolify.io/docs/knowledge-base/docker/compose)
- [Coolify Docs — Domains](https://coolify.io/docs/knowledge-base/domains)
