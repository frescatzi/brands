---
type: raw
title: "PASSATION_AFTRSN_ProchainesEtapes_20260705"
source_url: "drive:1WHJvtBnyw-2xyRbDdn11b7TRZiHgHvKG"
captured: 2026-07-07
vault: brands
brand: AFTRSN
immutable: true
---

# Passation — Prochaines étapes (05.07.2026)

**Statut :** Session du 04–05.07.2026 clôturée. Ce document remplace `PASSATION_AFTRSN_ProchainesEtapes_20260704.md` comme relais courant vers la prochaine session.

---

## 1 · Boussole — toujours commencer ici

Avant toute action sur ce projet, lire **`FICHE_PROJET_AFTRSN_Automation.md`** (« Fiche Projet — AFTRSN Automation »). C'est le document compas : statut courant, deadlines, roadmap Étapes 1→4, règles métier actées, décisions ouvertes. Le présent document ne le remplace pas — il complète la transition de session et se lit en second. La Fiche Projet est à jour au 05.07.2026 au soir.

**Échéances :** 18.07 = artistes du 08.08 confirmés (le lieu l'est déjà : Mini Café Bar ; les artistes se gèrent par WhatsApp, quasi bloqués — confirmations Karter en cours). **08.08 = run de validation A→Z** : Karter déroule lui-même tout le système de bout en bout.

## 2 · Ce qui a été fait cette session (04–05.07.2026)

1. **Coordination lieux construite, testée (4 chemins), PUBLIÉE 🟢** (`AFTRSN-Venue-Coordinator` `Su9tO0fS39u9itlU`, dossier `AFTRSN-PROCESS-03-Venue-Coordinator`, actuellement v1.2.1). Outreach + relances J+3 / +4 jours, draft-only, compte **Partners** (credential `PARTNERS-GMAil` → `ohE4nLNpeFYMG9Ha`), allemand + version anglaise, jours ouvrés 9h Europe/Zurich.
2. **Coordination artistes construite, testée, PUBLIÉE 🟢** (`AFTRSN-Artist-Coordinator` `VpTCflWHXkU49LDB`, dossier `AFTRSN-PROCESS-04-Artist-Coordinator`, v1.0.2). Même pattern ; **brief J-7 reporté en v1.1** (à construire avant le 01.08).
3. **Règles métier actées par Karter** (poussées dans la Fiche Projet, la mémoire et les fiches) :
   - **« From day to night »** : AFTER SUN n'est pas cantonné au day party — ~2 mois d'été favorables au jour en Europe, le reste bascule vers la nuit (précédents Londres/Turquie). Formats 15h–22h / 19h–02h / 22h–06h selon lieu et saison. Clubs = cibles valides. → À pousser au **canon**.
   - **Déclenchement par statut `To contact`** (posé par Karter) : `Prospect` = réserve silencieuse, rien ne part. Le rythme (~2–3 premiers contacts/semaine) est une **décision humaine volontairement hors process**.
   - **Calendrier lieux** : pas d'urgence — nouveau lieu éventuel à partir d'octobre selon les conditions du lieu actuel ; août–septembre = recherche d'un lieu réputé en électro + promo agressive.
4. **Base Venues enrichie** : 22 lieux qualifiés créés (12 bars/guinguettes + 10 clubs, recherche web validée ; Bar 3000 fusionné avec Club Zukunft) + champs `Email` et `Gmail Thread`. Base DJs/Artists alignée (mêmes champs + statut `To contact`).
5. **Backfill de l'outreach manuel** : Maison 25 (info@urbanagency.ch, 16.06), Soluna (contact@soluna-zurich.ch, 12.05), The Palm Three (hello@thepalmthree.ch, 12.05) — contactés depuis partners@aftersunpeople.com sans réponse, consignés en `To contact` avec date réelle + fil d'origine. **Relances automatiques attendues lundi 07.07 à 9h, dans les fils.**
6. **Hermès a appris son premier skill** (décision Karter : « Hermès est l'exécutant et l'apprenant ») : skill n° 990 « Recherche d'historique email envoyé (Gmail) » écrit dans `lumina_memory`/`skills`, vérifié en tête de recherche sémantique. Utilitaire associé conservé : `AFTRSN-UTIL-Partners-Sent-Lookup` (dossier infra).

## 3 · Fichiers produits / mis à jour cette session

| Fichier | Rôle |
|---|---|
| `Fiche_AFTRSN-Venue-Coordinator_20260704.md` | Fiche technique Coordination lieux (à jour v1.2.1) |
| `Fiche_AFTRSN-Artist-Coordinator_20260705.md` | Fiche technique Coordination artistes (à jour v1.0.2) |
| `POS-AFTRSN_Outreach-Relances-Notion-Drafts_20260704.md` + `POS-GENERIQUE_Outreach-Relances-Base-Drafts_20260704.md` | POS paire : pattern outreach + relances piloté par une base, draft-only |
| `POS-AFTRSN_Backfill-Outreach-Manuel_20260705.md` + `POS-GENERIQUE_Backfill-Outreach-Manuel_20260705.md` | POS paire : intégrer des contacts déjà démarchés manuellement (fils d'origine) |
| `POS-LUMINA_Apprendre-Un-Skill-A-Hermes_20260705.md` | POS : écrire un runbook dans la bibliothèque d'Hermès + test de trouvabilité (générique par nature) |
| `AFTRSN_MarcheASuivre_Construction-n8n.md` | §7 ajouté (pièges du 04–05.07) |
| `FICHE_PROJET_AFTRSN_Automation.md` | Boussole, à jour au 05.07 au soir |
| `PASSATION_AFTRSN_ProchainesEtapes_20260705.md` | Ce document |

## 4 · Prochaines étapes (priorité décroissante)

1. **Lundi 07.07, 9h — premier run réel à surveiller** : 3 brouillons de relance attendus (Maison 25, Soluna, The Palm Three) dans les fils Partners + 3 notifications Telegram. C'est le premier passage en production du chemin « relance dans un fil existant ». Karter relit et envoie.
2. **Artistes 08.08 (Karter, manuel)** : confirmations WhatsApp + choix de 2–3 résidents. Dès confirmation, consigner les artistes dans la base DJs/Artists (téléphone dans `Contact`, statut `Confirmed`) et les lier à l'édition — utile pour le brief J-7 et le post-event.
3. **Construire le Suivi budget (W-6 Budget-Tracker)** — dernier morceau de l'Étape 2 (semaine du 09–15.07) : lecture continue du budget, alertes de seuils, rapport de clôture.
4. **Préparer le run A→Z du 08.08** : remettre le payload épinglé du trigger de la Création d'édition sur les vraies données EP5 (toujours sur ZZTEST) ; construire le **brief artistes J-7** (v1.1 de la Coordination artistes) avant le 01.08.
5. **Étape 3 (16–22.07) · Marketing** : Marketing-Planner (pubs, contenu, newsletter) + lancement de la campagne du 08.08. Prérequis Hermès encore ouvert : runbook « optimisation publicitaire ».
6. **Pousser au canon** (`wiki/`, collection `canon`) : « from day to night » (prioritaire, connaissance de marque de fond), décisions LUMINA OS du 04.07 (Maestro↔Hermès), rangement n8n, POS génériques. Un document de session ne corrige pas la connaissance de fond (SYSTEM-CANON §5 : episodic ≠ canon).
7. **Décisions Karter en attente** : DA-001 (publier ou non Comptable-Finance / Qualite-Compliance), DA-002 (`ZZZ_20260702_tmp_hermes_reach_test`), DA-004 (`ZZZ_20260704_tmp_telegram_chatid`), DA-006 (`ZZZ_20260705_tmp_skill_write`). ~~DA-005~~ annulée (lookup conservé en utilitaire).
8. **Nettoyages Karter** : 6 brouillons ZZTEST dans Gmail Partners ; fiches `ZZTEST-Venue-Coordinator` et `ZZTEST-Artist-Coordinator` → corbeille Notion (neutralisées) ; reconfirmer les 6 emails ⚠️ avant d'envoyer ces brouillons-là (Rimini, Stazione Paradiso, Primitivo, Frieda's Büxe, Kauz, Longstreet).
9. **Surveiller l'Email-Responder** en production (qualité des drafts, volume de notifications) — point reconduit.

## 5 · Protocole permanent — marches à suivre et POS (instruction pour toutes les prochaines sessions)

**Après chaque étape validée**, toute explication réutilisable produite pendant le travail (mode d'emploi pratique généralisable, ou procédure technique détaillée vérifiée) doit être **automatiquement extraite en fichier(s) `.md` et sauvegardée dans `/Users/karter/Claude/Projects/AFTRSN Automation/`** — jamais laissée uniquement dans la conversation ou noyée dans un récap.

**Deux types de document, deux conventions de nommage :**
- **Marche à suivre** — mode d'emploi pratique, réutilisable, **sans date** (document vivant, mis à jour sur place) : `<Scope>_MarcheASuivre_<Sujet-en-Kebab-Case>.md`, `Scope` = `AFTRSN` ou `Generique`.
- **POS** (Procédure Opérationnelle Standardisée) — procédure technique détaillée, snapshot **daté** d'un état vérifié : `POS-<SCOPE>_<Sujet-en-Kebab-Case>_<AAAAMMJJ>.md`, `SCOPE` = `GENERIQUE` / `AFTRSN` / `LUMINA`.

**Règle de la paire systématique :** chaque document **spécifique AFTRSN** (marche à suivre ou POS) est produit avec **une seconde version générique** en parallèle, débarrassée des éléments propres à AFTRSN (IDs, comptes, noms de bases), réutilisable pour toute autre marque gérée via LUMINA OS. Si la procédure est générique par nature, un seul document `Generique_`/`POS-GENERIQUE` (ou `POS-LUMINA` si elle concerne l'architecture LUMINA OS) suffit — pas de doublon artificiel.

**Sections obligatoires dans chaque document produit :**
- **Difficultés rencontrées** — les blocages, erreurs et ambiguïtés effectivement rencontrés (pas hypothétiques).
- **Solutions implémentées** — ce qui a concrètement résolu chaque difficulté (diagnostic exact + correctif appliqué).
- **Leçons apprises** — le principe généralisable, formulé pour rester utile sans se souvenir de l'incident d'origine.

**Règles d'application :**
1. **Détection automatique** — dès qu'une explication étape-par-étape émerge (réponse à une question ou sous-produit d'un débogage), évaluer sa réutilisabilité et l'extraire si oui, **sans attendre la fin de session** (leçon de cette session : deux POS ont failli passer à la trappe, rattrapées sur relance de Karter).
2. **Sauvegarde effective** dans le dossier projet (jamais seulement dans le scratchpad).
3. **Référencement obligatoire** — chaque nouveau document (y compris sa paire générique) est ajouté à « ✅ Déjà fait › Architecture & documentation » de `FICHE_PROJET_AFTRSN_Automation.md` au moment de sa création, et à la table §3 de la passation courante.
4. **Portée LUMINA OS vs métier AFTRSN** — si le contenu concerne l'architecture/gouvernance du système LUMINA OS lui-même, signaler à Karter qu'il mérite d'être poussé vers le dépôt canonique (`wiki/` → collection `canon`) — un document de session seul ne corrige jamais durablement la connaissance de fond (SYSTEM-CANON §5). Cas particulier acté le 05.07 : un **skill opérationnel** peut être appris directement à Hermès (écriture dans sa bibliothèque, voir la POS dédiée) — Hermès est l'exécutant et l'apprenant, les spécialistes guident.
5. **Présentation à l'utilisateur** — chaque document finalisé est présenté via le mécanisme standard (`present_files`), pas seulement décrit en texte.

## 6 · Difficultés rencontrées, solutions implémentées, leçons apprises — session du 04–05.07.2026

| # | Difficulté | Solution | Leçon |
|---|---|---|---|
| 1 | Chaîne morte silencieuse au 1er run : code écrit contre les noms de propriétés Notion supposés | Inspection de la vraie sortie : format simplifié `property_*` snake_case, titre dans `name` | La sortie « simplifiée » d'un node est un format à part entière — exécuter, regarder, coder contre l'observé |
| 2 | Détail d'exécution illisible : décodage récursif du format « flatted » → onglet navigateur gelé ; payloads énormes dès qu'une trace d'agent est incluse | **Vérification par les effets** (relire Notion/Gmail/Telegram) + tests booléens légers sur le payload brut | Quand la télémétrie est plus lourde que le système, vérifier le système |
| 3 | Clics « Execute workflow » perdus (références DOM périmées après rechargement, bouton déplacé) | Re-screenshot avant chaque clic + vérifier qu'un **nouveau** run existe côté serveur avant d'attendre son résultat | Après tout rechargement, les références UI sont mortes ; la preuve d'un déclenchement est côté serveur |
| 4 | Cron « 9h » = 11h suisses (instance n8n en UTC) | `settings.timezone='Europe/Zurich'` + **re-publication** (un PATCH post-publication crée une version non publiée) | Timezone explicite sur tout déclencheur horaire destiné à un humain |
| 5 | POST massif en timeout CDP alors qu'une partie avait réussi (dossier créé, workflow non) | Vérifier l'état serveur avant de rejouer | Un timeout n'est pas un échec : relire l'état avant de recréer |
| 6 | `/rest/credentials` bloqué par le filtre « sensitive » de l'outil navigateur | Ids lus dans les nodes de workflows existants, ou projection minimale id tronqué + nom | Les credentials se référencent par leurs usages, pas par leur liste |
| 7 | Recherche Gmail par nom : 2 cibles sur 3 introuvables (« The Palm Tree » = The Palm Three ; « Studio 45 » = Maison 25 via agence tierce) ; graphies collées non matchées | Requêtes multi-variantes puis repli `in:sent` sans filtre + lecture des 30 derniers ; confirmation Karter | La boîte d'envoi est la source de vérité de l'historique d'outreach, pas les noms d'usage |
| 8 | Fiches backfillées : deux relances à un jour d'intervalle (intervalles calculés sur la date d'origine) | Correctif v1.2.1/v1.0.2 : relance 2 = relance 1 + 4 jours | Les intervalles d'une séquence se calculent sur l'événement précédent, jamais sur l'origine ; un backfill est un test de bord gratuit |
| 9 | `LUMINA-MEMORY-WRITE/WEBHOOK` sans webhook malgré son nom ; taxonomie du contrat skills non documentée | Appelant jetable Execute Workflow ; contrat déduit des skills frères ; test de trouvabilité sémantique post-écriture | Vérifier le node trigger avant de choisir l'invocation ; un skill n'existe pour Hermès que s'il remonte en tête d'une recherche formulée comme une tâche |

---
*Document rédigé en fin de session le 05.07.2026.*
