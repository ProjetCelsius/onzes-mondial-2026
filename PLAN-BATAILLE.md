# PLAN DE BATAILLE - 11 juin 2026, exécution full autonomie

Document d'orchestration maître. À lire après CONTEXT.md, avant toute action. Tout est conçu pour être exécuté sans intervention de Guillaume jusqu'aux points de contrôle.

## 0. Le pivot stratégique (décidé le 10 juin, 23h50)

On ne construit plus un outil à côté de Radarfoot. On construit **la proposition de V2 de radarfoot.fr/coupe-du-monde-2026** : sa page, son ADN, son HTML, mais largement améliorée. La surprise pour le pote, c'est de reconnaître SA page en mieux - pas de découvrir un autre site.

Proposition de valeur en une phrase : **"La ressource ultime pour suivre la Coupe du Monde"**. Tout ce qu'un fan cherche pendant 5 semaines, sur une seule page hub, dans la DA Radarfoot.

## 1. La page actuelle (inventaire bloc par bloc, relevé le 10 juin)

URL : radarfoot.fr/coupe-du-monde-2026. Structure observée :

1. Hero sombre : trophée or, titre, dates, drapeaux pays hôtes, bouton "Ajouter au calendrier", countdown JOURS/HEURES/MIN (mono), prochain match (Mexique-Afrique du Sud jeu. 11 juin 21:00), fil d'Ariane des phases (Groupes > 16es > 8es > Quarts > Demies > Finale) avec barre de progression, mention "Entrée des Bleus : mar. 16 juin 21:00"
2. Pills de navigation groupes : "Groupe I · les Bleus" épinglé + A-L + "Meilleurs 3èmes" (ancres)
3. Deux colonnes : "À LA UNE DU MONDIAL" (articles avec photo, lien "Tous nos articles") | "PROCHAINS MATCHS" (par jour, drapeaux, heure mono, badges diffuseurs M6/beIN, groupe, stade, lien "Calendrier")
4. "PHASE DE GROUPES" : 12 tableaux de classement (grille 3 colonnes desktop) Pts/J/Dif/G/N/D/BP, liseré vert "Qualifié"
5. Tableau "Meilleurs troisièmes" (12 lignes, 8 qualifiés)
6. Flux d'actus continu (timestamps, badges catégorie, sources presse)

Diagnostic : excellente base, mais c'est une page verticale monolithique. Pas de navigation latérale, pas de pages équipe, pas de compos, pas de bracket, pas de stats. Tout le potentiel est dans la transformation page > hub.

## 2. La V2 : architecture cible

