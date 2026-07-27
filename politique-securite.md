# Politique de sécurité

**INSKIP** · Version 1.0 · 27 juillet 2026
S'applique à l'ensemble des applications développées et opérées par INSKIP.

## 1. Principes

La sécurité des applications INSKIP repose sur trois principes : s'appuyer sur
des fournisseurs d'infrastructure spécialisés et reconnus plutôt que sur des
serveurs auto-administrés ; isoler strictement les environnements et les
données ; rendre chaque modification traçable et relue.

## 2. Hébergement et sous-traitants

| Fournisseur | Rôle | Données concernées |
|---|---|---|
| Vercel | Hébergement applicatif, CDN | Code applicatif servi, journaux techniques |
| Supabase (PostgreSQL managé) | Base de données, authentification, stockage | Données applicatives et comptes utilisateurs |
| GitHub | Code source, intégration continue | Code (aucune donnée client dans les dépôts) |

Ces fournisseurs assurent le chiffrement en transit (TLS) et au repos, la
redondance et les correctifs d'infrastructure. La localisation des données est
définie projet par projet à la création de la base *(par défaut : région
Union européenne — à confirmer contractuellement selon le projet)*.

## 3. Contrôle des accès

- Les dépôts de code sont **privés**, hébergés sur l'organisation GitHub
  d'INSKIP ; l'accès est nominatif et limité aux intervenants du projet
  (principe du moindre privilège).
- Les accès aux consoles d'administration (Vercel, Supabase, GitHub) sont
  restreints à l'équipe INSKIP habilitée, avec authentification à deux
  facteurs *(généralisation en cours de vérification sur l'ensemble des
  comptes de l'organisation)*.
- Côté applicatif, le contrôle d'accès aux données s'appuie sur
  l'authentification Supabase et des règles d'accès au niveau des lignes
  (Row Level Security), définies par application.

## 4. Gestion des secrets

- Aucun secret (clé d'API, identifiant, jeton) n'est stocké dans le code
  source : les secrets vivent dans les variables d'environnement des
  plateformes, séparées par environnement (production ≠ pré-production).
- Les fichiers d'environnement locaux sont systématiquement exclus du
  versionnement.
- En cas d'exposition suspectée d'un secret : révocation et rotation
  immédiates, puis analyse d'impact.

## 5. Isolation des environnements et des données

- La pré-production utilise une **base de données distincte** de la
  production : les données réelles ne sortent jamais de l'environnement de
  production.
- Les tests automatisés et les jeux d'exemple utilisent des données fictives.

## 6. Développement sécurisé

- Toute modification passe par une pull request relue et une chaîne
  d'intégration continue bloquante (tests + compilation).
- L'identité des auteurs de commits est vérifiée par un contrôle automatique.
- Les dépendances sont installées à versions verrouillées (lockfile) ; une
  revue des vulnérabilités des dépendances est réalisée lors des cycles de
  maintenance *(automatisation via alertes de sécurité GitHub : en cours de
  généralisation)*.
- Les assistants IA utilisés en développement sont encadrés par une
  configuration d'entreprise qui interdit toute action externe non validée et
  toute exfiltration de données clients.

## 7. Supervision et gestion des incidents

- Chaque application en production est supervisée automatiquement
  (vérification de disponibilité toutes les 15 minutes) ; une indisponibilité
  déclenche une alerte immédiate à l'équipe, tracée jusqu'à résolution.
- En cas d'incident de sécurité : confinement, correction, puis analyse
  a posteriori documentée. Si des données d'un client sont concernées, le
  client est notifié dans les meilleurs délais, et les obligations
  réglementaires applicables (notamment RGPD, le cas échéant sous 72 h auprès
  de l'autorité compétente) sont respectées.

## 8. Sauvegardes

Les bases de données bénéficient des sauvegardes automatiques quotidiennes du
fournisseur, avec restauration possible *(profondeur de rétention et
restauration à un instant donné selon le plan souscrit par projet — précisé
contractuellement)*.

## 9. Données personnelles

INSKIP applique les principes du RGPD : minimisation des données collectées,
finalités définies par application, sous-traitants encadrés par leurs accords
de traitement de données (DPA), et droits des personnes exercés via le contact
ci-dessous.

## 10. Signalement

Toute vulnérabilité ou préoccupation de sécurité peut être signalée à
**matthieu.chereau@inskip.fr**. Nous accusons réception sous 2 jours ouvrés.

## 11. Révision

Cette politique est revue à chaque évolution significative de nos pratiques et
au minimum une fois par an. Chaque version est datée et son historique conservé.
