# CONTEXT - état du projet au 11 juin 2026, fin de session "équipe 200K" (V6, soirée)

## SESSION DU 11 JUIN (soirée) - audit + master plan + exécution V6 sur radar-footprint

Onze commits poussés (1b36777 → a0c6d7a), page maître rebuiltée à chaque commit, smoke test étendu **86 checks TOUT VERT** (8 onglets).

**Livré :**
- **AUDIT.md** (commité) : 3 volets notés — UX 6,5/10, bugs 7/10, features 5,5/10 — constats fichier:ligne (tri "Gr." mort, "ce soir" en dur, 40 équipes culs-de-sac, zéro match center/chemin/h2h malgré données en base).
- **MASTER-PLAN-V6.md** (commité) : 200K€ / 5 personnes fictives / 4 semaines, 6 workstreams chiffrés (A nav+DS 48K, B qui-contre-qui 52K, C probas 30K, D stats 28K, E QA 24K, F réserve 18K), chaque ligne = livrable vérifiable par smoke.
- **E1+E4 zéro bug** : les 7 bugs de l'audit corrigés (+ offset header mobile mesuré `--v2-hdr-h` au dernier commit).
- **A1-A4 design system** : tokens `:root` (encres/filets/typo/motion), icônes SVG mono-trait (exit emojis), transitions onglet 200ms stagger, compteurs animés (IO), `prefers-reduced-motion`, raccourcis 1-8 + hints kbd, focus-visible, purge styles inline.
- **A5+B1+B3 navigation profonde** : routeur hash `#equipe/:id` / `#match/:id`, drawer latéral (bottom-sheet mobile) — **48/48 fiches équipe** (Elo, rang, percentile, groupe navigable, matchs+probas, XI si documenté, états honnêtes) + **match center 11/11** (jauge, face-à-face, XI côte à côte, diffuseurs). Fin des culs-de-sac. API QA `window.__v2`.
- **A6 cmd+K** : palette sombre (équipes/joueurs/matchs/stades/onglets), accents normalisés, pertinence préfixe>mot>inclusion, aide `?`.
- **B4 calculateur de chemin** : 96 chemins (48 × 1er/2e), adversaire le plus probable par Elo à chaque tour (résolution récursive des slots du bracket officiel), % victoire + cumul, conventions affichées. CTA depuis chaque fiche.
- **C1-C3 Monte Carlo** : `scripts/simulate.mjs` (10 000 tournois, 3es appariés aux slots officiels par backtracking, 0 repli) → `simulations.json` : **France 11,7% de titre, favori Espagne 27,1%**. Colonnes %16es/½/titre triables, strips, méthodo affichée.
- **D1-D3 wall joueurs** : onglet Joueurs (143 joueurs déjà en base), tri 9 colonnes, filtres texte/équipe/poste, stats croisées (top club, doyen/benjamin) ; panneau "Le XI bouge" (diff XI presse vs consensus, chips ±).
- **B2 H2H sourcé** : `h2h.json` (Sénégal 1-0 France ouverture 2002 ; AfSud 1-1 Mexique ouverture 2010, rejouée 16 ans jour pour jour) — 2 fetchs Wikipédia datés, état honnête ailleurs.
- **B5+D4** : hover previews desktop (tooltip riche), filtre équipe sur la timeline, stat-strips sur 100% des vues (Groupes "groupe de la mort", Actus, Équipes).

**Choix tranchés :** chemin de bracket = convention "adversaire le plus probable par Elo, terrain neutre, sans nul" (affichée) ; Monte Carlo = modèle assumé (buts non modélisés, départage aléatoire, méthode affichée + SOURCES.md) ; 3e de groupe dans le chemin = conv. plus fort Elo des groupes possibles ; h2h limité aux 2 affiches phares (POC) ; wall = données existantes uniquement, nulls « — » jamais bouchés.

