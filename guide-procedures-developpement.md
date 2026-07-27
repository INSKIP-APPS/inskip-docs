# Guide des procédures de développement et outils utilisés

**INSKIP** · Version 1.1 · 27 juillet 2026
S'applique à l'ensemble des applications développées et opérées par INSKIP.

## 1. Objet

Ce guide décrit les procédures, outils et contrôles qualité qu'INSKIP applique
au développement de ses applications. Il est tenu à jour en continu ; la
version publiée fait foi, son historique de modifications est traçable.

## 2. Outils et technologies

| Fonction | Outil | Usage |
|---|---|---|
| Gestion du code source | GitHub (organisation dédiée INSKIP) | Dépôts privés, revue de code, traçabilité complète des modifications |
| Framework applicatif | React + TypeScript (Vite) | Applications web ; le typage statique est vérifié à chaque build |
| Base de données et authentification | Supabase (PostgreSQL managé, région Paris — France) | Données, comptes utilisateurs, contrôle d'accès au niveau des lignes (RLS) |
| Hébergement et déploiement | Vercel | Déploiement continu, CDN, environnements séparés |
| Tests | Vitest, Testing Library, Playwright | Voir §4 |
| Intégration continue | GitHub Actions | Voir §5 |
| Assistants IA | Claude (Anthropic) | Encadrés par une configuration d'entreprise versionnée, voir §7 |

## 3. Organisation du code et des versions

- Chaque application vit dans un dépôt Git **privé**, sur l'organisation
  GitHub d'INSKIP — jamais sur un compte individuel.
- Deux branches permanentes : `main` (production) et `staging`
  (pré-production). Le travail passe par des branches de fonctionnalité et des
  **pull requests relues** avant intégration.
- L'identité des auteurs de commits est contrôlée (un garde-fou automatique
  bloque les commits dont l'identité ne correspond pas à un compte vérifié),
  garantissant la traçabilité de chaque modification.

## 4. Qualité et tests

- **Tests unitaires et de composants** (Vitest + Testing Library) : logique
  métier et composants d'interface, colocalisés avec le code testé.
- **Tests de bout en bout** (Playwright) : les parcours critiques de
  l'application sont vérifiés sur l'environnement de pré-production — jamais
  sur la production.
- Un correctif ou une évolution n'est intégré que si l'ensemble des tests
  passe.

## 5. Intégration continue

À chaque modification proposée (push ou pull request), une chaîne
d'intégration continue exécute automatiquement : l'installation propre des
dépendances, la totalité des tests unitaires et de composants, puis la
compilation TypeScript et le build de production. **Un échec bloque
l'intégration** — aucune modification ne peut atteindre la production sans
être passée par cette chaîne.

## 6. Environnements et déploiement

- **Production** : déployée automatiquement depuis la branche `main`.
- **Pré-production (staging)** : environnement séparé, avec sa **propre base
  de données** — l'isolation est assurée au niveau de l'infrastructure, les
  données de production ne sont jamais utilisées en pré-production.
- Les variables d'environnement (dont les secrets) sont strictement séparées
  entre environnements et ne figurent jamais dans le code source.
- Les déploiements sont atomiques et réversibles : un retour à la version
  précédente s'effectue en quelques minutes.

## 7. Usage des assistants IA

INSKIP utilise des assistants IA (Claude) dans son cycle de développement,
encadrés par une configuration d'entreprise centralisée et versionnée qui
impose notamment : la revue humaine des modifications, l'interdiction d'actions
externes non validées, et des règles de confidentialité sur les données
clients. Le code produit avec assistance IA suit exactement les mêmes
contrôles (tests, CI, revue) que le reste.

## 8. Supervision en production

Chaque application déployée embarque, dans son propre dépôt, un dispositif de
supervision automatisé : sonde de disponibilité toutes les 15 minutes, journal
d'incidents horodaté, et rapport mensuel de disponibilité versionné. Ce
dispositif fait partie du standard technique appliqué à toutes nos
applications — il n'est pas ajouté au cas par cas. Détail dans le document
*Disponibilité et supervision*.

## 9. Documentation et traçabilité

Les décisions d'architecture et les standards sont documentés et versionnés.
L'historique Git constitue la piste d'audit complète : qui a modifié quoi,
quand, et après quelle revue.

## 10. Évolution du présent guide

Ce guide évolue avec nos pratiques. Toute modification est versionnée, datée
et relue avant publication. Contact : matthieu.chereau@inskip.fr.
