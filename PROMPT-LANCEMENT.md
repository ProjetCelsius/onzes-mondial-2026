# Prompt de lancement - à coller demain matin dans une session fraîche

Copier tout le bloc ci-dessous tel quel. Conçu pour une exécution full autonomie.

---

Tu vas construire la proposition de V2 de la page Coupe du Monde de radarfoot.fr : "la ressource ultime pour suivre la Coupe du Monde 2026". Cadeau surprise pour le créateur du site. Tout est déjà préparé, ton travail est d'exécuter avec un niveau d'exigence maximal, en autonomie totale.

## Mise en route (dans cet ordre)

1. Token GitHub : Notion > page "Journal des sessions (historique)" > section "Informations techniques persistantes". RÈGLE ABSOLUE : jamais commité, les repos sont publics.
2. Clone `ProjetCelsius/onzes-mondial-2026` (référence documentaire) et `ProjetCelsius/radar-footprint` (repo produit, synchronisé Lovable : Guillaume y visualise le résultat).
3. Lis dans cet ordre : `CONTEXT.md` (état + données déjà collectées), `PLAN-BATAILLE.md` (LE document d'orchestration : pivot V2, architecture hub, mini-CDC des 10 features, séquencement, garde-fous), puis `CDC.md` (détail compos, charte graphique, méthodos). `FEATURES.md` = backlog au-delà du MVP.
4. Exécute le PLAN-BATAILLE phase par phase. Les données réelles (France + 7 équipes, sourcées) sont déjà dans `radar-footprint/src/data/mondial/`.

## Règles

- Autonomie : ne pose une question que si un choix change radicalement le produit. Sinon tranche, avance, consigne le choix.
- Qualité : "graphisme de média social pro", DA Radarfoot stricte (CDC section 5). Point de contrôle n°1 : le terrain France. Point de contrôle n°2 : side-by-side page actuelle vs V2.
- Données : zéro invention, tout sourcé (CDC 4.6), sous-agents par lots pour les 40 équipes restantes, les Elo et le calendrier.
- Commits atomiques, déploiement visible au plus tôt, CONTEXT.md mis à jour en fin de session.

## Livrable de fin de session

URL fonctionnelle + état des données (équipes complètes/light, sources) + les choix tranchés seuls + captures d'écran pour la démo au pote + reste à faire V2.

Vas-y.
