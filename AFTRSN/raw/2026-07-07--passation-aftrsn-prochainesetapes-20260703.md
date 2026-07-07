---
type: raw
title: "PASSATION_AFTRSN_ProchainesEtapes_20260703"
source_url: "drive:1XNwf2-toZE0tNZgYfDBM7hZTx-qED6uN"
captured: 2026-07-07
vault: brands
brand: AFTRSN
immutable: true
---

# Passation — Prochaines étapes (03.07.2026)

**Statut :** Session du 03.07.2026 clôturée. Ce document sert de relais vers la prochaine session.

---

## 🧭 Boussole — toujours commencer ici

Avant toute action sur ce projet, lire **`FICHE_PROJET_AFTRSN_Automation.md`**. C'est le document compas : statut courant, deadlines (18 juillet J-21, 08.08 MVP), roadmap Étape 1→4, décisions ouvertes (DA-001, DA-002). Ce présent document ne le remplace pas — il complète la transition de session et doit être lu en second, après la Fiche Projet.

⚠️ La Fiche Projet n'est pas encore mise à jour avec le travail du 03.07.2026 après-midi (voir §2 ci-dessous) — elle reflète l'état de la matinée. Mise à jour faite en parallèle de ce document (nouveaux docs ajoutés à "Déjà fait").

---

## 1 · Ce qui a été fait aujourd'hui (03.07.2026, matin + après-midi)

**Matin — build W-1 (voir `RECAP_Session_20260703_W1-n8n-Build.md`) :** construction complète des 14 nodes du workflow AFTRSN-Event-Creator.

