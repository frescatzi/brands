---
type: wiki
title: "Operator OS — architecture du dashboard de mesure"
status: draft
publish: none
vault: brands
brand: AFTRSN
sources: [AFTRSN/raw/2026-08-17--pos-exact-operator-os-architecture-dashboard-et-donnees-2026-08-17.md]
related: [operator-os-deploiement-coolify, architecture-processus-metier, runbook-memoire-centrale-asp-memory-et-agents-aftrsn, workflow-lumina-ai-router]
updated: 2026-08-19
---

# Operator OS — architecture du dashboard de mesure

Operator OS est le dashboard de mesure de la stack Hermès (LUMINA), déployé sur `luminaos.aftersunpeople.com`. Il n'a pas vocation à exécuter les agents : il agrège et affiche des métriques déjà produites ailleurs.

## Stack technique

- Backend : Python 3.13, FastAPI, uvicorn (port 8090).
- Stockage : SQLite (`operator.db`).
- Front : SPA vanilla JS + Chart.js, sans framework.
- Code source : `/opt/data/operator-os/`, modules `app/` (API + front) et `collector/` (agrégation).

## Les 4 vues et leurs sources

| Vue | Endpoint | Contenu | Source de données |
|---|---|---|---|
| Home | `/api/home` | Dépenses 28j/7j, sessions, ROI (taux horaire × minutes gagnées), coût par provider/modèle | `daily_usage`, agrégée depuis les 5 `state.db` Hermès |
| Skills | `/api/skills` | Catalogue de 243 skills, usages, taux de succès, valeur estimée | `skill_catalog` + `skill_usage` |
| Memory | `/api/memory` | Banques pgvector (`lumina_memory`, `aftrsn_memory`, `karteramaris_memory`), registre des skills, marques | webhook n8n `memory-dump` |
| Dream | `/api/dream` | 4 prescriptions classées sévérité × impact $, générées par un modèle Claude | table `dream_prescriptions` |

## Séparation collecteur / serveur

Règle d'or de l'architecture : le collecteur et le serveur ne partagent pas le même environnement d'exécution.

- Le **collecteur** (lecture des `state.db` Hermès, webhook mémoire) tourne sur le VPS via cron — jamais dans le conteneur Docker, qui n'a pas accès aux `state.db`.
- Le **conteneur** ne fait que servir l'API et lire `operator.db`, monté en volume sur `/data`.
- La base est synchronisée du VPS vers le conteneur chaque matin par un script hôte (`sync-operator.sh`, cron 7h15).

Cette séparation découle d'une contrainte physique : `/opt/data` n'existe pas nativement sur l'hôte, la base vivait dans le conteneur Hermès, et Docker ne supporte pas la copie directe conteneur-à-conteneur (`docker cp` A→B). La solution retenue passe par un fichier temporaire hôte (`docker cp hermes:... /tmp/` puis `docker cp /tmp/... lumina:/data/`), rejouée chaque matin par le cron.

## Sécurité

- Basic Auth via middleware (`OPERATOR_OS_USER` / `OPERATOR_OS_PASSWORD`) ; sans configuration, tout accès est refusé (503).
- Le port 8090 n'est jamais exposé publiquement, seul le domaine HTTPS via le proxy Coolify l'est.
- Les secrets sensibles (clés Anthropic, Postgres) restent dans n8n — jamais dans le code ni le repo.

Pour la chaîne de déploiement complète (Coolify, GitHub privé, variables d'environnement, dépannage), voir [[operator-os-deploiement-coolify]].

## Enseignements

- Toujours libérer le disque avant de lancer un build Docker sur un VPS à capacité limitée (38 Go) : un disque plein bloque le helper Coolify.
- Le build context Coolify doit contenir le code applicatif (repo Git), pas seulement le `docker-compose.yml`.
- Un déploiement se valide par l'URL publique réelle, jamais par le seul statut affiché dans l'UI Coolify.

<!-- AUTO-LIENS:début -->
## Voir aussi
- [[operator-os-deploiement-coolify]] : Operator OS · déploiement Coolify (GitHub privé, Basic Auth, synchro base)
- [[architecture-processus-metier]] : AFTRSN · Architecture par processus métier
- [[runbook-memoire-centrale-asp-memory-et-agents-aftrsn]] : AFTRSN · Runbook mémoire centrale asp_memory & agents
- [[workflow-lumina-ai-router]] : AFTRSN · Workflow LUMINA-AI-Router (routage multi-LLM par task_type via OpenRouter)
<!-- AUTO-LIENS:fin -->