**Reste à faire (semaine 4 du plan)** : Lighthouse mesuré + consigné, captures démo side-by-side, D5 échantillon effectifs (~70 joueurs clés top-8) si budget, A7 audit axe complet, scores live + classements dynamiques, pipeline quotidien compos (tier 1 #2), régénérer le token GitHub avant le 19/06. Les routes React Lovable (src/routes/mondial.*) n'ont pas été touchées.

---

# CONTEXT - état du projet au 11 juin 2026, fin de session QA+features (jour J du Mondial)

## SESSION DU 11 JUIN (journée) - missions A-F exécutées sur radar-footprint

Six commits poussés sur `radar-footprint` (8913caf → e5fb806), page maître `public/coupe-du-monde-2026.html` rebuiltée à chaque commit, smoke test DOM vert (28 checks).

**Livré :**
- **A. QA zéro bug** : `QA-CHECKLIST.md` + `scripts/qa-smoke.mjs` (harnais linkedom : exécution, 7 onglets jamais vides, zéro null/undefined/NaN visible, liens internes résolus, intégrité données). Bugs corrigés : page blanche sur hash inconnu (le 2e `show()` non validé), teaser/record de capes/dates en dur → tout calculé.
- **B. Probas Elo** : `src/data/mondial/elo.json` 48/48 (eloratings.net via World.tsv + en.teams.tsv, script `scripts/fetch-elo.mjs`, 2 fetchs, daté 11/06). Moteur : We logistique + nul N=0,26×(1−|2We−1|) + bonus hôte +100 (MX/US/CA chez eux), somme garantie 100. Jauges sur les Duels à la une, onglet Stats refondu : tableau 48 triable (Elo/nom/groupe/rang), percentile en barre, stat-strip plateau (médiane 1780, groupe I = plus relevé !), section Méthodo complète. Sources : `src/data/SOURCES.md`.
- **C. Calendrier + timeline** : `scripts/parse-calendrier.mjs` → `calendrier.json` (11 matchs : les 10 de la page extraite + France-Sénégal du hero, stade/diffuseurs null assumés). Timeline interactive en tête d'Aujourd'hui : chips par jour (compteur de matchs, "auj."), heure FR mono, jauges Elo par match, stat-strip du jour (choc Elo, favori le plus net, J-x Bleus), liens match radarfoot + duel. Badge sidebar J-x/JOUR J/LIVE calculé.
- **D. Bracket officiel** : croisements FIFA vérifiés (matchs 73-88, règlement via Wikipédia knockout stage, consulté 11/06) → `bracket.json` + arbre complet 16es→finale+petite finale, ordre des colonnes = adjacence officielle de l'arbre, slots "3e" avec groupes possibles (Annexe C, 495 scénarios — rien d'inventé), chemins du groupe I surlignés (1I→M77, 2I→M78). Repli slots neutres si données absentes.
- **E. Depth chart France** : toggle Terrain/Hiérarchie sur l'onglet Compos. 10 couloirs calculés depuis les 3 XI presse + banc FFF : mentions x/3 en barre, % convention, doubles postes gérés (Mbappé axe/aile gauche), rotation 0/3 en rangée grisée, tout cliquable vers la fiche joueur.
- **F. Radar percentiles** : CONSIGNÉ (pas recalibré) — il manque caps/âges des 48 effectifs ; seuls France (26 joueurs) et Elo (48/48) sont collectés. Le percentile Elo vs plateau est lui déjà livré dans le tableau Stats. Documenté dans `src/data/SOURCES.md` + méthodo affichée. Le radar React de Lovable (src/routes/mondial.*) n'a pas été touché.

**Choix tranchés :** constante de nuls 0,26 = choix de modèle documenté (pas une donnée) ; avantage hôte +100 façon eloratings.net, pays déduit du stade ; % titularisation = convention 3/3→95, 2/3→72, 1/3→45, 0/3→12 affichée partout ; périmètre calendrier = uniquement les matchs de la page extraite (esprit POC, zéro scraping au-delà) ; bracket = slots descriptifs tant que les 3es ne sont pas connus.