**Après-midi :**
- Corrigé et vérifié en exécution réelle : "Notion — Injecter checklist" (bug méthode HTTP POST → **PATCH**), upsert Venue (4 sous-bugs de câblage/filtre/mode corrigés).
- Identifié et documenté (non corrigé) : "Create an event" — référence cassée vers un dossier Drive via une branche devenue cul-de-sac (`Merge - Dossiers média` sans sortie).
- Classé et taggué AFTRSN-Event-Creator dans n8n (déplacé dans `AFTRSN-04-AUTOMATION`, tags 🌞🔄🔵), et rédigé sa fiche complète : `Fiche_AFTRSN-Event-Creator_20260703.md`.
- Discussion approfondie sur le découpage de W-1 en sous-workflows par logique métier → `W1_Decoupage_Sous-Workflows_20260703.md` (draft, partiellement dépassé par la clarification business ci-dessous).
- Clarification complète et dictée par Karter du vrai process métier bout-en-bout (création d'édition → pack promo → publicités → newsletter → post-event). Points clés à retenir :
  - Une date reçue du lieu = un événement = déclenchement automatique complet de l'administratif (Drive, checklist, fiche, mémoire), **sans validation humaine**, identique à chaque fois.
  - Pack promo (flyer + déclinaisons) : travail humain sur Canva, validé par la **core team** (terme anglais, pas "coaching team") avant diffusion manuelle. Dossier photos & vidéos : rempli **après** l'événement, usage post-promo — ne pas confondre les deux.
  - Arborescence Drive : Année/Mois/Édition — seule la date groupe les dossiers, pas la ville.
  - Budget : auto-brouillon (moyennes historiques + capacité venue, tirée de la page Venue), finalisé par un humain.
  - Publicités : auto-créées avec paramètres quasi-identiques (seule la ville change, Zürich = référence), jamais publiées sans revue humaine ; une fois live, monitoring + propositions d'optimisation par un agent (via Hermès), exécution seulement après validation Karter.
  - Site web (Wix) : page événement auto-construite, publication manuelle (ou auto sur choix futur de Karter). Newsletter : déclenchée à la publication du site, uniquement si un segment de contacts existe déjà pour la ville (toujours le cas pour Zürich) ; segmentation aujourd'hui manuelle dans Wix (pas de CRM) — l'automatisation de la segmentation est explicitement reportée à un futur workflow séparé.
- Clarifié l'identité de **Hermès** (voir `Hermes_Clarification_20260703.md`) : plateforme d'exécution d'agent IA autonome partagée entre marques (pas un agent du roster AFTRSN, pas propre à AFTRSN), hébergée à `hermes.aftersunpeople.com`, gouvernance critique (dépense/envoi/publication/suppression → validation Karter obligatoire, sans exception) intégrée directement dans le code d'exécution.
- Réception et absorption des deux documents canoniques **SYSTEM-CANON v1.1** et **BIBLE 360° LUMINA OS v1.1**, qui font désormais foi sur l'architecture, la gouvernance et le roster d'agents.
- Corrigé le roster d'agents dans la page Notion **🤖 Agents** : 8 agents confirmés (Maestro + 7 spécialistes, dont AFTRSN-CHANNEL-CONTENT qui manquait dans ma première correction).

## 2 · Fichiers produits aujourd'hui

| Fichier | Rôle |
|---|---|
| `Fiche_AFTRSN-Event-Creator_20260703.md` | Fiche technique complète du workflow W-1, format Bible §E |
| `W1_Decoupage_Sous-Workflows_20260703.md` | Draft de réflexion sur le découpage en sous-workflows (à réconcilier avec le process métier clarifié, voir §4) |
| `Hermes_Clarification_20260703.md` | Historique de l'investigation Hermès + conclusion + point ouvert |
| `PASSATION_AFTRSN_ProchainesEtapes_20260703.md` | Ce document |

## 3 · Prochaines étapes (priorité décroissante)

1. **"Create an event" (W-1)** — trancher entre les deux options documentées dans la Fiche : câbler `Merge2` (patch technique) ou repointer la Description vers la fiche Notion plutôt que le dossier Drive (solution alignée sur le process métier clarifié, dépendance croisée supprimée à la racine). Cette deuxième option est recommandée.
2. **Nettoyage W-1** — supprimer/câbler `Merge2` selon la décision ci-dessus ; supprimer les doublons de test (Venue "Mini Café Bar", pages Edition "EP5").
3. **Réconciliation `W1_Decoupage_Sous-Workflows_20260703.md`** — ce document ne reflète pas encore le process métier détaillé clarifié aujourd'hui (déclenchement par date, distinction pack promo/photos-vidéos, budget/pubs auto-brouillon, site/newsletter). À confirmer avec Karter avant de le reprendre — pas redemandé explicitement après la clarification métier.
4. **Décisions Karter en attente** (ne pas trancher seul) :
   - DA-001 : publier `AFTRSN-Comptable-Finance` / `AFTRSN-Qualite-Compliance`, ou documenter pourquoi non.
   - DA-002 : suppression de `ZZZ_20260702_tmp_hermes_reach_test`.
   - Nettoyage suggéré par la Bible : `ZZZ_20260703_tmp_telegram_chatid`.
5. **Hermès — point ouvert** : vérifier dans la bibliothèque Skills de Hermès si un runbook "optimisation publicitaire" existe déjà ; sinon le créer avec la gouvernance déjà actée (auto-brouillon + validation Karter avant toute action critique).
6. **Roadmap standard** (Fiche Projet, Étapes 1→4) : W-5 Email-Responder, W-2 Venue-Coordinator, W-3 Artist-Coordinator, W-6 Budget-Tracker, W-4 Marketing-Planner, W-7 Knowledge-Capture — inchangé, voir Fiche Projet pour le détail.

---

## 4 · Protocole — création et sauvegarde automatique des marches à suivre et POS

**Instruction permanente pour toutes les prochaines sessions sur ce projet.**

Dès qu'une session de travail produit une explication réutilisable (mode d'emploi pratique généralisable, ou procédure technique détaillée validée), ce contenu doit être **automatiquement extrait en fichier(s) `.md` séparé(s) et sauvegardé(s) dans le dossier projet** — jamais laissé uniquement dans la conversation ou noyé dans un récap de session.

**Deux types de document, deux conventions de nommage (déjà en usage dans ce dossier) :**

