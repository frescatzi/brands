---
type: wiki
title: "AFTRSN — Câblage Maestro ↔ sub-agents ↔ Router"
status: draft
publish: none
vault: brands
brand: AFTRSN
sources: [AFTRSN/raw/2026-07-02--pos-aftrsn-cablage-maestro-subagents-router-2026-07-02.md]
updated: 2026-08-20
related: ["workflow-lumina-ai-router", "architecture-processus-metier", "clonage-roster-agents-nouvelle-marque", "hermes-outil-ops-agents"]
---

# AFTRSN — Câblage Maestro ↔ sub-agents ↔ Router

État atteint le 02.07.2026 : chat → Maestro → délégation aux spécialistes → routage multi-LLM via le [[workflow-lumina-ai-router|LUMINA-AI-Router]] → synthèse, en production.

## Délégation Maestro → spécialistes

Chaque spécialiste (Culture-Steward, Experience-Designer, Secretary, Marketing, + Channel-Content hors tools) est branché sur Maestro via un node `toolWorkflow` dédié (un par spécialiste, jamais un node générique) : `workflowId` pointe l'id du sous-workflow, l'entrée `query` est fournie par le modèle (`$fromAI`), et une `description` en langage naturel indique à l'agent quand déléguer à qui. La connexion se fait en `ai_tool` vers le node AI Agent de Maestro.

Nodes obsolètes retirés au passage (ids/typos ne pointant plus vers un spécialiste existant) — un `toolWorkflow` orphelin échoue silencieusement en délégation, d'où l'intérêt de nettoyer avant d'ajouter les bons nodes. Test de validation : demander explicitement dans le chat une délégation à un expert précis, vérifier le node vert et le verdict retourné.

## Réparer un spécialiste inactif (cas Secretary)

Deux causes distinctes à ne pas confondre : (1) le system prompt à jour n'était pas encore collé dans `AI Agent → options.systemMessage` ; (2) le workflow ne pouvait pas s'activer faute de **credential Postgres** sur le node de mémoire de chat — copier la credential d'un agent sain résout ce second point. Un échec d'activation « Missing required credential » pointe toujours vers un node précis à identifier avant de chercher plus loin.

## Maestro → Router

Maestro appelle le Router via un `toolWorkflow` avec 3 entrées `$fromAI` : `task_type` (dont la liste des valeurs valides est décrite dans la `description` du node, pour que le modèle choisisse le bon type), `instruction`, `context_raw`. Le prompt de Maestro porte une section `ROUTING:` : utiliser le Router pour les tâches lourdes/spécialisées, jamais pour des données personnelles (PII → voir garde-fou dans [[workflow-lumina-ai-router]]).

Côté Router, le contrat accepte les champs **à plat** en plus de la forme imbriquée (`p.routing?.task_type || p.task_type`, etc.) pour rester compatible avec plusieurs appelants. Point d'attention critique : le trigger `executeWorkflowTrigger` en mode `jsonExample` doit déclarer **tous** les champs acceptés (plat + imbriqué) dans son exemple JSON — un champ non déclaré est silencieusement supprimé à l'entrée, et le Router retombe alors sur ses valeurs par défaut (`draft`) sans erreur visible.

## Dépannage

- Sous-workflow qui reçoit des valeurs par défaut inattendues → vérifier d'abord le `jsonExample` du trigger `executeWorkflowTrigger` (champ manquant = silencieusement droppé).
- Publication refusée (« Missing required credential ») → une credential manque sur un node précis, à identifier avant de modifier la logique.
- Une modification qui n'apparaît pas en production après un appel API → l'API PATCH ne crée qu'un brouillon ; la publication réelle passe par l'activation du workflow (endpoint dédié), pas par le PATCH seul.

## Pièges figés

- Ne jamais laisser un node `toolWorkflow` pointer un id de workflow supprimé/renommé — la délégation échoue sans erreur explicite côté agent appelant.
- Les connexions n8n se font par nom de node : renommer un node casse le câblage interne même si l'id du workflow ne change pas.
- Toujours tester une délégation par une demande explicite dans le chat, pas seulement en vérifiant que les workflows sont publiés.

<!-- AUTO-LIENS:début -->
## Voir aussi
- [[workflow-lumina-ai-router]] : AFTRSN · Workflow LUMINA-AI-Router (routage multi-LLM par task_type)
- [[architecture-processus-metier]] : AFTRSN · Architecture par processus métier (Maestro délègue, Hermès exécute)
- [[clonage-roster-agents-nouvelle-marque]] : AFTRSN · Playbook de clonage du roster d'agents vers une nouvelle marque
- [[hermes-outil-ops-agents]] : AFTRSN · Hermès branché comme outil Ops des agents
<!-- AUTO-LIENS:fin -->
