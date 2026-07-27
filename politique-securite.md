# Politique de sécurité

**INSKIP** · Version 2.0 · 27 juillet 2026
S'applique à l'ensemble des applications développées et opérées par INSKIP.

## 1. Principes

La sécurité des applications INSKIP repose sur trois principes : s'appuyer sur
des fournisseurs d'infrastructure certifiés plutôt que sur des serveurs
auto-administrés ; isoler strictement les environnements et les données ;
rendre chaque modification traçable et relue.

## 2. Hébergement, localisation des données et sous-traitants

| Fournisseur | Rôle | Données concernées |
|---|---|---|
| **Supabase** (PostgreSQL managé) | Base de données, authentification, stockage de fichiers | Données applicatives et comptes utilisateurs — **les données persistantes** |
| **Vercel** | Hébergement applicatif, distribution par CDN | Application servie, journaux techniques, mises en cache temporaires |
| **GitHub** | Code source, intégration continue, supervision | Code applicatif (aucune donnée client dans les dépôts) |

**Localisation des données.** Les données persistantes de nos applications sont
hébergées dans la **région Paris (France)** de notre hébergeur de bases de
données. Quelques projets antérieurs à cette règle sont hébergés dans une autre
région de l'Union européenne (Irlande). La région applicable à votre
application vous est précisée sur demande et peut être confirmée
contractuellement.

Vercel assure la diffusion de l'application via son réseau : il peut mettre en
cache temporairement des réponses dans ses points de présence, mais **le
stockage durable des données reste dans la base hébergée en France**.

## 3. Certifications et conformité de nos hébergeurs

Nous ne revendiquons aucune certification en propre : INSKIP est un cabinet de
conseil et de développement, non un hébergeur. Notre sécurité s'appuie sur des
plateformes certifiées, dont voici l'état des certifications à la date de ce
document.

**Supabase** (base de données, authentification, stockage)

| Cadre | Statut |
|---|---|
| SOC 2 Type 2 | Certifié — rapport d'audit délivré par le fournisseur selon ses conditions |
| ISO 27001 | Certifié |
| HIPAA | Conformité disponible, sous accord spécifique (BAA) auprès du fournisseur |
| RGPD | Agit en qualité de **sous-traitant** (*processor*) pour les données clients, avec accord de traitement des données (DPA) |
| Transferts hors UE | Encadrés par les **clauses contractuelles types** approuvées par la Commission européenne |
| Chiffrement | **AES-256 au repos**, TLS en transit ; jetons et clés chiffrés au niveau applicatif |

**Vercel** (hébergement et diffusion applicative)

| Cadre | Statut |
|---|---|
| ISO 27001 | Certifié |
| SOC 2 Type 2 | Attestation disponible auprès du fournisseur |
| PCI DSS v4.0 | Attestation de conformité |
| Data Privacy Framework (EU-US) | Certifié, inscription publique |
| TISAX (niveau 2) | Évaluation obtenue (secteur automobile) |
| HIPAA | Disponible pour les offres Enterprise du fournisseur |
| Chiffrement | **AES-256 au repos**, HTTPS/TLS en transit |

Ces informations proviennent des publications de sécurité de nos fournisseurs.
Les rapports d'audit eux-mêmes (SOC 2, certificat ISO 27001) sont délivrés par
ces fournisseurs, selon les conditions d'accès qu'ils définissent ; nous
relayons toute demande client en ce sens.

## 4. Contrôle des accès

- Les dépôts de code sont **privés**, hébergés sur l'organisation GitHub
  d'INSKIP — jamais sur un compte individuel. L'accès est nominatif et limité
  aux intervenants du projet (moindre privilège).
- Les accès aux consoles d'administration (Vercel, Supabase, GitHub) sont
  restreints à l'équipe INSKIP habilitée, avec authentification à deux
  facteurs.
- Côté applicatif, le contrôle d'accès aux données repose sur
  l'authentification de la plateforme et des **politiques de sécurité au niveau
  des lignes** (Row Level Security), définies par application et versionnées
  avec le code.

## 5. Gestion des secrets

- Aucun secret (clé d'API, identifiant, jeton) n'est stocké dans le code
  source : les secrets résident dans les variables d'environnement des
  plateformes, **séparées par environnement** (production ≠ pré-production).
- Les fichiers d'environnement locaux sont exclus du versionnement par
  configuration.
- En cas d'exposition suspectée : révocation et rotation immédiates, puis
  analyse d'impact.

## 6. Isolation des environnements et des données

- La pré-production dispose de sa **propre base de données**, distincte de
  celle de production : l'isolation est assurée au niveau de l'infrastructure,
  et non par une simple séparation logique.
- Les données réelles ne sont jamais copiées en pré-production ; les tests
  automatisés et jeux d'exemple utilisent des données fictives.

## 7. Développement sécurisé

- Toute modification passe par une revue de code (pull request) et une chaîne
  d'intégration continue **bloquante** : tests automatisés et compilation
  doivent réussir avant intégration.
- L'identité des auteurs de commits est vérifiée par un contrôle automatique,
  garantissant la traçabilité nominative de chaque modification.
- Les dépendances sont installées à versions verrouillées ; les alertes de
  sécurité de la plateforme sont suivies lors des cycles de maintenance.
- Les assistants IA utilisés en développement sont encadrés par une
  configuration d'entreprise centralisée qui interdit toute action externe non
  validée et toute transmission de données clients hors du cadre défini.

## 8. Supervision et gestion des incidents

- Chaque application en production est supervisée automatiquement (sonde toutes
  les 15 minutes) ; toute anomalie déclenche une alerte horodatée à l'équipe,
  tracée jusqu'à résolution. Voir le document *Disponibilité et supervision*.
- En cas d'incident de sécurité : confinement, correction, puis analyse
  a posteriori documentée.
- Si des données d'un client sont concernées, celui-ci est informé dans les
  meilleurs délais, et les obligations réglementaires applicables — notamment
  la notification à l'autorité compétente sous 72 heures au titre du RGPD —
  sont respectées.

## 9. Sauvegardes

Sauvegardes automatiques quotidiennes des bases de données par l'hébergeur,
avec 7 jours de rétention sur la configuration actuelle de nos projets. La
restauration à un instant précis est disponible en option et souscrite par
projet lorsque le besoin le justifie.

## 10. Données personnelles

INSKIP applique les principes du RGPD : minimisation des données collectées,
finalités définies par application, sous-traitants encadrés par leurs accords
de traitement (DPA) et, pour les transferts hors Union européenne, par les
clauses contractuelles types. Les demandes d'exercice de droits sont traitées
via le contact ci-dessous.

## 11. Signalement d'une vulnérabilité

Toute vulnérabilité ou préoccupation de sécurité peut être signalée à
**matthieu.chereau@inskip.fr**. Nous accusons réception sous 2 jours ouvrés.

## 12. Révision

Cette politique est revue à chaque évolution significative de nos pratiques et
au minimum une fois par an. Chaque version est datée et son historique conservé.
