---
type: raw
title: "PASSATION_AFTRSN_ProchainesEtapes_20260704"
source_url: "drive:1yeSHkcLMZDTmjKzRV0rlhoqzX68I-4iC"
captured: 2026-07-07
vault: brands
brand: AFTRSN
immutable: true
---

# Passation — Prochaines étapes (04.07.2026)

**Statut :** Session du 04.07.2026 clôturée. Ce document sert de relais vers la prochaine session.

---

## 1 · Boussole — toujours commencer ici

Avant toute action sur ce projet, lire **`FICHE_PROJET_AFTRSN_Automation.md`** (« Fiche Projet — AFTRSN Automation »). C'est le document compas : statut courant, deadlines (18 juillet J-21 : venue + 2 artistes confirmés ; 08.08 : MVP), roadmap Étapes 1→4, décisions ouvertes (DA-001, DA-002, DA-004). Le présent document ne le remplace pas — il complète la transition de session et se lit en second, après la Fiche Projet. La Fiche Projet est à jour au 04.07.2026 au soir.

---

## 2 · Ce qui a été fait aujourd'hui (04.07.2026)

1. **Correctif « Create an event »** (option B) : description du Calendar event → URL de la fiche Notion (hub central) ; `Merge2` supprimé ; dépendance croisée éliminée à la racine. **Décision prise, appliquée, testée, publiée — dossier clos.**
2. **🎭 The Backstage déclaré canonique** (teamspace AFTER SUN PEOPLE - HQ) ; 4 nodes Notion repointés vers les DB 🎭 en mode By ID ; doublons de test du 03.07 purgés.
3. **Architecture par processus métier** actée (`AFTRSN_Architecture_Processus_20260704.md`) : chaîne P1 Création d'édition → P2 Pack promo → P3 Publicités → P4 Site & Newsletter → P5 Post-event + transverses. Draft W-1a→W-1g remplacé.
4. **Décisions LUMINA OS (Karter)** — à pousser au canon (`wiki/`, collection `canon`) : Maestro délègue / **Hermès exécute** toutes les actions automatiques (option b : pilotage direct des outils) ; **cycle de supervision Maestro ↔ Hermès** en 6 étapes (critères d'acceptation → exécution → rapport des skills → contrôle → acceptation en banque de données ou rejet diagnostiqué → amélioration continue) ; **numérotation toujours à partir de 1** ; **aucun code technique (« W-5 », « P1.x »…) dans ce qui est adressé à Karter**.
5. **Découpage physique de la Création d'édition** : `AFTRSN-Event-Creator` = orchestrateur (9 nodes) + 6 sous-workflows métier, dossier `AFTRSN-02-BUSINESS AUTOMATION / AFTRSN-PROCESS-01-Event-Creation` (rangement Karter). **Testé end-to-end (ZZTEST, purgé ensuite) et PUBLIÉ 🟢** (« v1.0 — P1 Event-Creation »). 4 bugs hérités corrigés. Convention `AFTSN` → `AFTRSN` propagée (Drive, workflow, checklist, template Notion, docs).
6. **Email-Responder construit, testé et PUBLIÉ 🟢** (`AFTRSN-Email-Responder`, dossier `AFTRSN-PROCESS-02-Email-Responder`) : lecture Gmail horaire → drafts via Secretary → notification Telegram détaillée (format validé Karter, sans codes techniques) → mémoire. Jamais d'envoi automatique. **→ Étape 1 de la roadmap TERMINÉE (avec 4 jours d'avance).**
7. Note de connaissance « runbook » ajoutée à l'Obsidian Lumina Inbox.

## 3 · Fichiers produits / mis à jour aujourd'hui

| Fichier | Rôle |
|---|---|
| `AFTRSN_Architecture_Processus_20260704.md` | Architecture par processus P1→P5, règles de construction, cycle Maestro↔Hermès |
| `Fiche_AFTRSN-Email-Responder_20260704.md` | Fiche technique du processus Réponse email (format Bible §E) |
| `POS-GENERIQUE_Decoupage-Workflow-n8n-Orchestrateur-Sous-Workflows_20260704.md` | POS : découper un monolithe n8n via l'API REST (générique par nature) |
| `POS-AFTRSN_Repondeur-Email-Drafts-Agent_20260704.md` + `POS-GENERIQUE_Repondeur-Email-Drafts-Agent_20260704.md` | POS paire : pattern répondeur email draft-only via agent |
| `AFTRSN_MarcheASuivre_Construction-n8n.md` + `Generique_MarcheASuivre_Construction-n8n-Navigateur.md` | §6 enrichis : API REST interne, autosave concurrent, sémantique 0 items, contrats de sortie, ordre de publication, sortie Gmail parsée, @rawdatabot |
| `Fiche_AFTRSN-Event-Creator_20260703.md` | Note de restructuration + nouvel emplacement |
| `W1_Decoupage_Sous-Workflows_20260703.md` | Marqué remplacé, renuméroté 1→10 |
| `FICHE_PROJET_AFTRSN_Automation.md` | Boussole, à jour au 04.07 au soir |
| `PASSATION_AFTRSN_ProchainesEtapes_20260704.md` | Ce document |

