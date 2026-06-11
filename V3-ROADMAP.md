# V3 ROADMAP - le scan complet (11 juin 2026)

25 grosses features, chacune avec son mini-CDC, rankées de la plus importante à la moins importante.
Critère de ranking : impact lecteur quotidien x différenciation vs concurrence x faisabilité pendant le tournoi.
Doctrine transverse : densité mastermind (DOCTRINE-DENSITE.md), DA Radarfoot stricte, zéro donnée inventée.

---

## Tier 1 - les fondations du règne (à faire en premier)

### 1. Match Center universel
- **Quoi** : une page par match (104 matchs) : compos probables face à face, proba de victoire, h2h historique, stade, météo, diffuseur, heure FR, arbitre, enjeux de qualification.
- **Données** : calendrier complet + compos (base interne) + Elo + h2h Wikipédia.
- **UX** : accessible depuis CHAQUE carte match du site. La landing parfaite : on tape "Mexique Afrique du Sud" sur Google, on doit tomber là.
- **Done quand** : les 104 matchs ont leur page, les 16 premiers sont complets à 100%.

### 2. Les 48 compos complètes + pipeline de mise à jour quotidienne
- **Quoi** : industrialiser ce qui existe pour 8 équipes vers les 48, avec refresh quotidien des compos probables (la donnée pourrit en 24h pendant un Mondial).
- **Données** : sous-agents par lots de 8, sources presse datées, script compute-probabilities.
- **UX** : badge "mis à jour il y a Xh" sur chaque terrain - la fraîcheur est une feature.
- **Done quand** : 48/48 terrains avec %, aucune compo datée de plus de 48h.

### 3. Moteur de probabilités Elo + simulateur Monte Carlo
- **Quoi** : Elo des 48 (eloratings.net) > proba par match (formule logistique) > 10 000 simulations du tournoi entier > % de titre, de finale, de 8es par équipe, recalculés après chaque match réel.
- **UX** : jauges sur les cartes match + page "Probabilités" avec tableau 48 lignes triable + courbe d'évolution par équipe. Méthodo publiée.
- **Done quand** : "la France passe de 12% à 17%" est générable chaque soir en une commande.

### 4. Bracket interactif + Bracket Challenge
- **Quoi** : l'arbre 16es>finale qui se remplit en réel (croisements officiels FIFA vérifiés) + mode prédiction : l'utilisateur remplit SON bracket, état encodé dans l'URL, comparaison entre potes.
- **UX** : SVG zoomable mobile, drapeaux, scores. Le slot Finale doré.
- **Done quand** : remplissable, partageable, et juste (zéro croisement inventé).

### 5. Page équipe V3 ultra-dense
- **Quoi** : la page équipe passe au standard mastermind : terrain + stat-strip XI + radar 6 axes + depth chart (feature 6) + forme (5 derniers matchs) + calendrier + timeline d'actus filtrée + h2h vs adversaires du groupe + note presse sourcée.
- **Done quand** : un fan reste 5 minutes sur la page Sénégal sans avoir tout vu.

### 6. Depth chart par poste (originale)
- **Quoi** : pour chaque équipe, la hiérarchie à CHAQUE poste (titulaire / 2e choix / 3e choix avec %). Personne ne fait ça en français : on visualise la concurrence Koundé-Gusto, pas juste le XI.
- **UX** : vue "couloirs" verticale par poste, jetons empilés avec %, toggle depuis le terrain.
- **Données** : déjà calculables depuis les compos presse multiples + minutes récentes.
- **Done quand** : France complète, top 16 ensuite.

## Tier 2 - les machines à engagement quotidien

### 7. Live "second écran" + probable vs réel
- **Quoi** : à H-1, les compos officielles tombent (API-Football) : affichage côte à côte probable/réel + score de fiabilité de la rubrique ("on avait 10/11"). Pendant le match : événements clés en page compacte.
- **Done quand** : opérationnel pour un match test, score de fiabilité archivé match après match.

### 8. Tracker meilleurs troisièmes + scénarios de qualification
- **Quoi** : la règle la plus confuse du format 48 visualisée en live + génération automatique des scénarios ("l'Irak est qualifié si... éliminé si...") dès la 2e journée.
- **UX** : tableau 12 lignes avec ligne de flottaison 8/12 + phrases en français généré.
- **Done quand** : les scénarios se recalculent à chaque résultat sans intervention.

### 9. Wall des 1248 joueurs + radar joueur généralisé
- **Quoi** : base complète filtrable (club, championnat, âge, poste) + fiche et radar 6 axes pour chacun. "Le PSG envoie 14 joueurs au Mondial" devient une requête à un clic.
- **Done quand** : 100% des 26x48 listés, fiches enrichies au fil de l'eau.

### 10. Calculateur de chemin (originale, banger)
- **Quoi** : "si la France finit 1ère du groupe I, elle croise X en 16es, puis probablement Y en 8es..." - le parcours projeté jusqu'en finale avec probas cumulées à chaque étape, pour les 48 équipes.
- **Données** : bracket officiel + Monte Carlo (feature 3). C'est LE contenu débat de comptoir.
- **Done quand** : page France affiche son chemin le plus probable vers le titre avec les %.

