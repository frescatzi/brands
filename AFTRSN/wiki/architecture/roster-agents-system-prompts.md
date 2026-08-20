---
type: wiki
title: "AFTRSN — Roster d'agents : structure des system prompts et câblage n8n"
status: draft
publish: none
vault: brands
brand: AFTRSN
sources: [AFTRSN/raw/2026-07-02--aftrsn-system-prompts-1ere-vague-maestro-2-experts-secretaire-cablage-memoire-2026-07-01.md, AFTRSN/raw/2026-07-02--aftrsn-system-prompts-1ere-vague-maestro-3-experts-secretaire-cablage-memoire-2026-07-01.md, AFTRSN/raw/2026-07-02--aftrsn-system-prompts-1ere-vague-v2-maestro-culture-experience-secretaire-marketing-2026-07-01.md]
updated: 2026-08-20
related: ["cablage-maestro-subagents-router", "clonage-roster-agents-nouvelle-marque", "hermes-outil-ops-agents", "architecture-processus-metier"]
---

# AFTRSN — Roster d'agents : structure des system prompts et câblage n8n

Brouillons de conception (01.07.2026) des system prompts de la 1ʳᵉ vague d'agents AFTRSN, avant câblage n8n. Ces prompts ont évolué en trois itérations le même jour ; le roster réellement câblé peut avoir divergé depuis (ex. un agent Channel-Content existe en production hors de ces brouillons) — pour l'état de câblage courant, voir [[cablage-maestro-subagents-router]].

## Évolution du roster

1. **Brouillon A — Maestro + 3 experts** : Music & Sound Curator (autorité DJs/line-up/sound), Culture & Community Steward, Experience & Event Designer, + Secrétaire.
2. **Brouillon B — Maestro + 2 experts** : le **Music & Sound Curator est retiré définitivement** — un LLM ne connaît pas de façon fiable les DJs (locaux/internationaux) et leurs styles, cette donnée n'est pas dans le modèle ; à ne pas recréer sans base de données DJ dédiée.
3. **v2 — Maestro + Culture + Experience + Secrétaire + Marketing (5 agents)** : ajout d'un agent **Marketing (events)**, qui porte aussi le rôle de garde-voix de marque. 2ᵉ vague planifiée (plus tard) : Comptable & Finance, Qualité & Compliance, Content (Insta/newsletter/WhatsApp) — Channel-Content et Acquisition-Performance (anciens agents) envisagés comme base de Content et Marketing respectivement.

Convention constante sur les 3 versions : prompts rédigés **en anglais** (champ n8n), explications en français ; modèle de collaboration **orchestré + appels ciblés** ; **Karter = autorité finale**.

## Bloc commun (préfixé dans chaque prompt)

- **Naming** : la marque est AFTER SUN PEOPLE (AFTRSN) ; "After Sun" désigne l'événement, jamais la marque — jamais "ASP" ni "Afterson People".
- **Brand essence** : AFTER SUN = *Day & Night Ritual celebrating African Electronic Dance* (Afro House, Afro Tech, Melodic Afro House) ; esthétique warm/editorial/soulful, culturelle sans clichés.
- **Memory-first** : toujours interroger l'outil **Knowledge** avant de répondre.
- **Language** : répondre dans la langue de l'utilisateur (défaut français).
- **Authority** : Katel (Karter) est le décideur final ; toute action critique/irréversible (dépense, envoi, publication, suppression) est signalée pour approbation, jamais exécutée par l'agent lui-même.

## Patron de rôle par type d'agent

- **Maestro (orchestrateur)** : point d'entrée unique, décide qui impliquer, délègue (parallélisable), **synthétise** et rapporte à Karter de façon structurée (décision, rationale, avis de chaque agent, questions ouvertes).
- **Experts challengers (Culture-Steward, Experience-Designer, et Music-Curator dans le brouillon A)** : ne produisent jamais de livrable final — ils *pressure-testent* une idée et rendent un verdict (ex. credible/risky/off, ou serves-the-ritual/weakens-it) + le risque ou l'écart précis + des correctifs concrets.
- **Secrétaire (assistant pratique)** : rédige des brouillons (emails, agenda) via Hermès, **ne les envoie jamais** — "draft, don't send" — et rend le brouillon + une note de ce qui reste à décider par Karter.
- **Marketing (v2 uniquement)** : transforme un objectif en plan (audience, angles, canaux, KPIs, budget) sans écrire le copy final (brief l'agent Content à venir) ; porte aussi la conformité de la voix de marque (revue contre la liste `forbidden_words`, correctifs avant/après).

## Câblage n8n associé

- **Modèle de chat (OpenRouter, credential "OpenRouter account")** : Maestro + experts + Marketing sur un modèle de raisonnement (`~anthropic/claude-sonnet-latest`) ; rôles plus légers (challengers courts, Secrétaire) sur un modèle économique (`~openai/gpt-mini-latest` ou `~google/gemini-flash-latest`) — l'arbitrage précis évolue d'un brouillon à l'autre, à trancher au câblage réel.
- **Outil mémoire "Knowledge"** (HTTP → webhook `memory-search`) : `brand=aftrsn` fixe pour tous les sous-agents ; pour le **Maestro**, `brand` est choisi par le LLM via `$fromAI` (défaut `lumina`, peut demander `aftrsn`) — lecture large + brand context à la demande.
- **Chat memory (Postgres)** : Session ID = `{{ $json.sessionId || 'subagent' }}`.
- **Triggers de chaque sous-agent** : *When chat message received* (test) + *When Executed by Another Workflow* (appel réel par Maestro) ; source du prompt = `{{ $json.query || $json.chatInput }}`.
- **Outils de Maestro** : un *Call n8n Workflow Tool* dédié par sous-agent (jamais un node générique partagé) + **Knowledge** + option *Call LUMINA-AI-Router*.
- **Nommage des nodes** : outil mémoire = `Knowledge`, historique = `Chat memory` (le prompt y fait explicitement référence).
- **Ordre de câblage** : 1) renommer Supervisor → Maestro + coller le prompt à jour ; 2) créer/ajuster chaque sous-agent (dupliquer le patron, coller son prompt, fixer `brand=aftrsn`) ; 3) ajouter les Call-tools sur Maestro ; 4) activer les sous-agents ; 5) tester par une demande explicite dans le chat de Maestro.

<!-- AUTO-LIENS:début -->
## Voir aussi
- [[cablage-maestro-subagents-router]] : AFTRSN · Câblage Maestro ↔ sub-agents ↔ Router (état de câblage réel, dépannage)
- [[clonage-roster-agents-nouvelle-marque]] : AFTRSN · Playbook de clonage de ce roster vers une nouvelle marque
- [[hermes-outil-ops-agents]] : AFTRSN · Hermès branché comme outil Ops du Secrétaire et du Marketing
- [[architecture-processus-metier]] : AFTRSN · Architecture par processus métier (Maestro délègue, Hermès exécute)
<!-- AUTO-LIENS:fin -->
