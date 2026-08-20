---
type: wiki
title: "AFTRSN — Robot d'intake LUMINA (Drive → GitHub, classification OpenAI)"
status: draft
publish: none
vault: brands
brand: AFTRSN
sources: [AFTRSN/raw/2026-06-24--lumina-systeme-reference.md]
updated: 2026-08-20
related: ["runbook-memoire-centrale-asp-memory-et-agents-aftrsn", "lumina-publish-notion-vue-humaine", "memoire-multi-marques-ingestion"]
---

# AFTRSN — Robot d'intake LUMINA (Drive → GitHub, classification OpenAI)

Premier maillon de la chaîne de connaissance LUMINA : le workflow `LUMINA-INTAKE-ROUTER/DRIVE→GITHUB` vide périodiquement une boîte de dépôt Google Drive (« Lumina Inbox ») en classant chaque fichier puis en l'écrivant dans le bon coffre GitHub (`raw/`), avant de le supprimer de Drive. C'est ce qui alimente en amont la rédaction du wiki (Claude Code) puis l'ingestion pgvector — voir [[runbook-memoire-centrale-asp-memory-et-agents-aftrsn]] et [[memoire-multi-marques-ingestion]].

## Chaîne

`Schedule Trigger (horaire) → Search files and folders (liste tout l'inbox Drive) → Download file → Extract from File → classification OpenAI (chat completions, JSON forcé) → parsing → If Rejected → Create a file (GitHub) → Delete a file (Drive)`

- Le trigger planifié draine le **backlog entier** à chaque run (liste tous les fichiers présents, pas seulement les nouveaux) : un seul workflow gère aussi bien le rattrapage que le flux courant.
- La classification (modèle `gpt-4o-mini`, température 0, sortie JSON forcée) reçoit le texte extrait du fichier et renvoie `{vault, brand, reason}` : le fichier part vers `ai-automation`, `brands` (avec sous-dossier de marque) ou `personal`, ou est rejeté.
- Le parsing construit le chemin cible (`raw/AAAA-MM-JJ--slug.md`, ou `<brand>/raw/…` pour le vault `brands`) et le frontmatter `type: raw`, et réunit dans un seul item toutes les infos nécessaires aux deux nodes suivants (repo, path, contenu, id Drive du fichier).
- **Garde-fou d'ordre** : la suppression Drive n'intervient qu'**après** une écriture GitHub réussie (`Create a file` en `On Error = Stop Workflow`) — un fichier n'est jamais perdu ; si l'écriture échoue, il reste dans l'inbox pour le run suivant.

## Pièges résolus

- **Classifieur Gemini en 503** (palier gratuit saturé, même avec retry) → basculé sur **OpenAI** (`gpt-4o-mini`, coût négligeable) ; adapter le parsing au format de réponse (`choices[0].message.content`, pas `candidates[...]` façon Gemini).
- **Un seul fichier traité par run** → remplacer le déclencheur de test par un node qui **liste tout l'inbox** (Search files and folders, Return All).
- **Un node Code en « Run Once for All Items » ne ressort qu'un item** quand il traite une liste → repasser en **Run Once for Each Item**, avec appariement par `$('NomDuNode').item` plutôt que `.first()`/`.all()[0]`.
- **Suppression Drive en échec (« Referenced node doesn't exist »)** → référencer le nom réel du node de liste (`Search files and folders`), pas un nom générique.
- **GitHub 422 « sha wasn't supplied »** = le fichier existe déjà (cas d'un rattrapage sur un backlog déjà rangé) → tolérer en `On Error = Continue` le temps de vider le backlog, puis remettre `Stop Workflow` en régime normal.

## Marche à suivre — recréer le robot

1. Schedule Trigger (ex. toutes les heures).
2. Google Drive → Search files and folders (dossier inbox, Return All, `trashed = false`).
3. Google Drive → Download file (`File ID` = `{{ $json.id }}`, mode Expression).
4. Extract from File (texte, champ binaire `data`).
5. HTTP → appel OpenAI chat completions (JSON forcé, prompt système = grille de classement des vaults/marques) ; activer Retry On Fail.
6. Code (Run Once for Each Item) : parse la classification, apparie fichier + texte source, construit `path`/frontmatter/`repo`, calcule le rejet.
7. IF sur le flag de rejet.
8. GitHub → Create a file (repo par URL, `On Error = Stop Workflow`).
9. Google Drive → Delete a file (`File ID` de l'item apparié dans le node Code).
10. Tester en exécution manuelle, puis activer le workflow.

<!-- AUTO-LIENS:début -->
## Voir aussi
- [[runbook-memoire-centrale-asp-memory-et-agents-aftrsn]] : AFTRSN · suite de la chaîne — ingestion du wiki vers `asp_memory`/banques pgvector
- [[lumina-publish-notion-vue-humaine]] : AFTRSN · autre sortie dérivée du wiki, vue humaine Notion
- [[memoire-multi-marques-ingestion]] : AFTRSN · architecture multi-banques alimentée en aval de cet intake
<!-- AUTO-LIENS:fin -->
