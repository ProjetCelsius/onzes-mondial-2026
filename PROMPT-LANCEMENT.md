# Prompt de lancement - à coller demain dans une session fraîche

Copier tout le bloc ci-dessous tel quel. Il est conçu pour une exécution quasi autonome.

---

Tu vas construire "Onzes du Mondial 2026", un hub interactif des compositions probables des 48 équipes de la Coupe du Monde, cadeau surprise pour le site radarfoot.fr. Tout est déjà spécifié, ton travail est d'exécuter avec un niveau d'exigence maximal.

## Mise en route

1. Récupère le token GitHub dans mon Notion : page "Journal des sessions (historique)", section "Informations techniques persistantes". REGLE ABSOLUE : ce token ne doit jamais apparaître dans un fichier commité, le repo est public.
2. Clone `onzes-mondial-2026` depuis mon compte GitHub. Lis D'ABORD `CONTEXT.md` (état du projet, données déjà collectées, répartition Claude/Lovable, repo produit = `ProjetCelsius/radar-footprint`), puis `CDC.md` ENTIEREMENT avant d'écrire la moindre ligne de code. C'est le document de référence : périmètre, modèle de données, charte graphique, méthodos de calcul, plan d'implémentation. `data/equipes.json` contient le seed des 48 équipes.
3. Visite radarfoot.fr et radarfoot.fr/coupe-du-monde-2026 pour t'imprégner de la DA réelle, et recale les tokens de la section 5 du CDC sur le CSS live si tu constates un écart.

## Règles d'exécution

- Autonomie totale : ne me pose une question que si un choix change radicalement le produit. Sinon, tranche et avance, je corrigerai au résultat.
- Qualité : chaque écran doit être au niveau "graphisme de média social pro" (section 5.5 du CDC). Si c'est correct mais pas remarquable, ce n'est pas fini. Le point de contrôle critique est le rendu du terrain France : ne lance pas l'industrialisation des 47 autres équipes avant que ce rendu soit irréprochable.
- Données : ZERO donnée inventée. Effectifs et compos probables sourcés par recherche web réelle (listes officielles des 26, compos probables de la presse française et internationale datées de moins de 10 jours), consignés dans `data/SOURCES.md`. Champ introuvable = null, jamais une invention. Les % et notes se calculent avec les méthodos des sections 4.4 et 4.5.
- Parallélisation : utilise des sous-agents pour la collecte de données par lots d'équipes pendant que tu construis l'UI. Suis les phases P0 (France parfaite) puis P1 (15 têtes d'affiche) puis P2 (les 48 en mode light minimum) définies en 4.7.
- Commits atomiques au fil de l'eau, messages clairs. Déploie GitHub Pages dès le premier commit fonctionnel pour que je puisse suivre en live.

## Ordre de bataille (détail section 8 du CDC)

1. Scaffold Vite + React + TS + Tailwind, tokens DA, déploiement Pages opérationnel.
2. Design system terrain : Pitch, PlayerToken (maillot SVG + numéro + cartouche + % + note), overlay fiche joueur. Validation esthétique sur la France.
3. Données P0 puis P1 puis P2 (sous-agents).
4. Hub > page équipe > fiche joueur > duel > mode coach (drag & drop, presets formations, recalcul de note en direct, partage URL compressée, export image story/post).
5. Page Méthodo. Checklist Definition of done (section 9 du CDC), test mobile, Lighthouse.

## Livrable de fin de session

- URL GitHub Pages fonctionnelle
- Etat des données : combien d'équipes complètes / light, avec sources
- Les 3 choix que tu as tranchés seuls et pourquoi
- Ce qui reste pour la V2 (section 10 du CDC)

Vas-y.
