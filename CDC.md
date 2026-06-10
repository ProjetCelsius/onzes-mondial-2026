# Cahier des charges - Onzes du Mondial 2026

Hub interactif des compositions probables des 48 équipes de la Coupe du Monde 2026.
Cadeau surprise pour Radarfoot (radarfoot.fr). Document de référence pour une implémentation autonome.

Version 1.0 - rédigé le 10 juin 2026 (J-1 du Mondial). Auteur : Guillaume + Claude.

---

## 1. Pitch

Avant et pendant un Mondial, la question que tout le monde se pose est toujours la même : "ils jouent comment ?". Aucun média français ne propose aujourd'hui un endroit unique où l'on peut :

- voir le onze de départ probable des 48 équipes, sur un vrai terrain, avec un indice de confiance par joueur
- comparer deux équipes côte à côte en deux clics
- cliquer sur n'importe quel joueur et avoir sa fiche en overlay
- faire SA compo ("à ta place, coach") et la partager
- juger la force d'une équipe d'un coup d'oeil grâce à une note type FIFA Ultimate Team

C'est exactement le genre de feature "banger" qui fait revenir un visiteur tous les jours pendant 5 semaines, et qui se partage tout seul sur les réseaux. SEO massif en bonus : "composition probable France Coupe du monde 2026" et ses 47 déclinaisons sont des requêtes à très fort volume pendant tout le tournoi.

Nom de la feature : **Onzes du Mondial** (URL : `/onzes` en standalone, à terme `radarfoot.fr/coupe-du-monde-2026/onzes`).

---

## 2. Utilisateurs et cas d'usage

| Persona | Besoin | Parcours type |
|---|---|---|
| Le fan avant un match | "La France joue comment demain ?" | Hub > clic France > scan du onze en 5 secondes |
| Le comparateur | "France-Sénégal, qui est le plus fort ?" | Hub > mode duel > France vs Sénégal > stats comparées |
| Le sélectionneur du dimanche | "Moi j'aurais mis Olise en pointe" | Page France > Mode coach > drag & drop > partage de SA compo |
| Le curieux | "C'est qui ce joueur ouzbek ?" | Page Ouzbékistan > clic joueur > fiche overlay |
| Le parieur / fantasy | "Quel % de chances que X soit titulaire ?" | Page équipe > lecture des % par joueur |

Usage à 70%+ mobile (trafic foot typique). Le mobile-first n'est pas négociable.

---

## 3. Périmètre fonctionnel

### F1 - Le Hub (écran d'accueil)

L'écran d'entrée. Objectif : scanner les 48 équipes en moins de 10 secondes, compact et dense, sans scroll infini.

- Grille des 12 groupes (A à L), 4 équipes par groupe. Une carte équipe = drapeau rond + nom + note Radar (badge) + formation (ex : 4-2-3-1).
- Le groupe I ("les Bleus") est épinglé en premier, comme sur le site actuel.
- Barre d'outils sticky en haut :
  - recherche instantanée (équipe ou joueur, fuzzy)
  - tri : par groupe (défaut) / par note Radar décroissante
  - bouton "Comparer deux équipes" (active le mode sélection : on tape 2 cartes, ça ouvre le duel)
- Au hover/tap long d'une carte : aperçu micro-terrain (les 11 points de la formation, sans noms) pour donner envie de cliquer.
- Bandeau header : reprend le bandeau dégradé multicolore des groupes du site (voir charte, section 5.1).

### F2 - Page équipe (le coeur du produit)

Référence esthétique : les visuels de composition officiels de la FFF (terrain vertical bleu nuit, cartouches de noms, photos/maillots) croisés avec la DA Radarfoot. Le rendu doit être digne d'un graphisme de communauté de média social, pas d'un plugin générique.

Contenu :