Layout desktop : **sidebar latérale sticky** (gauche, ~220px, fond #141414, style nav Radarfoot) + contenu principal en blocs compacts. Mobile : sidebar devient barre d'onglets horizontale scrollable sticky sous le header.

Sections de la sidebar (= les ancres/routes du hub) :

1. Aujourd'hui (défaut)
2. Calendrier et résultats
3. Bracket
4. Groupes et classements
5. Équipes (les 48)
6. Compos probables
7. Joueurs
8. Stats et probas
9. Actus

Règle d'or : on REPREND les blocs existants (hero countdown, cartes matchs, tableaux groupes, flux actus - le HTML/style précis est à recopier depuis la page live en début de session) et on les recompose en plus compact. La V2 doit être immédiatement reconnaissable comme du Radarfoot.

## 3. Mini-CDC par feature du MVP (demain)

Chaque feature : But / Insertion / Données / Definition of done. Détails compos : CDC.md. Backlog complet : FEATURES.md.

### F1 - Le shell V2 (priorité absolue)
- But : la coquille qui transforme la page en hub. C'est LA surprise visuelle.
- Insertion : route `/coupe-du-monde-2026` dans radar-footprint. Sidebar + header + hero compact (countdown conservé, hauteur réduite ~30%) + système d'onglets.
- Données : equipes.json + calendrier des matchs relevé de la page live (à scraper en début de session : ~16 premiers matchs avec dates/stades/diffuseurs/groupes).
- DoD : navigation fluide entre les 9 sections, mobile parfait, DA indiscernable de radarfoot.fr.

### F2 - Section "Aujourd'hui"
- But : la landing quotidienne. Ce qui se passe AUJOURD'HUI au Mondial en un écran.
- Insertion : section par défaut du hub. Blocs : matchs du jour (cartes compactes style existant + proba de victoire), à la une (3 articles max), bloc compo du jour (lien F4), countdown prochain match des Bleus.
- Données : calendrier + actus de la page live + probas F6.
- DoD : le 11 juin au matin elle affiche Mexique-AfSud 21h et Corée-Tchéquie 04h avec leurs compos probables liées.

### F3 - Calendrier et résultats compact
- But : tous les matchs scannables en un écran, par jour, filtrables par équipe/groupe.
- Insertion : section 2. Reprend les cartes match existantes, en version dense (ligne par match).
- Données : calendrier relevé + à terme API-Football pour les scores.
- DoD : filtre "France" donne les 3 matchs des Bleus avec stades et diffuseurs.

### F4 - Compos probables (le coeur, déjà spécifié)
- But : CDC.md sections F1-F4 (terrain vertical, maillots SVG, %, fiches joueurs, duel).
- Insertion : section 6 + entrée par les pages équipe + bloc dans "Aujourd'hui".
- Données : data/mondial/ (France complète + 7 équipes), 40 restantes en sous-agents.
- DoD : France irréprochable (point de contrôle qualité n°1), 8 équipes navigables, duel France-Sénégal et Mexique-AfSud parfaits.

### F5 - Pages équipe (les 48)
- But : une fiche par sélection : compo, radar 6 axes, effectif, calendrier, forme, "ce qu'en dit la presse".
- Insertion : depuis Équipes (grille 12 groupes), les classements, les cartes match.
- Données : data/mondial/ + radars éditoriaux (valeurs dans PROMPT-LOVABLE.md).
- DoD : les 48 ouvrent une page (light = compo "en cours de relevé" + effectif si dispo), les 8 documentées sont complètes.

### F6 - Probas de victoire (Elo)
- But : sur chaque match à venir, jauge de proba A/nul/B. Crédibilité statistique de l'ensemble.
- Insertion : cartes match (F2, F3), match center (F7), duel (F4).
- Données : Elo de eloratings.net relevés par sous-agent en début de session > data/elo.json. Formule logistique documentée (FEATURES.md). Page Méthodo.
- DoD : les 48 équipes ont un Elo sourcé daté ; chaque match affiche des % qui somment à 100.

### F7 - Match center light
- But : une page par match : compos probables des 2 équipes face à face, proba, stade, heure, diffuseurs, h2h si trouvé.
- Insertion : clic sur toute carte match. Route `/coupe-du-monde-2026/match/<id>`.
- Données : F4 + F6 + calendrier.
- DoD : Mexique-AfSud (match d'ouverture) et France-Sénégal complets.

### F8 - Bracket visuel
- But : l'arbre 16es > finale, le visuel signature du format à 48.
- Insertion : section 3. SVG horizontal scrollable mobile, slots vides "1er groupe A" etc. avant les qualifications, drapeaux quand connus.
- Données : structure officielle des croisements du format 48 (à vérifier en ligne en début de session, c'est LA donnée piège : croisements 16es avec les 8 meilleurs troisièmes).
- DoD : arbre complet et juste, états vides élégants, prêt à se remplir match après match.

### F9 - Tracker des meilleurs troisièmes
- But : visualiser la règle la plus confuse du format. Service public + SEO.
- Insertion : section 4, sous les 12 groupes. Reprend le tableau existant + visu 8/12 qualifiés + explication d'une phrase.
- Données : classements (zéros avant les matchs, structure prête).
- DoD : tableau dynamique branché sur les mêmes données que les groupes.

### F10 - Groupes et classements (reprise)
- But : conserver l'existant qui marche, en version compacte 3 colonnes + mini-cartes cliquables vers les pages équipe.
- DoD : iso-fonctionnel avec la page actuelle, en mieux navigable.

## 4. Séquencement de la journée (full autonomie)

Phase 0 (20 min) : lire CONTEXT/CDC/PLAN. Inspecter ce que Lovable a produit cette nuit (le cas échéant) : si bon, capitaliser ; si moyen, refactorer sans état d'âme. **Le HTML/CSS rendu des deux pages clés est DÉJÀ extrait** (11 juin 00h30, via le navigateur de Guillaume) dans `radar-footprint/extraction/` : `coupe-du-monde-2026/` et `home/`, chacun avec index.html standalone visualisable (scripts retirés, assets absolutisés vers radarfoot.fr) + les 2 CSS du site en local. Police confirmée : **Geist** (chiffres vraisemblablement Geist Mono, à vérifier dans le CSS extrait). C'est la matière première de la V2 : partir de ce HTML, le recomposer en hub. Lancer EN PARALLÈLE les sous-agents données (phase D).

Phase 1 (matin) : F1 shell + F2 Aujourd'hui + F3 calendrier. Point de contrôle : side-by-side page actuelle vs V2, la V2 doit gagner nettement tout en restant "du Radarfoot".

Phase 2 : F4 compos (terrain France = étalon qualité) + F5 pages équipe + fiches joueurs.

Phase 3 : F6 probas + F7 match center + F8 bracket + F9 troisièmes + F10 groupes.

Phase D (en continu, sous-agents par lots de 8) : Elo des 48, calendrier complet, 40 équipes light (méthode et sources : CONTEXT.md). Un commit par lot validé.

Phase 4 (fin) : QA checklist CDC section 9, test mobile, Lighthouse, déploiement, captures d'écran pour la démo, mise à jour CONTEXT.md, résumé pour Guillaume (URL + état des données + choix tranchés).

## 5. Répartition Claude / Lovable

- Claude session du matin : ce plan, de bout en bout. C'est le chemin par défaut, il ne dépend de personne.
- Lovable (optionnel, si Guillaume veut itérer visuellement en journée) : PROMPT-LOVABLE.md reste valable pour les écrans compos. Toute itération Lovable se fait sur le même repo, Claude review au commit suivant.

## 6. Garde-fous

- Zéro donnée inventée (CDC 4.6). Les croisements du bracket se vérifient sur source officielle avant de coder.
- La DA ne se discute pas : tokens CDC section 5, relevés sur le site live en cas de doute.
- Si une feature menace le planning, on coupe dans cet ordre : F7 > F9 > F8 > F6. F1-F2-F4 sont intouchables.
- Token GitHub : Notion, jamais commité. Expire le 19 juin.
