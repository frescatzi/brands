---
type: wiki
title: "AFTRSN — Mémoire multi-marques : banques, ingestion, récupération"
status: draft
publish: none
vault: brands
brand: AFTRSN
sources: [AFTRSN/raw/2026-07-02--aftrsn-lumina-pos-exact-runbook-memoire-multi-banques-ajouter-une-marque-ingerer-router.md, AFTRSN/raw/2026-07-02--aftrsn-lumina-pos-exact-runbook-ingestion-bible-de-marque-drive-pdf-vers-aftrsn-memory-et-recuperation-simplifiee.md, AFTRSN/raw/2026-07-02--pos-aftrsn-ingestion-texte-et-recursion-drive-2026-07-02.md]
updated: 2026-08-20
related: ["runbook-memoire-centrale-asp-memory-et-agents-aftrsn", "memoire-episodique-consolidation-rag", "clonage-roster-agents-nouvelle-marque", "architecture-processus-metier"]
---

# AFTRSN — Mémoire multi-marques : banques, ingestion, récupération

Successeur de l'ancienne mémoire unique `asp_memory` (voir [[runbook-memoire-centrale-asp-memory-et-agents-aftrsn]]) : chaque marque a sa propre banque pgvector, routée via un registre, pour éviter qu'une recherche sans filtre noie les agents dans du contenu hors marque.

## Cartographie

- **`lumina_memory`** — banque partagée (automation/process), héritée d'`asp_memory`.
- **`aftrsn_memory`** — banque de la marque After Sun People, contient son canon (bible de marque).
- **`memory_registry`** — table de routage : `code, table_name, display_name, kind('shared'|'brand'), active`.
- Schéma commun à toute banque : `id, title, content, collection, knowledge_type, source, source_ref, content_hash UNIQUE, embedding VECTOR(1536), created_at, updated_at`, index HNSW cosine.
- Tags `collection` : `canon` (bible), `voice`, `ops`, `process`, `history`, `episodic`/`insights` (voir [[memoire-episodique-consolidation-rag]]).
- Credentials : embeddings OpenAI `text-embedding-3-small` (`OpenAi account`), DB (`Postgres account`), Drive (`Google Drive account - Live`).

## Workflows clés

| Workflow | Rôle |
|---|---|
| `LUMINA-RETRIEVAL-MEMORY/WEBHOOK` (webhook `memory-search`) | Récupération, routée par `brand` via le registre |
| `LUMINA-MEMORY-INGESTION/WIKI-GITHUB→PGVECTOR` | Ingestion de la banque partagée → `lumina_memory` |
| `AFTRSN-MEMORY-INGESTION/DRIVE→AFTRSN_MEMORY` | Ingestion de la bible (PDF unique, MVP) → `aftrsn_memory` |
| `LUMINA-MEMORY-INGEST-TEXT` | Ingestion générique texte/markdown, réutilisable pour toute marque |
| `AFTRSN-MEMORY-INGESTION/DRIVE-RECURSIVE` | Balayage d'un dossier Drive (PDF + markdown) → ingestion en masse |

## Ajouter une nouvelle marque

1. Choisir un **code** lowercase court et stable (ex. `aftrsn`).
2. Créer la table `<code>_memory` (schéma standard ci-dessus) + index HNSW.
3. Insérer la ligne dans `memory_registry` (`code`, `<code>_memory`, nom affiché, `kind='brand'`, `active=true`).
4. Aucun nouveau workflow à créer si les templates d'ingestion/récupération sont paramétrés par `code` (voir ci-dessous).
5. Ingérer le contenu de marque (canon) — voir sections suivantes.

## Ingérer du contenu

**Ingestion texte/markdown (générique, réutilisable)** : un sous-workflow reçoit `{brand, title, content, collection, source_ref, knowledge_type}` → Chunk (taille 3000, overlap 200) → Embed (1 appel/chunk) → Build Insert paramétré (table `<brand>_memory` dérivée du code, `$1..$7`, `content_hash = md5(content)`, `ON CONFLICT DO NOTHING`) → Postgres Insert (Query Parameters en Expression). Aucun `TRUNCATE` dans cette variante : elle préserve les collections `episodic`/`insights`/`skills` déjà en base ; l'idempotence par hash rend un ré-ingestion sans effet si le contenu est inchangé.

