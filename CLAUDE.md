# CLAUDE.md — Coffre `brands`

> **Schéma de CE coffre.** Zone *marques* : un **dossier par marque**, contenant sa connaissance, ses **agents** et ses **skills**.
> ⭐ **C'est ce coffre qui alimente la mémoire des agents (Postgres/pgvector).**
> Routage inter-coffres : `lumina-meta/ROUTING.md`. Coffre autonome (repo séparé). Source = ces fichiers ; GitHub = backup.

---

## 1. Périmètre

Tout ce qui est **propre à une marque** : identité, positionnement, ton de voix, produits, clients, stratégie, assets, **agents** et **skills** de la marque.

**Hors périmètre →** technique IA réutilisable hors marque → `ai-automation`. Perso/business générique → `personal`.

---

## 2. Structure : un dossier par marque

```text
brands/
├── CLAUDE.md           ← ce fichier
├── index.md            ← carte des marques + de leurs concepts
├── log.md              ← journal du coffre
└── AFTRSN/             ← une marque = un dossier
    ├── raw/            ← sources curées & immuables (propres à la marque)
    ├── wiki/           ← pages propres (LLM) — connaissance de la marque
    ├── agents/         ← définitions d'agents de la marque
    └── skills/         ← skills de la marque
```

Ajouter une marque = créer un dossier `brands/<MARQUE>/` avec la même structure interne.

---

## 3. Isolation des marques (NON NÉGOCIABLE)

Décision 2026-06-20 : **dossiers + tag `brand` obligatoire, escalade en instance dédiée à la demande.**

- **À l'ingestion :** chaque page/chunk est **estampillé `brand: <MARQUE>`** dans le frontmatter. Le pipeline Notion→Postgres propage ce tag dans `asp_memory`.
- **À la récupération :** la requête pgvector **filtre toujours par `brand`** côté serveur (n8n). L'étanchéité **ne dépend jamais du seul System Prompt**.
- **Escalade :** si une marque exige une isolation dure (client externe, données sensibles) → la promouvoir vers sa propre table/instance. Journaliser dans `lumina-meta/log.md`.

> Comme les marques sont des **dossiers** d'un même coffre, l'étanchéité se fait par dossier/tag — acceptable pour des marques internes.

---

## 4. Conventions

### 4.1 Frontmatter `wiki/` (noter `brand` rempli)
```yaml
---
type: wiki
title: "Nom du concept / synthèse"
status: draft          # draft | active | retired
publish: none          # none | notion
vault: brands
brand: AFTRSN          # OBLIGATOIRE dans ce coffre
sources: [AFTRSN/raw/2026-06-20--brief.md]
updated: 2026-06-20
---
```

### 4.2 Agents & skills
- `agents/<nom>.md` : rôle, périmètre, **droits de lecture restreints au dossier de la marque** (rappel à inscrire dans le system prompt de l'agent), outils/MCP autorisés.
- `skills/<nom>.md` : procédure réutilisable de la marque.
- Tâches critiques (coûts, clés API, suppression mémoire) → **gate CEO**.

---

## 5. Workflows

Identiques au modèle (voir `../ai-automation/CLAUDE.md` §4–6 pour le détail **Ingest / Q&A / Lint**), avec **une obligation en plus** : vérifier que **`brand` est renseigné** sur chaque page `wiki/` avant promotion `active` / `publish: notion`. Une page sans `brand` ne doit jamais partir vers Notion/Postgres.

**Lint spécifique marques :** détecter toute page `wiki/` sans `brand`, toute fuite potentielle (référence d'une marque dans le dossier d'une autre), tout agent dont le périmètre de lecture déborde son dossier.

---

## 6. `log.md` — format
```text
## [2026-06-20] ingest | AFTRSN | Brief de positionnement
## [2026-06-20] lint   | AFTRSN | 1 page sans brand corrigée
## [2026-06-20] agent  | AFTRSN | Création agent "community-manager"
```

---

*CLAUDE.md `brands` v2.2 — 2026-06-20.*