- Header équipe : drapeau, nom, groupe, 3 prochains matchs, sélectionneur, note Radar globale + sous-notes ATT / MIL / DEF.
- Le terrain (vertical, gardien en bas comme les visuels FFF) :
  - fond sombre Radarfoot (gradient #0E1116 vers #141A24), lignes du terrain en vert désaturé subtil, surfaces et rond central tracés
  - 11 jetons joueurs positionnés selon la formation
  - un jeton = maillot SVG aux couleurs de la nation + numéro + cartouche nom en dessous + pastille % de titularisation
  - code couleur du % : vert >= 90 (Certain), vert clair 70-89 (Probable), orange 40-69 (En balance), gris < 40 (Outsider)
- Sélecteur de formation : si la presse hésite entre deux systèmes (ex : 4-2-3-1 vs 4-3-3), des onglets permettent de basculer entre les onze probables alternatifs.
- Le banc : sous le terrain, rangée horizontale scrollable des 15 autres joueurs de la liste des 26, avec leur % et leur poste.
- Bloc "Ce qu'en dit la presse" : 2-3 phrases éditoriales sur les incertitudes (blessures, concurrence à un poste), avec les sources.
- CTA : "Comparer avec..." et "Faire ma compo" (F5).

### F3 - Fiche joueur (overlay)

Au clic sur n'importe quel jeton (terrain ou banc) :

- Mobile : bottom sheet qui glisse depuis le bas. Desktop : modal centrée. Fermeture : tap hors zone, croix, swipe down, Échap.
- Contenu : photo (réutiliser `radarfoot.fr/photos/joueurs/<slug>/...` si dispo, sinon avatar initiales aux couleurs du pays), nom, numéro, poste, club + ligue, âge, taille, pied fort, sélections / buts en sélection, stats saison 2025-26 (matchs, buts, passes dé), % de titularisation avec une ligne d'explication ("Présent dans 9 des 10 compos probables publiées"), note Radar individuelle.
- Lien "Voir sur Radarfoot" vers `radarfoot.fr/joueurs/<slug>` quand la page existe.
- Navigation précédent/suivant entre joueurs du onze sans fermer l'overlay.

### F4 - Le comparateur (mode duel)

- Accès : bouton du hub, CTA de page équipe, ou URL directe `/duel/france-vs-senegal`.
- Desktop : deux terrains côte à côte. Mobile : terrains empilés avec switcher sticky, ou demi-terrains face à face (à trancher au build selon le rendu, le face à face est préféré s'il reste lisible).
- Sous les terrains, le tableau de confrontation :
  - note Radar globale + ATT / MIL / DEF (barres opposées, le meilleur en vert)
  - âge moyen du onze, sélections cumulées, buts en sélection du onze
  - répartition par ligue (ex : 7 joueurs de Premier League vs 3)
  - joueur le plus cher / le plus capé de chaque côté
- Easter egg utile : si les deux équipes se rencontrent vraiment au Mondial, afficher la date et le stade du match.

### F5 - Mode coach ("Fais ta compo")

Le différenciateur. Sur chaque page équipe, bouton "Faire ma compo" :

- Bascule le terrain en mode édition : drag & drop des jetons entre postes, swap terrain/banc en tapant un joueur du banc puis sa cible.
- Changement de formation par presets : 4-3-3, 4-2-3-1, 4-4-2, 3-5-2, 3-4-3, 5-3-2 (les jetons se réorganisent avec animation).
- La note Radar de l'équipe se recalcule en direct : mettre un joueur hors poste applique un malus visible (ex : Olise en pointe = pénalité "hors poste" affichée). C'est la boucle de jeu : je modifie, le score bouge, je compare au onze de la rédac.
- Partage : l'état complet de la compo est encodé dans l'URL (querystring compressée, zéro backend). Bouton "Copier le lien" + export image PNG du terrain (canvas) au format story et au format carré, avec le branding Radarfoot, pour poster direct sur les réseaux.
- Reset en un tap vers le onze probable de référence.

### F6 - Note Radar (scoring type FUT)

Chaque joueur a une note 0-99, chaque équipe une note globale + 3 sous-notes. Méthodologie en section 4.5. Affichage :

- badge hexagonal ou pastille (style FUT mais aux couleurs Radarfoot : fond sombre, chiffre vert)
- paliers visuels : 85+ (élite, vert vif), 75-84 (très bon, vert), 65-74 (solide, neutre), < 65 (limité, gris)
- la note est éditoriale et assumée comme telle : une page "Méthodo" accessible depuis le footer explique le calcul en toute transparence. C'est un atout engagement (les gens vont débattre des notes), pas un problème.

### F7 - Partage et deep links

- Routes propres et partageables : `/` (hub), `/equipe/<slug>`, `/duel/<slugA>-vs-<slugB>`, `/compo/<slug>?c=<état encodé>`.
- Meta OG par page (titre, description). V2 : og:image générée par équipe.
- Boutons partage natifs (Web Share API mobile, copie de lien desktop).

### Hors périmètre V1 (assumé)

- Pas de backend, pas de compte utilisateur, pas de commentaires.
- Pas de mise à jour temps réel des compos officielles à H-1 des matchs (V2).
- Pas de stats avancées type xG (V2 si une source fiable est branchée).
- Pas de multilingue : français uniquement.

---

## 4. Données

### 4.1 Où trouver les données, gratuitement

Réponse à la question "base de données joueurs : où, gratuit, faut-il scraper ?". État des lieux honnête :

| Source | Quoi | Gratuit ? | Fiabilité | Verdict |
|---|---|---|---|---|
| Wikipédia (pages "Liste des joueurs CdM 2026" + pages équipes nationales) | Listes officielles des 26, numéros, clubs, sélections, buts, âges | Oui, totalement (licence CC) | Excellente pendant un Mondial (maintenu à la minute) | **Source primaire pour les effectifs** |
| API-Football (api-sports.io) | Effectifs, stats joueurs, compos officielles des matchs | Tier gratuit 100 requêtes/jour | Bonne | **Source d'enrichissement** (stats saison, photos). 100 req/jour suffisent pour un pipeline build-time étalé |
| football-data.org | Matchs, calendrier, effectifs basiques | Tier gratuit (10 req/min, compétitions limitées) | Bonne | Backup calendrier |
| FotMob (API non officielle) | Stats riches, notes, compos probables | Gratuit mais non documenté/non garanti | Bonne mais fragile | Enrichissement opportuniste, jamais en dépendance critique |
| Transfermarkt | Valeurs marchandes, profils | Pas d'API, scraping toléré à petite dose | Excellente | Optionnel V1 (valeur marchande = nice to have) |
| FBref / Sports Reference | Stats détaillées | Scraping autorisé avec politesse (rate limit) | Excellente | V2 pour stats avancées |
| Presse FR (L'Équipe, RMC, Foot Mercato, Dico du Sport...) | **Compos probables** | Lecture libre | C'est LA matière première du % | **Source primaire pour les onze probables**, relevé manuel/assisté |

**Architecture retenue : pipeline build-time, site 100% statique.**

On ne branche AUCUNE API au runtime. Des scripts Node (`scripts/collect/*.mjs`) tournent en amont, produisent des JSON versionnés dans `/public/data/`, et le site ne consomme que ces JSON. Avantages : zéro quota, zéro clé exposée, zéro dépendance ToS au runtime, perf maximale, et le repo raconte l'historique des compos (git diff des JSON = changelog gratuit).

Mise à jour : on relance le pipeline quand on veut (quotidien pendant le Mondial). Une GitHub Action `update-data.yml` déclenchable à la main (workflow_dispatch) peut automatiser le refresh + redeploy.

### 4.2 Pipeline de collecte

1. `collect-squads.mjs` : récupère les listes des 26 par équipe (Wikipédia comme source structurée, croisée avec API-Football). Sortie : `data/squads/<slug>.json`.
2. `collect-stats.mjs` : enrichit chaque joueur (club, âge, taille, pied, sélections, buts, stats saison) via API-Football, en respectant le quota (batché sur plusieurs runs si besoin).
3. Compos probables : relevé éditorial assisté. Pour chaque équipe, 3 à 6 compos probables publiées par la presse sont saisies dans `data/lineups-sources/<slug>.json` (source, date, URL, onze). Un script `compute-probabilities.mjs` agrège et calcule les %.
4. `compute-ratings.mjs` : calcule les notes Radar (méthodo 4.5).
5. `build-teams.mjs` : assemble le tout en un `public/data/teams/<slug>.json` par équipe + un `public/data/index.json` léger pour le hub.

Règle d'or : **zéro donnée inventée**. Chaque effectif et chaque compo probable doit être traçable vers une source réelle datée, consignée dans `data/SOURCES.md`. Si une stat est introuvable, le champ est `null` et l'UI l'omet proprement. Mieux vaut une fiche incomplète qu'une fiche fausse.

### 4.3 Modèle de données (TypeScript)

```ts
type Position = "GK" | "RB" | "RCB" | "CB" | "LCB" | "LB" | "RWB" | "LWB"
  | "CDM" | "RCM" | "CM" | "LCM" | "CAM" | "RW" | "LW" | "RS" | "ST" | "LS";

type Foot = "droit" | "gauche" | "ambidextre";

interface Player {
  id: string;                 // slug, ex "kylian-mbappe"
  name: string;               // affichage court, ex "Mbappé"
  fullName: string;
  number: number | null;      // numéro de maillot officiel
  position: Position;         // poste principal
  altPositions: Position[];
  club: string;
  league: string;
  age: number;
  heightCm: number | null;
  foot: Foot | null;
  caps: number;
  intGoals: number;
  seasonStats: { apps: number; goals: number; assists: number } | null;
  marketValueMEur: number | null;   // optionnel V1
  photoUrl: string | null;          // pattern radarfoot ou null -> initiales
  radarfootUrl: string | null;      // /joueurs/<slug> si la page existe
  rating: number;                   // note Radar 0-99 (calculée)
  startProbability: number;         // 0-100 (calculée)
  probabilityNote: string | null;   // ex "Titulaire dans 5 des 6 compos presse"
}

interface Lineup {
  formation: string;          // "4-2-3-1"
  isPrimary: boolean;
  players: { playerId: string; slot: Position }[];  // exactement 11, GK inclus
}

interface Team {
  id: string;                 // slug, ex "france"
  name: string;
  group: string;              // "A".."L"
  emblemUrl: string;          // /emblems/nations/<slug>.png (copié en local)
  kit: { primary: string; secondary: string; text: string };  // hex maillot
  coach: string;
  fifaRank: number | null;
  lineups: Lineup[];          // 1 à 3, la première = référence
  squad: Player[];            // les 26
  pressNote: string | null;   // 2-3 phrases incertitudes/blessures
  sources: { label: string; url: string; date: string }[];
  ratings: { overall: number; att: number; mid: number; def: number };
  matches: { opponentId: string; date: string; stadium: string }[];
}
```

L'état du mode coach encodé en URL : `{ formation, players: playerId[11] }` sérialisé JSON > compressé (lz-string) > base64url, dans `?c=`.

### 4.4 Méthodologie du % de titularisation

Pour chaque joueur, deux composantes :

1. **Consensus presse (70%)** : part des compos probables relevées (3 à 6 par équipe, sources datées de moins de 10 jours) où le joueur est titulaire.
2. **Temps de jeu récent en sélection (30%)** : part des minutes possibles sur les 5 derniers matchs de la sélection (qualifs + amicaux de préparation).

`startProbability = round(0.7 * consensus + 0.3 * tempsDeJeu)`, plafonné à 95 (rien n'est jamais sûr) sauf consensus unanime ET 5 titularisations sur 5 = 98. Blessé déclaré forfait = 0 avec mention. Le détail est affiché dans la fiche joueur (transparence = crédibilité).

### 4.5 Méthodologie de la note Radar (type FUT)

Note joueur 0-99, calculée puis arrondie :

- base "niveau de club" (0-40) : barème par club/ligue (élite européenne 34-40, top 5 ligues 26-34, autres ligues majeures 18-26, reste 10-18)
- expérience internationale (0-25) : log des sélections, bonus buts pour les offensifs
- forme saison 2025-26 (0-25) : minutes jouées + contributions (buts+passes pondérés par poste)
- courbe d'âge (0-9) : pic 24-29 ans, décote progressive avant/après

Note équipe : moyenne pondérée du onze de référence (gardien x1, défense x1 par joueur, milieu x1.1, attaque x1.15) + bonus banc (profondeur des 26). Sous-notes ATT / MIL / DEF = moyennes par ligne. Mode coach : un joueur aligné hors de `position` et `altPositions` prend un malus de 12 points dans le calcul de l'équipe (affiché "hors poste" sur le jeton).

Les barèmes vivent dans `data/rating-config.json` : ajustables sans toucher au code, et publiés sur la page Méthodo.

### 4.6 Les 48 équipes (seed)

Le fichier `data/equipes.json` du repo contient les 12 groupes avec slugs, noms, URLs d'emblèmes et couleurs de maillot par défaut. Rappel de la composition des groupes (relevée sur radarfoot.fr le 10 juin 2026) :

- A : Mexique, Afrique du Sud, Corée du Sud, Tchéquie
- B : Canada, Bosnie-Herzégovine, Qatar, Suisse
- C : Brésil, Maroc, Haïti, Écosse
- D : États-Unis, Paraguay, Australie, Turquie
- E : Allemagne, Curaçao, Côte d'Ivoire, Équateur
- F : Pays-Bas, Japon, Suède, Tunisie
- G : Belgique, Égypte, Iran, Nouvelle-Zélande
- H : Espagne, Cap-Vert, Arabie saoudite, Uruguay
- I : France, Sénégal, Irak, Norvège
- J : Argentine, Algérie, Autriche, Jordanie
- K : Portugal, RD Congo, Ouzbékistan, Colombie
- L : Angleterre, Croatie, Ghana, Panama

Top 2 de chaque groupe + 8 meilleurs troisièmes qualifiés pour les 16es de finale.

### 4.7 Plan de production des données (réaliste)

48 équipes x 26 joueurs = 1248 fiches. On phase :

- **P0 (preuve)** : France complète à 100% (26 joueurs, stats, 2 formations, %, sources). Sert d'étalon qualité.
- **P1 (têtes d'affiche)** : 15 équipes complètes - Argentine, Brésil, Angleterre, Espagne, Portugal, Allemagne, Pays-Bas, Belgique, Croatie, Maroc, Uruguay, Colombie, Sénégal, Norvège, Mexique.
- **P2 (couverture totale)** : les 32 restantes en "mode light" : onze probable + formation + % + club/âge/sélections par joueur. Stats saison et fiches enrichies au fil de l'eau.

Le hub affiche tout le monde dès P2. Une équipe "light" reste pleinement fonctionnelle (terrain, duel, coach), seules les fiches joueurs sont moins denses.

---

## 5. Charte graphique

Relevée sur radarfoot.fr (accueil + hub Coupe du Monde) le 10 juin 2026. Tokens estimés sur captures : à recaler pixel-perfect au build en inspectant le CSS du site live (fichier `_next/static/css/*.css`), mais les valeurs ci-dessous sont fidèles.

### 5.1 Couleurs

| Token | Valeur | Usage |
|---|---|---|
| `--bg` | #F4F4F5 | fond de page (gris très clair) |
| `--surface` | #FFFFFF | cartes, tableaux |
| `--surface-dark` | #141414 | header/nav |
| `--hero-from` / `--hero-to` | #0E1116 / #1A2230 | gradients des cartes sombres (hero, carte CdM, terrain) |
| `--green` | #22C55E | couleur signature ("Foot" du logo, liens "Voir tout", badges live, accents) |
| `--green-dark` | #15803D | hover, segment vert du bandeau groupes |
| `--gold` | #FACC15 | trophée, "J-1", accents Coupe du Monde |
| `--amber` | #D97706 | badge MERCATO |
| `--text` | #111827 | texte principal |
| `--text-muted` | #6B7280 | texte secondaire, timestamps |
| `--border` | #E5E7EB | bordures cartes |
| Bandeau groupes | #7F1D1D > #6D28D9 > #1D4ED8 > #15803D | strip horizontal multicolore segmenté par groupe (signature visuelle du hub CdM) |

### 5.2 Typographie

- Titres et UI : sans-serif grotesque moderne (rendu type Inter / Geist), graisses 600-800 pour les titres, interlettrage légèrement serré sur les gros titres.
- Horaires, comptes à rebours, données chiffrées : monospace (rendu type JetBrains Mono / Geist Mono) - c'est une signature forte du site (ex "22h 50m 42s", "jeu. 11 juin · 21:00"), à reprendre pour les %, notes et stats.
- Au build : vérifier les `font-family` exactes dans le CSS du site et utiliser les mêmes (Google Fonts ou fallback proche).

### 5.3 Composants et conventions observés

- Cartes : blanches, radius ~16px, bordure 1px #E5E7EB, ombre très légère ; les cartes match ont un liseré sombre en haut.
- Pills/badges : arrondis full ; badge catégorie vert avec point ("À LA UNE"), badge MERCATO crème/ambre ; tags équipes avec logo + sigle (PSG, OM...).
- Liens d'action : verts, avec flèche "→" ("Voir tout →", "Calendrier →").
- Sections : titre en small caps gras précédé d'une barre verticale verte ("LES MATCHS", "ARTICLES RADARFOOT").
- Countdown : chips sombres avec chiffres mono (JOURS / HEURES / MIN).
- Drapeaux : ronds, via `/emblems/nations/<slug>.png` (copier les 48 en local dans `/public/emblems/`, ne pas hotlinker).
- Diffuseurs : `/broadcasters/m6.svg`, `/broadcasters/bein-sports.svg`.
- Ton éditorial : français direct, factuel, un peu joueur. Pas d'anglicismes inutiles.

### 5.4 DA spécifique au terrain (la pièce maîtresse)

Inspiration : visuels de compo FFF (terrain vertical bleu nuit, cartouches rouges) transposés en Radarfoot :

- terrain vertical en gradient sombre (#0E1116 > #141A24), lignes blanches à 12-15% d'opacité, légère vignette
- jetons : maillot SVG (corps + manches, 2 couleurs du kit national), numéro en gros au centre du maillot, ~56px de large desktop / ~44px mobile
- cartouche nom sous le maillot : fond #141414 à 85%, radius 8px, texte blanc 600, taille 12-13px
- pastille % : petit badge mono collé au cartouche, couleur selon palier (section F2)
- pastille note Radar : hexagone ou cercle en haut à droite du maillot, chiffre mono gras
- animations : apparition du onze en stagger (gardien puis lignes), 250ms ease-out par ligne ; drag & drop avec spring léger en mode coach
- export image : même rendu, en 1080x1920 (story) et 1080x1350 (post), avec header drapeau + nom équipe + footer "radarfoot.fr - Onzes du Mondial"

### 5.5 Esthétique générale

Le niveau d'exigence est "graphisme de média social pro", pas "outil de gestion". Concrètement : espacements généreux, hiérarchie typo stricte, jamais plus de 2 familles de polices, micro-interactions partout (hover states, transitions 150-250ms), aucun élément par défaut du navigateur visible. Si un écran semble "correct mais pas remarquable", il n'est pas fini.

---

## 6. UX / UI

### 6.1 Wireframes (desktop)

Hub :

```
+----------------------------------------------------------------------+
| [bandeau dégradé multicolore : J-1 · Groupes A..L cliquables]        |
| ONZES DU MONDIAL          [recherche] [tri: groupe|note] [Comparer]  |
+----------------------------------------------------------------------+
| GROUPE I · LES BLEUS                                                  |
| [France 86|4-2-3-1] [Sénégal 78|4-3-3] [Irak 65|5-4-1] [Norvège 79]  |
| GROUPE A                                                              |
| [Mexique 77] [Afr. du Sud 70] [Corée du Sud 75] [Tchéquie 74]        |
| ...                                                                   |
+----------------------------------------------------------------------+
```

Page équipe :

```
+----------------------------------------------------------------------+
| [drapeau] FRANCE   Groupe I   Note 86 (ATT 90 MIL 85 DEF 84)         |
| Sélectionneur : D. Deschamps   Prochain : Sénégal, mar. 16/06 21:00  |
| [onglets formation: 4-2-3-1 (réf) | 4-3-3]    [Faire ma compo]       |
|                                                                       |
|                       [ TERRAIN VERTICAL ]                            |
|                  attaquants en haut, gardien en bas                   |
|                                                                       |
| BANC : [jeton][jeton][jeton]... (scroll horizontal, % sur chacun)    |
| CE QU'EN DIT LA PRESSE : ...                       [Comparer avec v] |
+----------------------------------------------------------------------+
```

Duel : deux colonnes terrain + tableau de stats en barres opposées dessous.

### 6.2 Interactions clés

- Tout est cliquable au doigt : cibles >= 44px.
- Hub > équipe : transition fluide (le drapeau de la carte "vole" vers le header si faisable simplement, sinon fade rapide).
- Overlay joueur : navigation gauche/droite entre joueurs, swipe down pour fermer.
- Mode coach : tap joueur banc > les slots compatibles pulsent > tap slot = swap. Drag & drop en plus sur desktop.
- Recherche joueur depuis le hub : la sélection ouvre la page équipe avec l'overlay du joueur déjà ouvert.

### 6.3 Responsive

- Breakpoints : < 640 (mobile, 1 colonne, terrain pleine largeur), 640-1024 (2 colonnes hub), > 1024 (4 cartes par rangée de groupe, duel côte à côte).
- Le terrain reste vertical à toutes les tailles (ratio ~0.68, hauteur max 78vh desktop).
- Duel mobile : deux demi-terrains face à face si lisible, sinon empilés avec switcher sticky.

### 6.4 États

- Loading : skeletons aux formes des cartes/terrain (pas de spinner global).
- Équipe "light" (P2) : fiches joueurs réduites avec mention "Fiche enrichie bientôt", jamais de champs vides bruts.
- 404 équipe : renvoi hub avec message sympa.
- Données indisponibles : fallback gracieux, jamais de NaN/undefined visible.

### 6.5 Accessibilité

- Navigation clavier complète (tab sur les jetons, Entrée = fiche, Échap = fermer).
- Contrastes AA sur tous les textes (attention aux % orange sur sombre).
- `prefers-reduced-motion` respecté (désactive stagger et springs).
- Alt text sur drapeaux et photos, aria-label sur les jetons ("Mbappé, attaquant gauche, titulaire à 95%").

---

## 7. Architecture technique

### 7.1 Stack

- **Vite + React 18 + TypeScript + Tailwind CSS** (rapidité de build, portage facile vers le Next.js de Radarfoot ensuite).
- Animations : Framer Motion (stagger, springs, layout animations du mode coach).
- État : React state + URL (nuqs ou parsing manuel). Pas de store global lourd.
- Export image : html-to-image ou canvas manuel (le rendu SVG du terrain facilite le canvas).
- Compression état compo : lz-string.
- Zéro backend, zéro base de données runtime : JSON statiques.

### 7.2 Arborescence cible

```
/
  CDC.md, PROMPT-LANCEMENT.md, README.md
  data/                      # données sources + scripts (hors bundle)
    equipes.json             # seed 48 équipes (fourni)
    squads/, lineups-sources/, rating-config.json, SOURCES.md
  scripts/collect/*.mjs      # pipeline (section 4.2)
  public/
    data/index.json          # hub léger (48 entrées)
    data/teams/<slug>.json   # 1 fichier par équipe (lazy load)
    emblems/*.png            # 48 drapeaux copiés en local
  src/
    components/ (Pitch, PlayerToken, PlayerSheet, TeamCard, DuelView,
                 CoachMode, RatingBadge, GroupGrid, SearchBar, ...)
    pages/ (Hub, Team, Duel, Methodo)
    lib/ (ratings.ts, probabilities.ts, share.ts, formations.ts)
  .github/workflows/deploy.yml      # build + GitHub Pages
  .github/workflows/update-data.yml # refresh données (manuel)
```

### 7.3 Routing et SEO

- React Router en mode hash si GitHub Pages pose problème, sinon history + fallback 404.html (pattern SPA GitHub Pages standard).
- Title/meta dynamiques par route. Pré-rendu statique des 48 pages équipe (vite-plugin-ssg ou script de prerender) si le temps le permet : sinon V2.
- Performances : index.json < 30KB, team JSON < 60KB chacun, lazy loading par route, images lazy, score Lighthouse mobile cible >= 90.

### 7.4 Déploiement

- GitHub Pages via Actions (build Vite > artifact > Pages). Le repo est public pour Pages gratuit.
- URL de démo immédiate : `<user>.github.io/onzes-mondial-2026`. Portage ultérieur chez Radarfoot = copier composants + JSON dans son repo Next.js.

---

## 8. Plan d'implémentation (pour la session de demain)

1. **Setup (30 min)** : scaffold Vite/TS/Tailwind, tokens DA, polices, copie des 48 emblèmes, déploiement Pages qui marche dès le premier commit (pipeline avant features).
2. **Design system (1h)** : Pitch SVG + PlayerToken + RatingBadge + TeamCard + overlay. Validation esthétique sur la France AVANT d'industrialiser. C'est le point de contrôle qualité numéro 1.
3. **Données P0 (1h, en parallèle via sous-agents)** : recherche web des listes et compos probables France + 3 équipes contrastées (Argentine, Irak, Mexique) pour éprouver le modèle sur des cas riches ET pauvres en données.
4. **Hub + page équipe + fiche joueur (2h)** : navigation complète sur les équipes P0.
5. **Duel (1h)** puis **Mode coach (1h30)** : drag & drop, presets formations, recalcul note, partage URL + export image.
6. **Données P1 puis P2 (en tâche de fond, sous-agents parallèles)** : industrialiser la collecte, 1 commit par équipe validée.
7. **Vérification finale** : checklist section 9, test mobile réel, Lighthouse, relecture des données France par échantillonnage croisé avec 2 sources.

### 9. Definition of done

- [ ] Les 48 équipes visibles sur le hub, chacune avec terrain fonctionnel (au minimum mode light)
- [ ] France + 15 têtes d'affiche complètes (26 joueurs, stats, %, sources, 2 formations si débat presse)
- [ ] Duel, mode coach, partage URL, export image : fonctionnels mobile + desktop
- [ ] Aucune donnée inventée ; data/SOURCES.md complet ; champ inconnu = null géré par l'UI
- [ ] DA fidèle Radarfoot (tokens section 5), niveau "je le montre à un directeur artistique sans trembler"
- [ ] Lighthouse mobile >= 90 perf / >= 95 a11y ; site déployé sur GitHub Pages
- [ ] Page Méthodo en ligne (calcul des % et des notes)

---

## 10. Pistes V2 (pendant le Mondial)

- Compos officielles à H-1 (API-Football les fournit) avec comparaison "probable vs réel" et score de fiabilité de la rubrique
- og:image générées par équipe et par compo partagée
- "Onze type du Mondial" évolutif au fil des matchs
- Votes des lecteurs sur les débats de poste (Olise vs Doué...) - nécessite un micro-backend (Cloudflare Workers + KV)
- Intégration native dans radarfoot.fr (pages serveur, SEO complet)
