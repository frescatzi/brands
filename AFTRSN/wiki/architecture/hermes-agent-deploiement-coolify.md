---
type: wiki
title: "AFTRSN — Hermes Agent : déploiement Coolify (accès, dépannage)"
status: draft
publish: none
vault: brands
brand: AFTRSN
sources: [AFTRSN/raw/2026-07-02--aftrsn-lumina-pos-exact-hermes-agent-sur-coolify-deploiement-acces-depannage-2026-07-01.md]
updated: 2026-08-20
related: ["architecture-processus-metier", "hermes-outil-ops-agents", "operator-os-deploiement-coolify"]
---

# AFTRSN — Hermes Agent : déploiement Coolify (accès, dépannage)

Hermès est le bras d'exécution des agents AFTRSN (web search/browse/extract/vision, génération d'images, TTS, `execute_code`, skills, cron, sous-agents parallèles, support MCP) — voir la logique « Maestro délègue, Hermès exécute » dans [[architecture-processus-metier]]. ⚠️ À ne pas confondre avec un éventuel modèle Hermes local gratuit : cet agent-ci **appelle des API payantes** (OpenRouter/Anthropic/OpenAI/Google).

## Déploiement (Coolify, VPS Hetzner)

Projet **AFTRSN-Automation / production**, compose à 2 services :
- **hermes-agent** (`nousresearch/hermes-agent`, `gateway run`) — backend, détient les clés API. **Sans domaine public.**
- **hermes-webui** (`ghcr.io/nesquena/hermes-webui`, port 8787, `HERMES_WEBUI_HOST=0.0.0.0`) — porte le domaine **https://hermes.aftersunpeople.com**, protégé par mot de passe (variable Coolify `SERVICE_PASSWORD_HERMESWEBUI`, au niveau ressource).

## Règle d'or Coolify pour exposer un service

- Le domaine se déclare via **`SERVICE_FQDN_<SERVICE>_<PORT>`** (ex. `SERVICE_FQDN_HERMESWEBUI_8787=hermes.aftersunpeople.com`) : c'est cette variable qui crée la route Traefik. **`SERVICE_URL_*` ne fait que générer une URL, sans créer de route.**
- Domaine précédé de `https://` → déclenche Let's Encrypt. Un domaine nu reste en http/certificat auto-signé.
- Un seul service peut porter un domaine donné ; deux services sur le même domaine créent un conflit Traefik (404).
- Toute édition de compose/domaine exige un **Redeploy** complet (un simple Restart ne suffit pas).

## Sécurité

- Accès uniquement via Traefik 443 + mot de passe fort (la WebUI exécute des commandes et parcourt les fichiers).
- Le port 8787 ne doit **jamais** être ouvert en firewall public — inutile avec Traefik, et permettrait un accès direct sans TLS.

## Dépannage

| Symptôme | Cause probable | Fix |
|---|---|---|
| `404 page not found` | Domaine sur le mauvais service, `SERVICE_URL` utilisé à la place de `SERVICE_FQDN`, ou domaine dupliqué | Poser `SERVICE_FQDN_HERMESWEBUI_8787=<domaine>`, retirer le domaine de hermes-agent, Redeploy |
| `Not Secure` (http) | Domaine sans schéma | Ajouter `https://` dans Domains |
| `Not Private` (certificat auto-signé) | Let's Encrypt pas encore émis | `https://` + Redeploy, attendre 1–2 min, vérifier l'email LE de l'instance |
| WebUI injoignable | Bind localhost | `HERMES_WEBUI_HOST=0.0.0.0` |

## Dette / à faire

- Servir un modèle Hermes open-weight local (endpoint OpenAI-compatible) comme « workhorse gratuit » — nécessite un GPU, non disponible à date.
- Rotation du mot de passe WebUI, durcissement de l'auth MCP.

<!-- AUTO-LIENS:début -->
## Voir aussi
- [[architecture-processus-metier]] : AFTRSN · Architecture par processus métier (Maestro délègue, Hermès exécute)
- [[hermes-outil-ops-agents]] : AFTRSN · Hermès branché comme outil Ops des agents (Secrétaire, Marketing)
- [[operator-os-deploiement-coolify]] : Operator OS · déploiement Coolify (GitHub privé, Basic Auth, synchro base)
<!-- AUTO-LIENS:fin -->