- **Marche à suivre** — mode d'emploi pratique, réutilisable, sans date (document vivant, mis à jour sur place si la procédure évolue) :
  `<Scope>_MarcheASuivre_<Sujet-en-Kebab-Case>.md`
  `Scope` = `AFTRSN` (spécifique au projet) ou `Generique` (réutilisable au-delà d'AFTRSN).
  *Exemples existants :* `AFTRSN_MarcheASuivre_Construction-n8n.md`, `Generique_MarcheASuivre_Construction-n8n-Navigateur.md`.

- **POS (Procédure Opérationnelle Standardisée)** — procédure technique détaillée, snapshot d'un état vérifié à un instant donné, **datée** :
  `POS-<SCOPE>_<Sujet-en-Kebab-Case>_<AAAAMMJJ>.md`
  `SCOPE` = `GENERIQUE` / `AFTRSN` / `LUMINA` (voir Bible §K pour les exemples déjà référencés côté LUMINA OS).

**Règle de la paire systématique (spécifique + générique) :**

Chaque fois qu'une marche à suivre **spécifique à AFTRSN** est créée (`AFTRSN_MarcheASuivre_...`), produire **automatiquement une seconde version générique** en parallèle (`Generique_MarcheASuivre_...`), qui reprend la même procédure débarrassée des éléments propres à AFTRSN (noms de bases Notion, comptes Google spécifiques, IDs de workflow, etc.) pour être réutilisable sur un autre projet/marque géré via LUMINA OS. C'est déjà le patron observé dans ce dossier (`AFTRSN_MarcheASuivre_Construction-n8n.md` + `Generique_MarcheASuivre_Construction-n8n-Navigateur.md`) — à systématiser désormais pour toute nouvelle marche à suivre, pas seulement celle-ci. Si la procédure est déjà générique par nature (ne touche à aucun élément propre à AFTRSN), un seul document `Generique_...` suffit — pas de doublon artificiel.

**Sections obligatoires dans tout `MarcheASuivre` ou `POS` produit désormais :**

Chaque marche à suivre et chaque POS doit inclure, en plus de sa procédure, trois sections dédiées :
- **Difficultés rencontrées** — les blocages, erreurs, ambiguïtés effectivement rencontrés en construisant/exécutant la procédure (pas hypothétiques).
- **Solutions implémentées** — ce qui a concrètement résolu chaque difficulté (diagnostic exact + correctif appliqué), pas seulement "corrigé".
- **Leçons apprises** — le principe généralisable à retenir pour éviter de retomber dans le même piège la prochaine fois, formulé pour être utile même sans se souvenir du détail de l'incident d'origine.

**Autres règles d'application :**

1. **Détection automatique** — dès qu'une explication étape-par-étape émerge pendant une session (que ce soit en réponse à une question ou en sous-produit d'un débogage), évaluer si elle est réutilisable. Si oui, l'extraire en document séparé plutôt que de la laisser uniquement dans le chat ou dans un récap ponctuel.
2. **Sauvegarde effective** — toujours écrire dans `/Users/karter/Claude/Projects/AFTRSN Automation/` (jamais seulement dans le scratchpad), après brouillon dans le scratchpad si le document est long.
3. **Référencement obligatoire** — chaque nouveau document (y compris sa paire générique) doit être ajouté à la section "✅ Déjà fait > Architecture & documentation" de `FICHE_PROJET_AFTRSN_Automation.md`, avec son nom de fichier exact, au moment de sa création (pas différé).
4. **Portée LUMINA OS vs métier AFTRSN** — si le contenu concerne l'architecture ou la gouvernance du système LUMINA OS lui-même (pas seulement un processus métier AFTRSN), signaler à Karter que le document mérite aussi d'être poussé vers le dépôt canonique (repo `wiki/`) pour ingestion dans la collection `canon` — un document de session seul, ou une sauvegarde en mémoire épisodique, ne corrige jamais durablement la connaissance de fond du système (règle établie dans le SYSTEM-CANON, §5 : episodic ≠ canon).
5. **Présentation à l'utilisateur** — comme tout livrable, présenter le fichier via le mécanisme standard (`present_files`) une fois finalisé, pas seulement le décrire en texte.

---

## 5 · Difficultés rencontrées, solutions implémentées, leçons apprises — session du 03.07.2026

