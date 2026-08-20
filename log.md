# log.md — coffre `brands`

> Append-only. Ingests, Q&A, lints, créations d'agents/skills. Préfixer par marque.
> Actions : `ingest` · `qa` · `lint` · `agent` · `skill` · `schema-change`.

---

## [2026-06-20] schema-change | Création du coffre brands (v2.2) + dossier AFTRSN
## [2026-07-07] ingest | AFTRSN | AFTRSN/raw/2026-07-07--test-brands.md ignoré (note de test auto-ingest, hors périmètre wiki — aucune connaissance de marque à synthétiser)
## [2026-07-07] ingest | AFTRSN | AFTRSN/raw/2026-07-07--TEST-AFTSRN-Brands-04.md ignoré (note de test auto-ingest, frontmatter seul sans corps — aucune connaissance de marque à synthétiser)
## [2026-07-20] ingest | AFTRSN | AFTRSN/raw/2026-07-20--passation-incidents-memoire-et-intake-2026-07-05.md ignoré pour wiki (passation/journal d'incidents infra n8n-Postgres, hors périmètre marque — ajouté à _archive_queue.json)
## [2026-07-21] ingest | AFTRSN | AFTRSN/raw/2026-07-21--aftrsn-architecture-processus-20260704.md -> AFTRSN/wiki/architecture-processus-metier.md (corps identique à AFTRSN/raw/2026-07-07--aftrsn-architecture-processus-20260704.md, jamais compilé jusqu'ici — première synthèse de ce contenu)
## [2026-07-21] ingest | AFTRSN | aftrsn/raw/2026-07-21--aftrsn-marcheasuivre-construction-n8n.md -> AFTRSN/wiki/methode-construction-workflow-n8n.md (nouveau raw arrivé dans un dossier parasite `aftrsn/` en minuscules — page compilée dans le dossier canonique `AFTRSN/wiki/`, cf. brand du frontmatter)
## [2026-07-21] ingest | AFTRSN | AFTRSN/raw/2026-07-21--pos-aftrsn-backfill-outreach-manuel-20260705.md -> AFTRSN/wiki/backfill-outreach-manuel.md
## [2026-07-21] ingest | AFTRSN | AFTRSN/raw/2026-07-21--pos-aftrsn-capture-connaissance-debrief-20260706.md -> AFTRSN/wiki/workflow-knowledge-capture-debrief-post-event.md
## [2026-07-21] ingest | AFTRSN | AFTRSN/raw/2026-07-21--pos-aftrsn-repondeur-email-drafts-agent-20260704.md -> AFTRSN/wiki/workflow-repondeur-email-drafts.md
## [2026-07-21] ingest | AFTRSN | AFTRSN/raw/2026-07-21--aftrsn-lumina-marche-a-suivre-exacte-reparation-recuperation-apres-rename-lumina-memory-ingestion-bible-de-marque-depuis-drive-etape-4-mvp-2026-06-30.md ignoré pour wiki (marche à suivre / journal de débogage n8n-pgvector, hors périmètre marque — ajouté à _archive_queue.json)
## [2026-07-21] ingest | AFTRSN | AFTRSN/raw/2026-07-21--aftrsn-lumina-pos-exact-lumina-ai-router-routage-multi-llm-via-openrouter-2026-07-01.md -> AFTRSN/wiki/workflow-lumina-ai-router.md
## [2026-07-21] ingest | AFTRSN | AFTRSN/raw/2026-07-21--aftrsn-lumina-pos-exact-runbook-memoire-centrale-asp-memory-et-agents-aftrsn.md -> AFTRSN/wiki/runbook-memoire-centrale-asp-memory-et-agents-aftrsn.md (sujet déjà capturé le 2026-07-02 et 2026-07-07, corps identique — jamais compilé avant ; ajouté aux sources)
## [2026-07-21] ingest | AFTRSN | AFTRSN/raw/2026-07-21--aftrsn-lumina-plan-de-projet-lumina-ai-os-etapes-achevees-et-a-venir-2026-07-01.md ignoré pour wiki (plan de projet / suivi de milestones par phases, corps identique aux captures du 2026-07-02 et 2026-07-07 — hors périmètre wiki, ajouté à _archive_queue.json)
## [2026-08-17] ingest | AFTRSN | AFTRSN/raw/2026-08-17--pos-exact-operator-os-architecture-dashboard-et-donnees-2026-08-17.md -> AFTRSN/wiki/architecture/operator-os-dashboard-mesure.md
## [2026-08-19] ingest | AFTRSN | AFTRSN/raw/2026-08-17--pos-exact-operator-os-deploiement-coolify-basicauth-synchro-2026-08-17.md -> AFTRSN/wiki/architecture/operator-os-deploiement-coolify.md (chaîne de déploiement Coolify de l'Operator OS ; complète [[operator-os-dashboard-mesure]] qui pointait ce raw comme « non encore compilé »)
## [2026-08-19] lint | AFTRSN | audit backlinks : 3 pages orphelines reconnectées (operator-os-dashboard-mesure, runbook-memoire-centrale-asp-memory-et-agents-aftrsn, workflow-lumina-ai-router) + 2 liens rendus réciproques ; 9 pages, 0 lien mort, 0 orpheline
## [2026-08-20] ingest | AFTRSN | AFTRSN/raw/2026-07-02--aftrsn-lumina-marche-a-suivre-exacte-reparation-pipeline-memoire-credential-openai-insert-parametre-et-etat-des-lieux-agents-2026-06-29.md -> AFTRSN/wiki/runbook-memoire-centrale-asp-memory-et-agents-aftrsn.md (sujet déjà couvert par ce runbook, qui la référençait déjà comme « détail pédagogique » — ajoutée aux sources, enrichi de la décision de nommage AFTRSN/LUMINA)
## [2026-08-20] ingest | AFTRSN | AFTRSN/raw/2026-07-02--aftrsn-lumina-pos-exact-hermes-agent-sur-coolify-deploiement-acces-depannage-2026-07-01.md -> AFTRSN/wiki/architecture/hermes-agent-deploiement-coolify.md
## [2026-08-20] ingest | AFTRSN | AFTRSN/raw/2026-07-02--aftrsn-lumina-pos-exact-runbook-ingestion-bible-de-marque-drive-pdf-vers-aftrsn-memory-et-recuperation-simplifiee.md + AFTRSN/raw/2026-07-02--aftrsn-lumina-pos-exact-runbook-memoire-multi-banques-ajouter-une-marque-ingerer-router.md + AFTRSN/raw/2026-07-02--pos-aftrsn-ingestion-texte-et-recursion-drive-2026-07-02.md -> AFTRSN/wiki/sop/memoire-multi-marques-ingestion.md (3 raw fusionnés en une page : architecture multi-banques, ingestion bible MVP, généralisation texte/récursion Drive — même sujet évolutif du 02.07.2026)
## [2026-08-20] ingest | AFTRSN | AFTRSN/raw/2026-07-02--pos-aftrsn-memoire-episodique-consolidation-rag-2026-07-02.md -> AFTRSN/wiki/sop/memoire-episodique-consolidation-rag.md
## [2026-08-20] ingest | AFTRSN | AFTRSN/raw/2026-07-02--pos-aftrsn-cablage-maestro-subagents-router-2026-07-02.md -> AFTRSN/wiki/sop/cablage-maestro-subagents-router.md
## [2026-08-20] ingest | AFTRSN | AFTRSN/raw/2026-07-02--pos-aftrsn-clonage-roster-agents-nouvelle-marque-2026-07-02.md -> AFTRSN/wiki/sop/clonage-roster-agents-nouvelle-marque.md
## [2026-08-20] ingest | AFTRSN | AFTRSN/raw/2026-07-02--pos-aftrsn-hermes-outil-ops-agents-2026-07-02.md -> AFTRSN/wiki/sop/hermes-outil-ops-agents.md
## [2026-08-20] lint | AFTRSN | backlinks réciproques ajoutés sur workflow-lumina-ai-router, architecture-processus-metier, operator-os-deploiement-coolify, workflow-knowledge-capture-debrief-post-event pour pointer vers les 6 nouvelles pages ; index.md mis à jour
