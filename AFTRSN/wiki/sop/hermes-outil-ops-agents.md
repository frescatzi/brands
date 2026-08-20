---
type: wiki
title: "AFTRSN — Hermès branché comme outil Ops des agents"
status: draft
publish: none
vault: brands
brand: AFTRSN
sources: [AFTRSN/raw/2026-07-02--pos-aftrsn-hermes-outil-ops-agents-2026-07-02.md]
updated: 2026-08-20
related: ["hermes-agent-deploiement-coolify", "cablage-maestro-subagents-router", "architecture-processus-metier"]
---

# AFTRSN — Hermès branché comme outil Ops des agents

Le Secrétaire et le Marketing peuvent déléguer une tâche opérationnelle (web, fichiers, exécution) à [[hermes-agent-deploiement-coolify|Hermès]] via un sous-workflow dédié, sans jamais exposer de secret aux agents.

## Décision de sécurité

- **Refusé** : autoriser l'accès aux variables d'environnement dans les nodes Code — cela exposerait toutes les clés/mots de passe de l'instance n8n à n'importe quel node.
- **Retenu** : le mot de passe Hermès vit dans un **credential Custom Auth** dédié, isolé (jamais lisible depuis un node Code ou une expression), avec un domaine autorisé restreint au seul host Hermès. L'URL Hermès elle-même n'est pas un secret et reste en dur dans le workflow.

## Le sous-workflow d'exécution

Chaîne : login (récupère un cookie de session via le credential Custom Auth) → ouverture d'une session Hermès → envoi du message → **poll** périodique du résultat jusqu'à ce que la réponse soit stable (deux lectures identiques) ou qu'un nombre maximal de tours soit atteint ; retourne le texte, l'id de session et le modèle utilisé. Le poll absorbe les erreurs transitoires (ex. 400 pendant l'initialisation du tour) sans faire échouer tout l'appel.

## Brancher sur un agent

Un node `toolWorkflow` nommé pour Hermès est ajouté à l'agent (Secrétaire, Marketing), avec une description précisant explicitement le périmètre (ex. le Secrétaire ne fait que préparer une action, jamais l'envoyer lui-même). Connexion `ai_tool` vers le node AI Agent.

## Tester

Le chat de l'éditeur n8n n'est pas fiable pour ce type de test en automation — préférer un appel API direct qui déclenche le workflow avec des données pré-injectées sur le bon trigger (`When Executed by Another Workflow`).

## Pièges figés

1. Un credential Custom Auth ne fusionne pas son corps en mode JSON brut → utiliser un corps en mode champs nom/valeur pour que l'injection du credential fonctionne.
2. Sur un n8n en mode queue (avec workers séparés), le code des nodes Code tourne sur le worker et l'accès aux variables d'environnement y est bloqué par défaut → ne jamais dépendre de l'environnement, toujours passer par un credential.
3. Le chat de test de l'éditeur est fragile pour les workflows appelés en sous-workflow → toujours valider par un déclenchement API explicite.
4. Un appel HTTP + une temporisation dans un node Code fonctionnent, mais le polling doit absorber les erreurs transitoires (try/catch) pendant l'initialisation côté Hermès.
5. Prérequis : le backend Hermès doit être correctement déployé et authentifiable (voir [[hermes-agent-deploiement-coolify]]) — sinon les réponses signalent une authentification manquante.

<!-- AUTO-LIENS:début -->
## Voir aussi
- [[hermes-agent-deploiement-coolify]] : AFTRSN · Hermès Agent : déploiement Coolify (accès, dépannage)
- [[cablage-maestro-subagents-router]] : AFTRSN · Câblage Maestro ↔ sub-agents ↔ Router
- [[architecture-processus-metier]] : AFTRSN · Architecture par processus métier (Maestro délègue, Hermès exécute)
<!-- AUTO-LIENS:fin -->
