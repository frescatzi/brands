---
type: wiki
title: "Operator OS — déploiement Coolify (GitHub privé, Basic Auth, synchro base)"
status: draft
publish: none
vault: brands
brand: AFTRSN
sources: [AFTRSN/raw/2026-08-17--pos-exact-operator-os-deploiement-coolify-basicauth-synchro-2026-08-17.md]
related: [operator-os-dashboard-mesure, architecture-processus-metier, runbook-memoire-centrale-asp-memory-et-agents-aftrsn, hermes-agent-deploiement-coolify]
updated: 2026-08-20
---

# Operator OS — déploiement Coolify (GitHub privé, Basic Auth, synchro base)

Chaîne de déploiement de l'Operator OS sur `luminaos.aftersunpeople.com`, via Coolify, depuis un repo GitHub privé. Pour ce que le dashboard mesure et comment il est construit, voir [[operator-os-dashboard-mesure]] — cette page ne couvre que la mise en ligne et son exploitation. Statut : verrouillé, validé live le 17.08.2026.

## Chaîne de déploiement

1. **Code sur GitHub privé** — repo `frescatzi/luminaos` : `Dockerfile`, `app/`, `collector/`, `config.json`, `docker-compose.yml`. Les secrets sont exclus via `.gitignore`.
2. **Ressource Coolify** — type « Private Repository » (GitHub App `luminaos-app`), Build Pack Dockerfile, port 8090.
3. **Domaine** — `luminaos.aftersunpeople.com`, DNS GoDaddy pointant vers `178.105.197.189` ; HTTPS géré par Coolify.

La règle qui gouverne toute la chaîne : **le build context doit contenir le code applicatif**. C'est la raison du passage par GitHub — Coolify télécharge le repo au moment du build. Un `docker-compose.yml` collé à la main dans l'UI ne suffit jamais.

## Variables d'environnement

| Variable | Rôle |
|---|---|
| `OPERATOR_OS_USER` | identifiant Basic Auth |
| `OPERATOR_OS_PASSWORD` | mot de passe Basic Auth |
| `OPERATOR_OS_DB` | chemin de la base, `/data/operator.db` |

`OPERATOR_OS_USER` et `OPERATOR_OS_PASSWORD` sont référencées dans le compose par `${...}` et fournies par Coolify — jamais versionnées. Sans ces variables, l'application refuse tout accès (503) : c'est le comportement voulu, pas une panne.

## Volume et synchronisation

- Volume `/data` persistant, contenant `operator.db`.
- Le collecteur tourne sur le VPS (cron Dream 7h), pas dans le conteneur.
- Synchro hôte → conteneur à 7h15 chaque matin.

Cette séparation collecteur / serveur est structurelle et détaillée dans [[operator-os-dashboard-mesure]] : le conteneur n'a pas accès aux `state.db` d'Hermès, il ne fait que servir l'API et lire la base qu'on lui pousse.

## Dépannage

| Symptôme | Cause réelle | Correctif |
|---|---|---|
| « no available server » puis 404 | DNS correct mais aucun conteneur déployé | relancer un déploiement complet |
| Build bloqué « Helper container not yet started » | disque VPS plein — le helper Coolify n'est jamais créé | nettoyer les volumes orphelins (`docker volume rm` sur les hash sans lien) puis `docker restart coolify` |
| Conteneur « Exited » | build context sans le code (compose collé à la main) | passer par le repo Git privé |

## Enseignements

- **Le build context doit contenir le code** : un compose collé à la main ne suffit jamais.
- **Disque plein = build Docker impossible.** Libérer l'espace avant de déployer, a fortiori sur un VPS à capacité limitée (38 Go).
- **Vérifier par l'URL publique réelle** (attendre un 200 ou un 401), jamais par le statut affiché dans l'UI Coolify.
- **Basic Auth est le minimum** pour exposer un dashboard interne sur un domaine public.

<!-- AUTO-LIENS:début -->
## Voir aussi
- [[operator-os-dashboard-mesure]] : Operator OS · architecture du dashboard de mesure
- [[architecture-processus-metier]] : AFTRSN · Architecture par processus métier
- [[runbook-memoire-centrale-asp-memory-et-agents-aftrsn]] : AFTRSN · Runbook mémoire centrale asp_memory & agents
- [[hermes-agent-deploiement-coolify]] : AFTRSN · Hermès Agent : déploiement Coolify (accès, dépannage)
<!-- AUTO-LIENS:fin -->
