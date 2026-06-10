# Onzes du Mondial 2026

Hub interactif des compositions probables des 48 équipes de la Coupe du Monde 2026.
Projet cadeau pour [Radarfoot](https://radarfoot.fr).

## Ce que c'est

- Un hub compact des 48 équipes classées par groupe, avec note d'équipe type FUT
- Un terrain vertical par équipe : onze probable, % de titularisation par joueur, banc
- Une fiche joueur en overlay (club, âge, sélections, stats saison)
- Un comparateur de deux équipes (terrains + stats face à face)
- Un mode coach : fais ta propre compo, la note se recalcule, partage-la en lien ou en image

## Contenu du repo

| Fichier | Rôle |
|---|---|
| `CDC.md` | Cahier des charges complet : fonctionnel, données, charte graphique Radarfoot, UX, architecture, plan |
| `PROMPT-LANCEMENT.md` | Le prompt à coller dans une session Claude pour lancer l'implémentation autonome |
| `data/equipes.json` | Seed : les 48 équipes, 12 groupes, slugs, couleurs maillot |

## Statut

- 10 juin 2026 (J-1) : cahier des charges rédigé, repo créé. Implémentation prévue le 11 juin.

Stack cible : Vite + React + TypeScript + Tailwind, 100% statique, déployé sur GitHub Pages. Zéro backend.
