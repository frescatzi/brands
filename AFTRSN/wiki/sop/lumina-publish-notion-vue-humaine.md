---
type: wiki
title: "AFTRSN — Publication Notion (vue humaine, upsert idempotent depuis le wiki)"
status: draft
publish: none
vault: brands
brand: AFTRSN
sources: [AFTRSN/raw/2026-06-24--lumina-systeme-reference.md]
updated: 2026-08-20
related: ["lumina-intake-robot-drive-github", "runbook-memoire-centrale-asp-memory-et-agents-aftrsn", "memoire-multi-marques-ingestion"]
---

# AFTRSN — Publication Notion (vue humaine, upsert idempotent depuis le wiki)

Second dérivé du wiki Git (en parallèle de pgvector, jamais en chaîne) : le workflow `LUMINA-MEMORY-INGESTION/PUBLISH-NOTION` republie chaque page `wiki/*.md` de GitHub dans une base Notion, pour donner aux humains une vue de lecture confortable — jamais éditée à la main, toujours régénérée depuis Git. Sert de pendant humain à l'ingestion pgvector décrite dans [[runbook-memoire-centrale-asp-memory-et-agents-aftrsn]] et [[memoire-multi-marques-ingestion]].

## Base Notion cible

Base « AI Automation — Knowledge » (teamspace AI-AUTOMATION), propriétés : `Page` (title), `Type` (select concept/synthese/sop), `Source` (url GitHub), `Tags` (multi), `Vault` (select), `Updated` (date). L'intégration n8n doit être ajoutée en **Connection** sur la base, sinon l'appel échoue en « database not found ».

## Chaîne (upsert par fichier)

```
Trigger → Query Notion DB → HTTP GitHub Trees → Split Out (tree)
        → Filter (wiki/*.md, exclut README) → Get a file → Set Document → Code (Parsing)
              ├─→ Build Notion payload → POST Notion          (créer la version fraîche)
              └─→ Query BY SOURCE → Split Out (results) → ARCHIVE PAGE   (archiver l'ancienne)
```

- **Idempotence = upsert par fichier** : pour chaque page wiki, on cherche sa page Notion existante par l'URL Source, on l'archive (`PATCH /v1/pages/{id}` avec `{"archived":true}`, `On Error = Continue`) puis on recrée la version fraîche (`POST /v1/pages`, header `Notion-Version: 2022-06-28`). Jamais de doublon, jamais de purge globale — on ne touche que la page concernée.
- **Code (Parsing)** extrait le frontmatter (titre, type, tags, vault, URL source, date) et isole le corps ; il ne doit recevoir **qu'une seule entrée** (Set Document) — un second flux branché dessus injecte des items parasites (voir pièges).
- **Build Notion payload** convertit le markdown en blocs Notion (titres, listes à puces/numérotées, citation, paragraphe ; nettoyage de la syntaxe markdown ; découpe à ~1990 caractères par bloc, 95 blocs max) et construit les propriétés — c'est lui qui produit l'**URL Source complète**, référence de matching pour la requête de recherche.
- **Query BY SOURCE** interroge la base Notion (`POST /v1/databases/{id}/query`) en filtrant sur `properties.Source.url` exactement égal à l'URL produite par Build Notion payload.

## Pièges résolus

- **Fil parasite `Query Notion DB → Code (Parsing)`** : un item non-wiki s'invitait dans le parsing → page fantôme (titre vide, URL tronquée). Fix : Code (Parsing) ne doit recevoir que la branche Set Document.
- **Expression `{{ }}` non évaluée dans le corps JSON du filtre** → envoyée en texte brut, 0 résultat. Fix : passer le body en **mode Expression** avec `JSON.stringify(...)`, pas en JSON statique.
- **URL source tronquée** (`…/blob/main/`) côté Code (Parsing) → ne matche jamais la page Notion existante. Fix : filtrer sur `payload.properties.Source.url` (produit par Build Notion payload), pas sur la valeur brute extraite par le parsing.
- **`README.md` capté par le filtre** `wiki/*.md` → l'exclure explicitement (condition « path ne contient pas README »).
- **Course archive/create** : ne jamais déclencher la branche d'archivage seule — Git reste la source de vérité, donc un run de publish complet suffit toujours à reconstruire Notion proprement.

## Dette connue

- Le filtre de publication n'est pas encore restreint à `status: active` (ligne commentée dans le Code de parsing) : en l'état, toute page `wiki/*.md` part vers Notion, y compris les brouillons — **contraire à la règle du CLAUDE.md du coffre** (une page sans promotion `status: active` ne devrait pas partir vers Notion). À corriger avant tout usage en continu.
- Notion reste optionnel et non porteur : une panne ou un retard de publication n'affecte jamais les agents (qui lisent pgvector, pas Notion).

<!-- AUTO-LIENS:début -->
## Voir aussi
- [[lumina-intake-robot-drive-github]] : AFTRSN · premier maillon de la chaîne — dépôt Drive classé et rangé dans `raw/` GitHub
- [[runbook-memoire-centrale-asp-memory-et-agents-aftrsn]] : AFTRSN · dérivé parallèle — ingestion du même wiki vers `asp_memory`/pgvector
- [[memoire-multi-marques-ingestion]] : AFTRSN · architecture multi-banques pgvector alimentée depuis le même wiki
<!-- AUTO-LIENS:fin -->