**Reste à faire :** scores live + classements dynamiques (zéros aujourd'hui), 40 équipes light, match center par match (16 premiers), Monte Carlo (% titre), radar recalibré quand les 48 effectifs seront collectés, emblèmes en local (/public/emblems/, toujours hotlinkés), régénérer le token GitHub avant le 19/06.

---

**PIVOT MAJEUR (10 juin 23h50)** : le produit n'est plus un outil standalone mais la **proposition de V2 de radarfoot.fr/coupe-du-monde-2026** - sa page, reprise et largement améliorée, transformée en hub avec sidebar latérale. Proposition de valeur : "la ressource ultime pour suivre la Coupe du Monde". Tout est orchestré dans `PLAN-BATAILLE.md` (architecture, mini-CDC par feature, séquencement full autonomie). Le reste de ce document décrit l'état antérieur, toujours valide pour les données et la DA.

Journal de bord cross-sessions. À lire en PREMIER par toute nouvelle session (Claude ou humain).

## Objectif immédiat

Demain matin, Guillaume montre à son pote (créateur de radarfoot.fr) une V1 fonctionnelle et belle : hub des compos probables du Mondial, à brancher presque telle quelle sur son site.

## Ce qui est FAIT

1. `CDC.md` : cahier des charges complet (fonctionnel, données, charte graphique Radarfoot, UX, architecture, méthodos % et note FUT). LE document de référence.
2. `data/equipes.json` : seed des 48 équipes, 12 groupes, slugs, couleurs maillot.
3. Données réelles collectées et sourcées (recherche web du 10 juin) :
   - `data/squads/france.json` : liste officielle des 26 Bleus (numéros, clubs, âges, sélections, buts) + 3 compos probables de médias différents + blessures (Koundé) + débats de poste.
   - `data/lineups-sources/sept-equipes.json` : onze probable + banc + formation + sélectionneur + sources pour Sénégal, Argentine, Brésil, Angleterre, Espagne, Mexique, Afrique du Sud.
4. `PROMPT-LANCEMENT.md` : prompt de reprise pour une session Claude autonome.
5. `PROMPT-LOVABLE.md` : maxi-prompt à coller dans Lovable (voir répartition du travail ci-dessous).
6. Repo Lovable inspecté : `ProjetCelsius/radar-footprint` = prototype TanStack Start + shadcn/ui + Recharts + Supabase, avec une page "Compare les joueurs" (radar 6 axes Vitesse/Tir/Passes/Dribble/Défense/Physique, 12 joueurs en dur dans `src/lib/players.functions.ts`). C'est le repo SYNCHRONISÉ avec Lovable : ce qu'on y pousse, Lovable le voit, et Guillaume visualise en live.

## Décision d'architecture (10 juin, soir)

Le produit final se construit DANS `radar-footprint` (raccordement demandé par Guillaume : Lovable connecté = visualisation immédiate). Les données du Mondial y sont copiées sous `src/data/mondial/`. Le repo `onzes-mondial-2026` reste la référence documentaire (CDC, données maîtres, prompts, journal).

## Répartition du travail

- **Lovable** (pas de limite de tokens) : toute l'UI. Coller `PROMPT-LOVABLE.md` dans Lovable. Écrans : hub par groupes, page équipe avec terrain vertical + maillots SVG + %, fiche joueur overlay, duel, radar équipe. Données déjà dans son repo.
- **Claude (session suivante)** : données des 40 équipes restantes (mode light : onze probable + formation + clubs, méthode = sous-agents par lots de 8, sources type "Dico du Sport tout savoir sur X" + mediapronos effectifs), calcul des % (méthodo CDC 4.4) et notes (CDC 4.5), radars équipe (axes ci-dessous), contrôle qualité du rendu Lovable, intégration finale.

## Nouvelles idées de Guillaume à intégrer (consignées le 10 juin au soir)

1. **Radar par équipe** : forces/faiblesses sur 6 axes, mix objectif/subjectif. Axes proposés : Attaque, Défense, Milieu, Expérience (caps moyens), Profondeur de banc, Star power. Affiché sur la page équipe + superposé dans le duel.
2. **Radar par joueur** : type FIFA (Vitesse/Tir/Passes/Dribble/Défense/Physique), déjà prototypé par Lovable. À terme pour tous les joueurs du Mondial. Source possible : notes éditoriales + stats publiques ; consigner la méthodo.
3. **Analyse tactique par équipe** : 2-3 phrases (style de jeu, pressing, transitions) + forces/faiblesses listées. Certaines métriques objectives, d'autres subjectives assumées (badge "avis de la rédac").
4. **Base de données interne propre** de toutes les équipes et joueurs, bien consignée : c'est `src/data/mondial/` (master dans onzes-mondial-2026/data/), schéma du CDC section 4.3. Toute feature future se nourrit de cette base unique.
5. Visuels maillots : petits maillots SVG qui ressemblent aux vrais (couleurs dans equipes.json, à affiner avec rayures/motifs si simple).
6. **Vision "Hub ultime"** (10 juin, tard) : une seule page de référence façon widget Google CdM (onglets Matchs/Équipes/Joueurs/Classements/Bracket, sidebar sticky). + probas de victoire par match-up (Elo gratuit), bracket visuel interactif. Backlog complet de 30 features classées par style : voir `FEATURES.md`.

## Niveau de qualité attendu (rappel)

- "Graphisme de média social pro" : CDC section 5.5. Référence : visuels compo FFF en DA Radarfoot (sombre, vert #22C55E, mono pour les chiffres).
- Zéro donnée inventée. Tout sourcé, champ inconnu = null géré par l'UI. Les radars/notes subjectifs sont étiquetés comme éditoriaux.
- Mobile-first, cibles 44px, animations sobres 150-250ms.

## TODO MVP (demain matin)

- [ ] Coller PROMPT-LOVABLE.md dans Lovable, itérer sur le rendu (Guillaume juge à l'oeil, son radar qualité fait foi)
- [ ] Session Claude : vérifier le travail Lovable, calculer %/notes/radars dans les JSON, pousser
- [ ] Données : compléter numéros/âges manquants des 7 équipes, puis 40 équipes en mode light (sous-agents)
- [ ] Duel France-Sénégal (16 juin) et Mexique-Afrique du Sud (11 juin, match d'ouverture) parfaits pour la démo
- [ ] Vérif finale : mobile réel, données France croisées avec 2 sources, aucun null visible

## TODO V1.1 (après la démo)

- [ ] Mode coach complet (drag & drop, presets, malus hors poste, partage URL, export image)
- [ ] 48 équipes complètes, page Méthodo, og:images
- [ ] Régénérer le token GitHub (expire le 19 juin, en plein Mondial)

## Protocole de reprise (nouvelle session Claude)

1. Token GitHub : Notion > "Journal des sessions (historique)" > Informations techniques persistantes. JAMAIS commité (repos publics).
2. Lire ce CONTEXT.md, puis CDC.md.
3. Cloner `ProjetCelsius/radar-footprint` (produit) et `ProjetCelsius/onzes-mondial-2026` (référence).
4. Mettre à jour ce fichier en fin de session (journal + décisions + état).
