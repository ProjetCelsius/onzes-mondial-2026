# Maxi-prompt Lovable - à coller tel quel dans le projet radar-footprint

---

Tu vas transformer ce projet en "Onzes du Mondial 2026" : le hub des compositions probables des 48 équipes de la Coupe du Monde 2026, pour le média foot français Radarfoot. Les données réelles et sourcées sont DÉJÀ dans le repo sous `src/data/mondial/` (equipes.json = 48 équipes/12 groupes/couleurs maillot ; france.json = les 26 Bleus + 3 compos probables de la presse ; sept-equipes.json = onze probables de 7 équipes). Ne modifie JAMAIS ces données, ne crée AUCUN joueur fictif. Garde la page radar joueurs existante, elle deviendra une sous-page.

## Direction artistique (stricte, c'est la charte du site radarfoot.fr)

- Fond de page #F4F4F5, cartes blanches radius 16px bordure #E5E7EB, header sombre #141414
- Vert signature #22C55E (accents, liens "Voir tout →", badges), or #FACC15 (Coupe du Monde), texte #111827, secondaire #6B7280
- Surfaces sombres en gradient #0E1116 vers #1A2230 (hero, terrain)
- Typo : Inter (titres 600-800) + JetBrains Mono pour TOUS les chiffres (horaires, %, notes, stats)
- Bandeau signature : strip horizontal dégradé bordeaux #7F1D1D > violet #6D28D9 > bleu #1D4ED8 > vert #15803D en haut du hub
- Drapeaux ronds : `https://radarfoot.fr/emblems/nations/<id>.png` (les id sont dans equipes.json)
- Sections : titre small-caps gras précédé d'une barre verticale verte
- Qualité cible : visuel de compo de média social pro (référence : graphiques de composition officiels de la FFF), PAS un dashboard générique. Mobile-first, cibles tactiles 44px, transitions 150-250ms.

## Écrans à construire

### 1. Hub `/mondial`
Bandeau dégradé + titre "Onzes du Mondial". Les 12 groupes (A-L) en grille compacte, le groupe I "les Bleus" épinglé en premier. Carte équipe : drapeau rond + nom + formation si connue (mono) + badge "Compo dispo" vert pour les 8 équipes avec données (france, senegal, argentine, bresil, angleterre, espagne, mexique, afrique-du-sud), sinon état "Bientôt" grisé élégant. Barre sticky : recherche instantanée + bouton "Comparer" (sélection de 2 équipes au tap, puis ouverture du duel).

### 2. Page équipe `/mondial/equipe/:id`
- Header : drapeau, nom, groupe, sélectionneur, et pour la France 3 onglets de compos probables (une par source presse, label = nom du média, formation affichée en mono).
- LE TERRAIN (pièce maîtresse) : vertical, gardien en bas, fond gradient sombre #0E1116>#141A24 avec lignes de terrain SVG blanches à 12% d'opacité (surfaces, rond central). Les 11 joueurs positionnés selon la formation (4-3-3, 4-2-3-1, 5-4-1...). Chaque joueur = petit maillot SVG (corps + manches) aux couleurs kit de equipes.json avec le numéro au centre en mono gras, cartouche nom en dessous (fond #141414/85%, radius 8px, texte blanc 12px 600). Pour la France, pastille % de titularisation sous le cartouche (mono, vert >=90, vert clair 70-89, orange 40-69 ; calcul simple : part des 3 compos presse où le joueur figure : 3/3=95, 2/3=72, 1/3=45).
- Banc : rangée horizontale scrollable sous le terrain (nom + poste + club).
- Bloc "Ce qu'en dit la presse" : le champ notes du JSON + liste des sources (label + lien + date).
- Radar équipe (Recharts, déjà installé) : 6 axes Attaque/Défense/Milieu/Expérience/Banc/Star power, valeurs provisoires éditoriales avec badge "note de la rédac" (France 88/84/85/82/86/90 ; Espagne 92/86/93/80/88/89 ; Argentine 88/83/87/90/82/93 ; Brésil 89/80/84/84/85/88 ; Angleterre 87/85/86/83/87/86 ; Sénégal 80/79/78/76/74/79 ; Mexique 74/75/76/78/70/68 ; Afrique du Sud 66/70/68/62/58/55).
- Équipes sans données : layout complet mais terrain en état vide élégant "Compos en cours de relevé - dispo avant leur 1er match".

### 3. Duel `/mondial/duel/:idA-vs-:idB`
Deux terrains côte à côte (empilés sur mobile avec switcher sticky), radars superposés sur le même graphe (2 couleurs, légende), tableau comparatif : âge moyen du onze, sélections cumulées, répartition par championnat. Mets en avant France vs Sénégal (vrai match du 16 juin) comme exemple par défaut.

### 4. Fiche joueur (overlay)
Au clic sur un jeton ou un joueur du banc : bottom sheet mobile / modal desktop. Avatar initiales aux couleurs du kit, nom complet, numéro, poste, club, âge, sélections, buts (tout ce qui existe dans le JSON ; champ null = ligne omise, jamais de "null" affiché). Pour la France : % avec la phrase "Titulaire dans X des 3 compos presse relevées". Navigation flèches entre joueurs sans fermer.

## Contraintes techniques

- N'utilise PAS Supabase pour ça : tout en import statique des JSON.
- Routes TanStack Router cohérentes avec l'existant. La home actuelle peut rediriger vers /mondial avec un lien vers l'ancienne page radar joueurs.
- Pas de librairie en plus (Recharts est là). Terrain et maillots = SVG fait main, soigné.
- Code TypeScript propre, composants réutilisables (PitchVertical, PlayerToken, PlayerSheet, TeamRadar, GroupGrid).

Commence par la page équipe France (c'est la vitrine), puis le hub, puis le duel France-Sénégal, puis les fiches joueurs.
