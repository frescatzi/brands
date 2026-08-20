---
type: wiki
title: "AFTRSN — Mémoire épisodique, consolidation nocturne et RAG dans le Router"
status: draft
publish: none
vault: brands
brand: AFTRSN
sources: [AFTRSN/raw/2026-07-02--pos-aftrsn-memoire-episodique-consolidation-rag-2026-07-02.md]
updated: 2026-08-20
related: ["memoire-multi-marques-ingestion", "workflow-lumina-ai-router", "workflow-knowledge-capture-debrief-post-event", "cablage-maestro-subagents-router"]
---

# AFTRSN — Mémoire épisodique, consolidation nocturne et RAG dans le Router

Couche « mémoire vivante » au-dessus des banques par marque (voir [[memoire-multi-marques-ingestion]]) : les agents journalisent ce qu'ils font, une consolidation nocturne en tire des enseignements, et le Router y puise du contexte avant chaque appel LLM créatif.

## Écriture épisodique

Sous-workflow `LUMINA-MEMORY-WRITE` (`executeWorkflowTrigger {brand, title, content, collection, knowledge_type, source, source_ref}`) → Embed → Build Insert paramétré (`md5(content)` pour l'idempotence) → Postgres. **Volontairement pas un webhook** : sur ce n8n en mode queue, les webhooks créés par API ne s'enregistrent pas (500 avant exécution) — d'où un `executeWorkflowTrigger`, appelé comme un outil par les agents.

Maestro dispose de l'outil **« Save Episode »** vers ce sous-workflow (`brand=aftrsn`, `collection=episodic`, title/summary/topic déterminés par le modèle). Consigne de prompt : après avoir traité une requête, appeler Save Episode une fois avec un résumé de 2–3 phrases (demande, décision, résultat). Le même sous-workflow est réutilisé par le workflow Knowledge-Capture (voir [[workflow-knowledge-capture-debrief-post-event]]) pour écrire les insights de débrief post-event.

## Filtrage par collection à la récupération

`memory-search` accepte un paramètre `collection` optionnel : fourni → `WHERE collection = '<coll>'` (sanitizé) ; sinon → `WHERE collection IS DISTINCT FROM 'episodic'` par défaut, pour que les épisodes bruts ne polluent pas les recherches de contenu canon. `limit` paramétrable (défaut 5, max 20).

## Consolidation nocturne

Cron quotidien → lit les 50 derniers épisodes (`collection='episodic'`) → un LLM (OpenRouter, modèle raisonnement) condense en 3–5 enseignements → écrit un digest daté dans `collection='insights'` (`source='nightly-consolidation'`). Aucune purge : les épisodes bruts sont conservés indéfiniment, seuls les insights consolidés s'accumulent en plus. Limite connue : la sélection est `ORDER BY id DESC LIMIT 50` faute de colonne `created_at` en base — pas de reprise incrémentale possible pour l'instant.

## RAG dans le Router

Le [[workflow-lumina-ai-router|LUMINA-AI-Router]] interroge `memory-search` (`{brand, question, limit:4}`) pour les types de tâche créatifs/analytiques (copy, reasoning, analysis, draft) quand une instruction/contexte est fournie, et préfixe le résultat dans le contexte envoyé au LLM. Best-effort (try/catch : en cas d'indisponibilité, l'appel continue sans mémoire). Le garde-fou PII (`task_type='private'`, bloqué avant tout appel réseau) reste prioritaire et intact.

## Pièges figés

- Les webhooks créés par API ne s'enregistrent pas en mode queue → `executeWorkflowTrigger` (appel-outil) ou création du webhook depuis l'UI.
- Ne jamais laisser `episodic` polluer les recherches canon → exclusion par défaut au niveau de la requête.
- Un insight par nuit (titre daté) pour éviter l'explosion de la banque ; `md5(content)` évite les doublons exacts.
- Le même modèle d'embedding (`text-embedding-3-small`, 1536 dims) doit être utilisé partout (écriture, recherche, insights) sous peine de distances incohérentes.
- Tester par appel API direct (`POST /rest/workflows/:id/run` + `triggerToStartFrom` + `pinData`) : le chat de l'éditeur n8n est peu fiable pour ce type de test.

<!-- AUTO-LIENS:début -->
## Voir aussi
- [[memoire-multi-marques-ingestion]] : AFTRSN · Mémoire multi-marques : banques, ingestion, récupération
- [[workflow-lumina-ai-router]] : AFTRSN · Workflow LUMINA-AI-Router (routage multi-LLM par task_type)
- [[workflow-knowledge-capture-debrief-post-event]] : AFTRSN · Workflow Knowledge-Capture : débrief post-event vers mémoire insights
- [[cablage-maestro-subagents-router]] : AFTRSN · Câblage Maestro ↔ sub-agents ↔ Router
<!-- AUTO-LIENS:fin -->