**Ingestion bible de marque (MVP, un seul fichier)** : chaîne dédiée `Truncate aftrsn_memory → Download (Drive, By ID) → Extract PDF → Set Document (collection='canon') → Chunk → Embed → Build Insert → Postgres Insert`, ciblée sur un fichier Drive précis. Le `TRUNCATE` en tête de cette variante signifie un **reload complet** de la banque — à ne lancer que si l'on est prêt à reconstruire toute la table (un échec après le truncate la laisse vide).

**Récursion Drive (étape suivante, PDF + markdown)** : balaie un dossier Drive entier — Drive Search (`fileFolder`, `returnAll`) → filtre ne gardant que `application/pdf` / `text/markdown` / `text/plain` (docx exclu, n8n ne le parse pas nativement) → Download → branche par `mimeType` (Extract PDF ou Extract Text) → Set Document → Chunk → Embed → Build Insert → Postgres Insert, sans `TRUNCATE` (variante additive, idempotente par hash). Limite connue : la récursion ne descend qu'à un niveau (`in parents` ne descend pas dans les sous-dossiers).

> Point d'attention technique commun aux 3 chaînes : après un appel Embed, `$json` est remplacé par la réponse OpenAI — les métadonnées du document (title, source_ref…) doivent être récupérées via l'item du node source (`$('Chunk').all()[i]` ou équivalent), aligné par index.

## Récupération (câblage de référence)

`Webhook → Embed Question (OpenAI) → Build Search → Postgres Search`, réponse en **When Last Node Finishes / All Entries** (jamais de node `Respond to Webhook` avec ce mode). `Build Search` résout le nom de banque via une **allowlist** (`{lumina:'lumina_memory', aftrsn:'aftrsn_memory', ...}`, défaut `lumina`) — jamais d'interpolation directe d'un nom de table venant de l'input (anti-injection SQL). `Postgres Search` en **Always Output Data** pour qu'une banque vide renvoie `[{}]` plutôt qu'un crash.

## Règles de routage & sécurité

- Le MCP (`search_brand_memory(brand=code, question)` / `search_process_memory(question)`) et les workflows ne reçoivent qu'un **code** ; la table réelle est toujours résolue via `memory_registry`, jamais construite par interpolation libre.
- Câblage des agents : Gardien-marque / Contenu-canaux / agents de domaine → banque de leur marque ; agents dev/ops → `lumina_memory`. L'outil mémoire des agents s'appelle uniformément **`Knowledge`** (cohérence node ↔ prompt).

## Vérifier

Node `Verify Table Count` (`GROUP BY source_ref`) après chaque ingestion, puis test end-to-end via le MCP `search_brand_memory` — jamais se fier au seul statut vert des nodes.

## Dette connue

- Filtrage `collection` pas encore branché dans la recherche du Gardien-marque.
- Récursion Drive limitée à un niveau de profondeur.
- Auth MCP absente (« none », MVP) — à durcir (OAuth2).
- Produire des `.md` propres dans Obsidian pour capter en profondeur les contenus non encore ingérés (ex. genres musicaux détaillés, au-delà des 2 chunks du PDF design initial).

## Pièges figés

- Ne jamais construire de SQL par concaténation ni interpoler un nom de table venant de l'utilisateur → paramètres + allowlist registre.
- Webhook + « last node finishes » sans node Respond = récupération robuste ; un node Respond intermédiaire coupe la connexion en aval.
- Pipelines TRUNCATE-puis-reload : un échec après le truncate vide la table — à réserver aux reconstructions volontaires ; préférer les variantes additives (hash + `ON CONFLICT DO NOTHING`) pour l'ingestion courante.
- `.docx` non supporté nativement par n8n → convertir en `.md`/`.pdf`, ou n'ingérer que PDF+MD.
- Toujours valider par un appel MCP réel, pas seulement au niveau des nodes.

<!-- AUTO-LIENS:début -->
## Voir aussi
- [[runbook-memoire-centrale-asp-memory-et-agents-aftrsn]] : AFTRSN · Runbook mémoire centrale asp_memory & agents (architecture prédécesseure)
- [[memoire-episodique-consolidation-rag]] : AFTRSN · Mémoire épisodique, consolidation nocturne et RAG dans le Router
- [[clonage-roster-agents-nouvelle-marque]] : AFTRSN · Playbook de clonage du roster d'agents vers une nouvelle marque
- [[architecture-processus-metier]] : AFTRSN · Architecture par processus métier
<!-- AUTO-LIENS:fin -->
