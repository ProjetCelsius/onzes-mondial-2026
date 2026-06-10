# FEATURES - backlog du hub Coupe du Monde (consigné le 10 juin 2026)

## La vision (formulée par Guillaume)

Pas une collection de tableaux : UNE page de référence, "la page où il faut aller", façon widget Google quand on tape "coupe du monde" - onglets (Matchs / Équipes / Joueurs / Classements / Bracket), bandeau de navigation latéral sticky, tout accessible en un clic, archi laser. Radarfoot a aujourd'hui les classements de poules et un fil d'Ariane, mais pas ce hub unique. C'est l'opportunité.

## Ressources statistiques gratuites (pour probas et match-ups)

| Source | Quoi | Accès |
|---|---|---|
| eloratings.net (World Football Elo Ratings) | Elo de toutes les sélections, historique | Gratuit, tableau scrapable en douceur |
| Classement FIFA officiel | Points par sélection | Gratuit, page publique |
| Cotes des bookmakers (The Odds API) | Probas implicites du marché par match | Tier gratuit 500 req/mois |
| FBref / Wikipédia | H2H historiques, stats de confrontations | Gratuit |

Méthode probas : conversion Elo > probabilité par formule logistique 1/(1+10^(-diff/400)) ajustée nul (modèle classique, transparent, publiable dans la page Méthodo). Croisée avec les cotes du marché quand dispo. Tout en build-time, comme le reste de la base interne.

## Backlog classé par niveau de style (jugement éditorial assumé)

### Tier S - les bangers (différenciants, partageables, on les montre)

1. **Le Hub ultime** : la page unique avec onglets Matchs/Équipes/Joueurs/Classements/Bracket, sidebar sticky, countdown, recherche globale. La colonne vertébrale de tout le reste.
2. **Bracket interactif** : l'arbre complet 16es > finale, visuel, zoomable mobile, qui se remplit en vrai au fil du tournoi. LE truc que tout le monde cherche et que personne ne fait bien en français.
3. **Bracket challenge** : tu remplis TON bracket avant les 16es, lien partageable, comparaison entre potes. Zéro backend nécessaire (état dans l'URL).
4. **Simulateur de tournoi Monte Carlo** : 10 000 simulations sur les Elo, % de titre par équipe, recalculé après chaque match réel. "La France est passée de 12% à 17% après la victoire contre le Sénégal" = contenu quotidien gratuit.
5. **Proba de victoire par match-up** : jauge face à face Elo + FIFA + cotes sur chaque duel et chaque match à venir.
6. **Mode coach + export image** (déjà au CDC) : ta compo, ta note, ton visuel story à partager.
7. **Onze type du Mondial évolutif** : recalculé match après match, avec le terrain maison.
8. **Radars équipe et joueur** (déjà consignés) : forces/faiblesses 6 axes, superposables en duel.

### Tier A - très forts (le coeur du réacteur quotidien)

9. **Match center** : une page par match - compos probables des deux côtés, h2h historique, proba, stade, diffuseur, heure française. La landing parfaite jour de match.
10. **Tracker des meilleurs troisièmes** : la règle la plus confuse du format à 48 visualisée en temps réel. Service public, SEO en or.
11. **Scénarios de qualification auto-générés** : "la France est qualifiée si... / éliminée si...". Calculable, personne ne le fait proprement.
12. **Course au Soulier d'or** : top buteurs/passeurs avec courbes de progression et projection.
13. **Wall des 1248 joueurs filtrable** : par club, championnat, âge. "Le PSG envoie 14 joueurs au Mondial" = articles automatiques.
14. **Calendrier intelligent** : filtre par équipe/groupe/stade, fuseau français mis en avant, ajout calendrier en un tap.
15. **Carte interactive des 16 stades** : où, quels matchs, météo locale, distances (le sujet polémique du Mondial).
16. **H2H historique** : bilan complet des confrontations sur chaque affiche (France-Sénégal 2002, on en parle ?).
17. **Battle joueur vs joueur** : radars superposés jusqu'à 3 joueurs (prototype Lovable existant, à étendre aux 1248).
18. **Indice "groupe de la mort"** : somme des Elo par groupe, classement objectif des groupes les plus relevés.

### Tier B - solides (la profondeur qui fidélise)

19. **Badge forme** : 5 derniers résultats par équipe, partout où l'équipe apparaît.
20. **Timeline d'actus par équipe** : le flux Radarfoot existant filtré par sélection, intégré aux pages équipe.
21. **Records tracker** : Mbappé vs records de Pelé/Klose/Ronaldo, mis à jour en live.
22. **Courbe "la cote des Bleus"** : évolution de la proba de titre de la France jour après jour, embeddable.
23. **Mode second écran** : page live compacte jour de match (score, événements, compos réelles vs probables).
24. **Comparaison compo probable vs compo officielle** : à H-1, score de fiabilité de la rubrique ("on avait 10/11").
25. **Générateur d'affiches de match** : visuel auto partageable par match (drapeaux, proba, heure), brandé Radarfoot.
26. **Carte du monde des 48 qualifiés** : heatmap par continent, anecdotes (premières participations : Curaçao, Jordanie, Cap-Vert, Ouzbékistan...).
27. **PWA installable + notifications** : compos officielles, coups d'envoi, buts (V2, demande un peu d'infra).

### Tier C - cerises (si le temps le permet)

28. **Quiz quotidien Mondial** : 5 questions, score partageable, contenu généré depuis la base interne.
29. **Archive nostalgie** : "il y a 4 ans jour pour jour" (Qatar 2022), comparaisons d'époques.
30. **Easter egg** : konami code sur la page France = le onze de 98 en mode légende sur le terrain.

## Règle de priorisation

Tout converge vers le Hub (1) : chaque feature est un onglet ou un bloc du hub, jamais une page orpheline. Ordre de build conseillé après le MVP compos : 2 (bracket) > 5 (probas) > 9 (match center) > 10 (troisièmes) > 4 (simulateur) > 3 (bracket challenge).
