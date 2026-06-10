# CONTEXT - état du projet au 10 juin 2026, 23h30 (J-1 du Mondial)

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
