---
type: wiki
title: "AFTRSN — Playbook : cloner le roster d'agents vers une nouvelle marque"
status: draft
publish: none
vault: brands
brand: AFTRSN
sources: [AFTRSN/raw/2026-07-02--pos-aftrsn-clonage-roster-agents-nouvelle-marque-2026-07-02.md]
updated: 2026-08-20
related: ["cablage-maestro-subagents-router", "memoire-multi-marques-ingestion", "workflow-lumina-ai-router", "roster-agents-system-prompts"]
---

# AFTRSN — Playbook : cloner le roster d'agents vers une nouvelle marque

Provisionner une nouvelle marque suit trois étapes : (1) créer sa banque mémoire, (2) ingérer son canon, (3) **cloner le roster d'agents par script** plutôt que de le ré-écrire à la main — le clone hérite d'emblée des skills génériques partagées.

## Principe

Le roster (6 sous-agents spécifiques marque : Culture-Steward, Experience-Designer, Secretary, Marketing, Comptable-Finance, Qualité-Compliance + le Maestro) est copié en profondeur (nouveaux ids internes) pour chaque nouveau code de marque, avec deux rebinds systématiques :
- **Knowledge** (outil mémoire) : le champ `brand` dans son corps de requête est réécrit sur le nouveau code — c'est ce rebind qui **scelle** la mémoire de l'agent cloné sur la banque de la nouvelle marque (voir [[memoire-multi-marques-ingestion]]).
- **Prompt système** : un bloc *BRAND CONTEXT* (nom de marque, voix/ton, mots interdits) est préfixé, et les mentions de la marque source sont remplacées par celles de la nouvelle marque.

Le Maestro cloné a en plus besoin d'un **remap** de chaque outil `toolWorkflow` pointant un sous-agent : chaque référence doit être redirigée vers l'id du sous-agent nouvellement créé (voir [[cablage-maestro-subagents-router]] pour le câblage d'origine).

**Outils partagés jamais clonés** : le Router (routage multi-LLM) et Save Episode (écriture mémoire épisodique) sont brand-agnostiques — ils dérivent la marque de leur input à l'exécution. Les cloner créerait des doublons inutiles et casserait le routage central.

## Procédure d'onboarding complète

1. Provisionner la banque mémoire de la marque (`{code, brand_name, tone_voice, forbidden_words, kb_ref}`) : crée `<code>_memory`, insère la ligne `memory_registry` et le profil de marque.
2. Ingérer le canon de la marque (`collection='canon'`) via l'ingestion multi-format (voir [[memoire-multi-marques-ingestion]]).
3. **Dry-run** du clonage : générer les transformations sans rien créer, et vérifier que le rebind mémoire est correct pour chaque sous-agent avant d'aller plus loin.
4. **Exécution réelle** : créer et activer les 6 sous-agents + le Maestro clonés, préfixés du code de la nouvelle marque.
5. Tester : ouvrir le chat du Maestro cloné, poser une question métier, vérifier la délégation, la voix de marque dans la réponse, et que l'outil Knowledge interroge bien la banque de la nouvelle marque (pas celle d'AFTRSN).

## Pièges figés

1. **Ne jamais cloner les outils partagés** (Router, Save Episode) — ils sont brand-agnostiques par construction.
2. **Le rebind Knowledge est le point d'isolation mémoire critique** : si le champ `brand` n'est pas réécrit dans le clone, l'agent cloné continue de lire la banque d'AFTRSN. Toujours vérifier ce rebind avant toute exécution réelle.
3. **Les connexions n8n se font par nom de node** : garder les noms de nœuds identiques dans le clone, seul l'identifiant interne change — renommer un node casse le câblage.
4. **La voix de marque vit dans le prompt** (bloc BRAND CONTEXT + profil) ; les skills génériques partagées sont héritées automatiquement, les skills spécifiques à la marque s'accumulent séparément dans sa propre banque et remontent via l'outil d'exécution (recherche combinant les skills partagées et celles de la marque active).
5. **Suppression d'un clone de test** = action destructive → toujours effectuée manuellement par un humain, jamais par l'assistant.
6. L'activation d'un clone exige des credentials valides sur tous ses nodes (Postgres, LLM, embeddings) — héritées de la source, donc normalement déjà correctes.

<!-- AUTO-LIENS:début -->
## Voir aussi
- [[cablage-maestro-subagents-router]] : AFTRSN · Câblage Maestro ↔ sub-agents ↔ Router (roster source)
- [[memoire-multi-marques-ingestion]] : AFTRSN · Mémoire multi-marques : banques, ingestion, récupération
- [[workflow-lumina-ai-router]] : AFTRSN · Workflow LUMINA-AI-Router (outil partagé, jamais cloné)
- [[roster-agents-system-prompts]] : AFTRSN · Roster d'agents : structure des system prompts et câblage n8n (roster à cloner)
<!-- AUTO-LIENS:fin -->