| # | Difficulté rencontrée | Solution implémentée | Leçon apprise |
|---|---|---|---|
| 1 | "Notion — Injecter checklist" : `400 invalid_request_url`. Piste initiale (double header `Notion-Version`) écartée sans effet. | Vérifié la doc API Notion : l'endpoint "Append block children" exige la méthode **PATCH**, pas POST. Changé, testé, checklist injectée avec succès. | Ne pas se fier au premier symptôme plausible (header dupliqué) — vérifier l'API officielle avant de patcher à l'aveugle. Un correctif qui ne change rien à l'erreur doit être retiré, pas empilé sur d'autres tentatives. |
| 2 | Upsert Venue dupliquait des pages à chaque run — 4 causes distinctes empilées (mauvais câblage, filtre Notion "Equals" cassé sur un champ titre, connexion résiduelle de bypass, champs restés en mode "Fixed" au lieu de "Expression"). | Diagnostiqué et corrigé chaque cause séparément plutôt que de chercher un correctif unique. | Un bug de duplication peut avoir plusieurs causes indépendantes empilées — corriger la première ne suffit pas ; retester après chaque correctif avant de conclure. |
| 3 | "Create an event" : `Invalid expression` / `No path back to referenced node` — référence vers `$('Create folder')` mais aucun chemin de connexion réel jusqu'à ce nœud. | Diagnostic par inspection directe des edges du canvas via JavaScript (`document.querySelectorAll('.vue-flow__edge')`), pas par capture d'écran — a révélé que `Merge - Dossiers média` n'a aucune sortie (cul-de-sac hérité d'une correction de session précédente). Correctif complet non terminé (voir §3, priorité 1). | Les captures d'écran / l'inspection visuelle du canvas n8n ne suffisent pas pour vérifier le câblage réel — toujours confirmer via une requête JS sur le DOM. Une référence croisée `$('Node')` exige un chemin de connexion réel, même si le nœud référencé "tourne" sans erreur propre. |
| 4 | Confusion sur l'identité de "Hermès" — plusieurs hypothèses fausses, y compris une réponse incomplète de Maestro et de l'agent Marketing eux-mêmes. | Escaladé explicitement à Maestro (chaîne d'autorité correcte selon la gouvernance), puis vérifié indépendamment par lecture du code du workflow `LUMINA-Hermes-Exec` et visite directe du site — pas arrêté à la première réponse d'agent. | Ne pas prendre la première réponse d'un agent comme vérité si la gouvernance indique qu'un autre agent (ou une vérification directe) détient une connaissance plus complète — un agent ne connaît souvent que son propre point d'accès à un système, pas le système entier. Vérifier par le code/l'accès direct reste plus fiable que le témoignage d'un agent. |
| 5 | Compréhension initiale du process métier de création d'édition partiellement fausse sur plusieurs points (batching de dates vs une-date-un-événement ; confusion pack promo / dossier photos-vidéos ; "coaching team" au lieu de "core team"). | Karter a dicté le process réel bout-en-bout ; questions de clarification posées avant toute modification technique ; corrections intégrées une à une. | Le process métier doit être clair et validé **avant** de concevoir ou modifier un workflow technique — un dépendency graph technique doit refléter le vrai process métier, jamais l'inverse. Ne pas supposer qu'une compréhension initiale (même documentée) est correcte sans confirmation explicite. |
| 6 | `notion-update-page` : `validation_error` en batchant 3 `content_updates` dans un seul appel. | Refait en appels séparés, un seul `content_update` par appel. | Les opérations Notion en batch peuvent se comporter de façon imprévisible (ex. concaténation apparente des `old_str`) — préférer des appels unitaires en cas de doute, surtout si le contenu de la page a pu changer entre-temps. |
| 7 | Déplacement du workflow dans Drive/n8n : clic sur un dossier au nom trop proche (`LUMINA-04-AUTOMATION` au lieu de `AFTRSN-04-AUTOMATION`) dans un menu déroulant tronqué. | Requête de recherche resaisie avec un préfixe plus spécifique (`AFTRSN-04`) pour obtenir une correspondance unique avant de cliquer. | Sur les menus déroulants avec noms similaires tronqués, affiner la recherche jusqu'à une correspondance unique plutôt que de cliquer sur le premier résultat plausible. |
| 8 | Clics peu fiables sur les menus contextuels "⋮" de n8n via capture d'écran + clic par coordonnées (options partiellement illisibles). | Localisation de l'élément par texte (`find`) puis clic programmatique via JS (`querySelectorAll('[role="menuitem"], li')` + `.click()`) plutôt que par coordonnées. | Pour les menus/dropdowns peu fiables à l'écran, préférer un ciblage DOM programmatique (par texte/rôle) à un clic par coordonnées — plus robuste et reproductible. |

---
*Document rédigé en fin de session le 03.07.2026.*