## 4 · Prochaines étapes (priorité décroissante)

1. **Reprendre la décision sur « Create an event »** (demande explicite de Karter en fin de session). ⚠️ **Note d'état** : cette décision a été prise et exécutée le 04.07 — option B (description du Calendar event → fiche Notion), testée end-to-end et publiée en production. Si la reprise vise autre chose (revoir l'option retenue, enrichir la description, ajouter le lien Drive en plus du lien Notion — « option hub » documentée dans l'ancien draft de découpage §8), commencer par clarifier avec Karter **ce qui doit être repris exactement** avant de toucher au workflow publié.
2. **Roadmap Étape 2 (semaine du 09–15.07)** : Venue-Coordinator puis Artist-Coordinator (critiques pour la deadline du 18.07 : venue + 2 artistes confirmés), puis Budget-Tracker.
3. **Pousser au canon** (`wiki/`, collection `canon`) : les décisions LUMINA OS du §2.4, le nouveau rangement n8n (la Bible §C référence encore l'ancien), et les deux POS génériques du jour. Un document de session ne corrige pas la connaissance de fond (SYSTEM-CANON §5 : episodic ≠ canon).
4. **Décisions Karter en attente** (ne pas trancher seul) : DA-001 (publier ou non Comptable-Finance / Qualite-Compliance), DA-002 (supprimer `ZZZ_20260702_tmp_hermes_reach_test`), DA-004 (supprimer `ZZZ_20260704_tmp_telegram_chatid`, désactivé).
5. **Hermès** : vérifier/créer le runbook « optimisation publicitaire » (point ouvert depuis le 03.07) ; à terme, migrer la logique des processus publiés en runbooks Hermès (option b actée) — les workflows n8n validés servent de référence.
6. **Avant le premier run réel de la Création d'édition** : remettre le payload épinglé du trigger sur les vraies données EP5 (il est resté sur ZZTEST).
7. **Surveiller les premiers runs horaires de l'Email-Responder** en production (qualité des drafts sur de vrais emails, volume de notifications).

## 5 · Protocole permanent — marches à suivre et POS (instruction pour toutes les prochaines sessions)

**Après chaque étape validée**, toute explication réutilisable produite pendant le travail (mode d'emploi pratique généralisable, ou procédure technique détaillée vérifiée) doit être **automatiquement extraite en fichier(s) `.md` et sauvegardée dans `/Users/karter/Claude/Projects/AFTRSN Automation/`** — jamais laissée uniquement dans la conversation ou noyée dans un récap.

**Deux types de document, deux conventions de nommage :**
- **Marche à suivre** — mode d'emploi pratique, réutilisable, **sans date** (document vivant, mis à jour sur place) : `<Scope>_MarcheASuivre_<Sujet-en-Kebab-Case>.md`, `Scope` = `AFTRSN` ou `Generique`.
- **POS** (Procédure Opérationnelle Standardisée) — procédure technique détaillée, snapshot **daté** d'un état vérifié : `POS-<SCOPE>_<Sujet-en-Kebab-Case>_<AAAAMMJJ>.md`, `SCOPE` = `GENERIQUE` / `AFTRSN` / `LUMINA`.

**Règle de la paire systématique :** chaque document **spécifique AFTRSN** (marche à suivre ou POS) est produit avec **une seconde version générique** en parallèle, débarrassée des éléments propres à AFTRSN (IDs, comptes, noms de bases), réutilisable pour toute autre marque gérée via LUMINA OS. Si la procédure est générique par nature, un seul document `Generique_`/`POS-GENERIQUE` suffit — pas de doublon artificiel.

**Sections obligatoires dans chaque document produit :**
- **Difficultés rencontrées** — les blocages, erreurs et ambiguïtés effectivement rencontrés (pas hypothétiques).
- **Solutions implémentées** — ce qui a concrètement résolu chaque difficulté (diagnostic exact + correctif appliqué).
- **Leçons apprises** — le principe généralisable, formulé pour rester utile sans se souvenir de l'incident d'origine.

**Règles d'application :**
1. **Détection automatique** — dès qu'une explication étape-par-étape émerge (réponse à une question ou sous-produit d'un débogage), évaluer sa réutilisabilité et l'extraire si oui, sans attendre la fin de session.
2. **Sauvegarde effective** dans le dossier projet (jamais seulement dans le scratchpad).
3. **Référencement obligatoire** — chaque nouveau document (y compris sa paire générique) est ajouté à « ✅ Déjà fait › Architecture & documentation » de `FICHE_PROJET_AFTRSN_Automation.md` au moment de sa création.
4. **Portée LUMINA OS vs métier AFTRSN** — si le contenu concerne l'architecture/gouvernance du système LUMINA OS lui-même, signaler à Karter qu'il mérite d'être poussé vers le dépôt canonique (`wiki/` → collection `canon`) — un document de session seul ne corrige jamais durablement la connaissance de fond (SYSTEM-CANON §5).
5. **Présentation à l'utilisateur** — chaque document finalisé est présenté via le mécanisme standard (`present_files`), pas seulement décrit en texte.

## 6 · Difficultés rencontrées, solutions implémentées, leçons apprises — session du 04.07.2026

| # | Difficulté | Solution | Leçon |
|---|---|---|---|
| 1 | Deux arbres « The Backstage » aux DB homonymes ; le workflow écrivait dans le déprécié | Inspection des ancêtres des pages via le connecteur Notion ; décision Karter (🎭 canonique) ; repointage **By ID** | Identifier les ressources par ID/arborescence, jamais par nom ; faire trancher le canonique par l'humain avant de repointer |
| 2 | Session n8n expirée en cours de travail (autosave « Unauthorized », options en erreur) | Reconnexion Karter ; état revérifié après rechargement | Toujours revérifier l'état **sauvegardé** (updatedAt + rechargement), pas l'état affiché |
| 3 | Mutation du store Pinia + cmd+s = rien de sauvegardé ; plus tard, un PATCH API **écrasé par l'autosave** de l'éditeur ouvert | Toute écriture par l'API REST (`browser-id`) + **rechargement de l'éditeur immédiatement après chaque PATCH** | L'éditeur ouvert est un écrivain concurrent ; API > UI > store, et relire l'état serveur après coup |
| 4 | Découpage de 27 nodes irréaliste à la souris | Construction programmatique (`POST /rest/workflows`), réécriture des références croisées, vérification « zéro leftover » | Un refactoring structurel se fait par l'API avec vérification automatisée (voir POS dédié) |
| 5 | Recherche Notion à 0 résultat → chaîne morte **en silence** (run « Success ») | Always Output Data sur le node de recherche + code de vérification testant la présence d'un `id`, pas `items.length` | Tester explicitement le chemin « résultat vide » d'un upsert ; un test vert ne prouve que le chemin emprunté |
| 6 | Sous-workflow renvoyant N items → mappings `undefined` en aval | `executeOnce` sur le node de sortie + références `.first()` dans l'orchestrateur | Contrat : un sous-workflow d'étape renvoie exactement 1 item, garanti mécaniquement |
| 7 | Expressions héritées malformées (sans `=` ni `}}`) → texte brut en sortie | Audit et réécriture propre des assignments | Le texte brut `{{…}}` dans un output est la signature du bug ; auditer tout workflow hérité |
| 8 | Node Gmail en 403 malgré credential valide | Activation de l'API Gmail dans la console Google Cloud du projet OAuth | Un 403 avec credential valide = API désactivée côté fournisseur, pas un bug de workflow |
| 9 | Parseur MIME écrit à l'aveugle : 100 % des emails filtrés | Inspection de la vraie sortie d'exécution (champs déjà parsés `text`/`html`/`from.text`) et réécriture | Ne jamais coder contre un format supposé — exécuter une fois, regarder, coder contre l'observé |
| 10 | Notification jugée inutilisable par Karter (codes techniques, pas de résumés) | Sortie d'agent balisée (résumés + draft en un appel), notification par email traité, zéro jargon | Le format des notifications est une exigence produit ; règle permanente : pas de codes techniques adressés à Karter |
| 11 | chat_id Telegram introuvable (@username refusé par l'API bots) | Astuce Karter : message à **@rawdatabot** qui renvoie le `chat.id` | Le plus court chemin vers un identifiant utilisateur est souvent un bot d'introspection, pas un workflow capteur |

---
*Document rédigé en fin de session le 04.07.2026.*