### 11. Historique des compos - le "git diff" du XI (originale)
- **Quoi** : chaque mise à jour de compo est versionnée : "Gusto remplace Koundé dans le XI probable (depuis le 10/06)". Timeline des entrées/sorties par équipe. Très ADN "radar".
- **Données** : les JSON sont déjà versionnés dans git - il suffit de l'exposer.
- **Done quand** : page France montre l'évolution du XI depuis le 14 mai.

### 12. Course au Soulier d'Or + records tracker
- **Quoi** : top buteurs/passeurs live avec projection, et traqueur de records (Mbappé vs Klose 16 buts, vs records de Pelé...) mis à jour à chaque but.
- **Done quand** : la page existe avec les compteurs à zéro et les barres de records pré-tracées.

### 13. Recherche universelle cmd+K (vibe mastermind)
- **Quoi** : un palais de commande : taper "mbap" > fiche, "FRA" > équipe, "16 juin" > matchs du jour, "azteca" > stade. Le geste power-user qui fait passer le site pour un Bloomberg du foot.
- **Done quand** : indexe équipes + joueurs + matchs + stades, < 50ms.

## ARBITRAGE GUILLAUME (11 juin) - à respecter strictement

- Esprit POC : scraping et recherche web MINIMAUX. Match center sur les 16 premiers matchs seulement. Wall joueurs limité à ~100 joueurs. Les features gourmandes en collecte se font "à minima, sur quelques tests".
- Le budget va dans le CODE et la QUALITÉ : zéro bug, tout interactif et cliquable, data viz partout, densité virtuose, pages imbriquées au cordeau.
- Validées : navigation fluide, compos, page équipe dense, probas victoire, bracket interactif, depth chart, tracker troisièmes, groupes & classements (en mieux), calculateur de chemin, historique compos (à minima), match center 16 matchs.
- Le persona : le fan de foot qui se frotte les mains d'anticipation - "qui contre qui, quelle proba, les précédents, les stats" - et qui revient plusieurs fois par jour. PAS de gadgets "FoodX" (quiz, modes TV, nostalgie : coupés).
- Anciens tiers 3-4 REMPLACÉS ci-dessous par de l'amélioration UX/data viz pure.

## Tier 3 - l'expérience fan hardcore (UX et data viz)

### 14. H2H universel
- **Quoi** : sur chaque affiche, l'historique des confrontations (ex : France-Sénégal 2002, seule rencontre en CdM) + forme des 5 derniers matchs des deux équipes en pastilles V/N/D.
- **Done quand** : visible sur les 16 premiers matchs et dans chaque duel.

### 15. Stat-strips contextuels systématiques
- **Quoi** : CHAQUE vue (match, équipe, groupe, duel, bracket) ouvre sur 4-6 métriques mono calculées depuis la base. Jamais un écran qui démarre par du vide.
- **Done quand** : audit des vues : 100% ont leur strip.

### 16. Tableaux triables et filtrables partout
- **Quoi** : classements, 48 équipes, joueurs : tri par Elo, âge moyen, sélections, formation ; filtre instantané. Le réflexe tableur du mastermind.
- **Done quand** : tout tableau de plus de 6 lignes est triable.

### 17. Timeline du jour interactive
- **Quoi** : fil chronologique des matchs du jour (heure FR, probas, liens compos/match center) en tête de l'onglet Aujourd'hui. La première chose vue le matin.
- **Done quand** : le 12 juin au réveil, les 3 matchs du jour y sont, cliquables.

### 18. Hover previews (densité sans clic)
- **Quoi** : survol d'une équipe → micro-terrain + 3 stats en tooltip riche ; survol d'un joueur → mini-fiche. Desktop only, dégradation propre mobile.
- **Done quand** : actif sur le hub 48 équipes et les classements.

### 19. Radars superposables à la demande
- **Quoi** : n'importe quelle paire équipes ou joueurs comparable en 2 clics depuis n'importe quelle page (bouton "comparer" persistant, sélection cross-pages).
- **Done quand** : France vs Sénégal ET Mbappé vs Mané superposables depuis leurs fiches.

### 20. Percentiles visuels
- **Quoi** : chaque stat affichée avec sa barre de percentile vs les 48 ("âge moyen 27.4 : plus jeune que 38% du plateau"). Transforme un chiffre brut en jugement instantané.
- **Done quand** : appliqué aux stat-strips équipe.

## Tier 4 - le polish virtuose

### 21. Navigation imbriquée parfaite + cmd+K
- **Quoi** : breadcrumbs, retours contextuels, deep links sur tout (équipe, joueur, match, onglet), recherche universelle cmd+K (équipes, joueurs, matchs).
- **Done quand** : n'importe quel écran est atteignable en moins de 3 actions et partageable par URL.

### 22. Micro-interactions
- **Quoi** : stagger d'apparition du terrain (gardien puis lignes), transitions d'onglets 200ms, compteurs animés à l'entrée dans le viewport, hover states partout. prefers-reduced-motion respecté.
- **Done quand** : aucun changement d'état sec à l'écran.

### 23. États intelligents (jamais de vide)
- **Quoi** : une équipe sans compo montre quand même effectif partiel, Elo, calendrier, groupe - toujours la donnée la plus proche disponible, jamais un "bientôt" sec.
- **Done quand** : les 40 équipes light ont une page utile.

### 24. Performance et solidité
- **Quoi** : navigation < 100ms, données lazy par onglet, images lazy, Lighthouse mobile >= 90, zéro erreur console.
- **Done quand** : mesuré et consigné.

### 25. QA systématique zéro-bug
- **Quoi** : checklist par page (overflow, mobile, liens morts, null visibles, contrastes), passée avant chaque push. Le bug du banc qui déborde ne doit plus JAMAIS arriver.
- **Done quand** : checklist dans le repo, verte sur toutes les pages.

### 14. Centre stades & logistique
- **Quoi** : carte interactive des 16 stades, matchs par stade, fuseaux, météo (la chaleur est LE sujet), distances entre villes - le pain point logistique de cette CdM à 3 pays.
- **Done quand** : chaque stade a sa fiche reliée aux matchs.

### 15. Power rankings hebdo + indice groupe de la mort
- **Quoi** : classement de force éditorialisé hebdomadaire (mix Elo + forme + avis rédac assumé) + scoring objectif des groupes (somme des Elo). Contenu débat garanti.
- **Done quand** : édition n°1 publiée avant les 8es.

### 16. Générateur d'affiches partageables
- **Quoi** : pour chaque match/compo/stat : une image story 1080x1920 et un post carré générés (canvas), brandés Radarfoot. La machine à reach Instagram/X.
- **Done quand** : 3 templates (match, compo, stat du jour) exportables en un clic.

### 17. Pronos entre potes (ligue privée sans backend)
- **Quoi** : chacun pronostique les scores, état dans l'URL/localStorage, classement de la bande. Zéro compte, zéro serveur.
- **Done quand** : une ligue de 5 potes tient tout le tournoi sans bug.

### 18. La cote des Bleus (widget embeddable)
- **Quoi** : courbe d'évolution de la proba de titre de la France jour par jour, en iframe/embed que la presse peut intégrer (backlinks SEO massifs).
- **Done quand** : l'embed fonctionne sur un site tiers.

### 19. Hall des stats insolites (auto-générées)
- **Quoi** : depuis la base interne : XI le plus jeune, banc le plus capé, équipe la plus "Premier League", doyens et benjamins, fratries... Régénéré chaque jour. Le grisant du mastermind.
- **Done quand** : 20 stats auto-calculées avec leurs requêtes documentées.

### 20. Quiz quotidien Mondial
- **Quoi** : 5 questions/jour générées depuis la base (effectifs, histoire, stats), score partageable façon Wordle.
- **Done quand** : 7 jours de quiz d'avance en file.

## Tier 4 - confort et conquête

### 21. PWA + notifications compos officielles
- **Quoi** : installable, notif à H-1 quand la compo de TON équipe tombe. Rétention pure.
- **Done quand** : notif reçue en conditions réelles sur mobile.

### 22. Mode TV / mode bar (originale)
- **Quoi** : plein écran auto-rotatif (compos du jour > probas > classements > soulier d'or, 10s par écran) pour écran secondaire, bar ou bureau. Personne ne fait ça.
- **Done quand** : tourne 1h sans interaction sans bug.

### 23. Comparateur multi-équipes (4 en grille)
- **Quoi** : extension du duel à 4 équipes (radars superposés, tableaux croisés) - "compare les 4 du groupe I".
- **Done quand** : groupe I comparable en un clic depuis la page groupe.

### 24. Mode nostalgie "il y a 4/8/20 ans"
- **Quoi** : chaque jour, le parallèle avec Qatar 2022, Russie 2018, France 98 (compos d'époque sur le même terrain SVG). Machine à partage générationnelle.
- **Done quand** : France 98 vs France 26 affichable côte à côte.

### 25. API publique légère + embeds presse
- **Quoi** : les JSON de la base interne exposés proprement (CC-BY avec lien) + 2-3 widgets embarquables. Stratégie cheval de Troie : Radarfoot devient la source que les autres citent.
- **Done quand** : un tiers peut afficher le terrain France chez lui avec le crédit.

---

## Ordre de bataille V3 conseillé

Semaine 1 (phase de groupes) : features 1-2-3-4 (les fondations) puis 7-8 (le live).
Semaine 2 : 5-6-9-10-11 (profondeur) + 16 (viralité).
Semaines 3-5 (phase finale) : le reste, porté par l'audience qui monte.

Chaque feature = un mini-CDC ci-dessus + la doctrine densité + la DA du CDC.md. Tout passe par la base interne unique (src/data/mondial/), jamais de données en dur dans les composants.
