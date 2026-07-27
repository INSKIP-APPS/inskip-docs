# Politique de sécurité

**INSKIP** — s'applique à l'ensemble des applications développées et opérées par INSKIP.
*Dernière mise à jour : juillet 2026*

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

### Localisation des données — le détail

**Base de données (données persistantes, données personnelles).** Hébergée dans
la **région Paris, France** (`eu-west-3`) pour nos applications. Quelques
projets antérieurs à cette règle sont hébergés dans une autre région de l'Union
européenne (Irlande). La région applicable à votre application vous est
précisée sur demande et peut être confirmée contractuellement.

**Hébergement applicatif (Vercel).** Vercel est une société de droit américain
dont l'infrastructure s'appuie sur AWS. Son réseau comprend plus de 120 points
de présence répartis dans le monde, et une vingtaine de régions de calcul, dont
**Paris (`cdg1`)**. Ce que cela implique concrètement pour nos applications :

- **La majorité de nos applications sont des applications web statiques** :
  Vercel n'y distribue que les fichiers de l'interface (HTML, JavaScript,
  styles), qui ne contiennent aucune donnée personnelle. Ces fichiers sont mis
  en cache dans les points de présence mondiaux pour la performance. **Les
  données, elles, circulent directement entre le navigateur de l'utilisateur et
  la base hébergée en France** — elles ne transitent ni ne séjournent dans les
  serveurs de calcul de Vercel.
- **Pour les rares applications comportant du code exécuté côté serveur**
  (fonctions), la région d'exécution est un paramètre explicite. Notre règle
  est de la fixer sur **Paris (`cdg1`)**, au plus près de la base de données.
  À défaut de configuration, Vercel exécute par défaut en Virginie
  (États-Unis) : nous vérifions ce point application par application et le
  corrigeons le cas échéant. La configuration effective de votre application
  vous est communiquée sur demande.
- Le chiffrement TLS est terminé dans la région Vercel qui traite la requête.

**Transferts hors Union européenne.** Vercel étant un fournisseur américain,
les transferts éventuels (notamment métadonnées techniques et journaux) sont
encadrés par sa certification **Data Privacy Framework UE–États-Unis** et par
les clauses contractuelles types. Pour Supabase, voir le tableau ci-dessous.

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
| PCI DSS v4.0 | Attestation de conformité (prestataire et commerçant) |
| Data Privacy Framework UE–États-Unis | Certifié, inscription publique consultable |
| TISAX (niveau 2) | Évaluation obtenue (exigences du secteur automobile) |
| HIPAA | Disponible pour les offres Enterprise du fournisseur |
| RGPD | Sous-traitant, accord de traitement des données (DPA) disponible |
| Chiffrement | **AES-256 au repos**, HTTPS/TLS en transit |
| Régions de calcul | 20 régions dont **Paris (`cdg1`)** ; plus de 120 points de présence pour la diffusion |

Ces informations proviennent des publications de sécurité de nos fournisseurs.
Les rapports d'audit eux-mêmes (SOC 2, certificat ISO 27001) sont délivrés par
ces fournisseurs, selon les conditions d'accès qu'ils définissent ; nous
relayons toute demande client en ce sens.

## 4. Chiffrement des échanges et certificats

**Toutes nos applications sont exclusivement servies en HTTPS.** Chaque site
public dispose d'un **certificat TLS** valide, provisionné et **renouvelé
automatiquement** par l'hébergeur — il n'y a donc pas de risque d'expiration
par oubli humain. Les certificats sont émis par des autorités publiquement
reconnues (Google Trust Services, Let's Encrypt selon la plateforme).

- **HTTPS est forcé** : toute requête en HTTP est redirigée vers HTTPS ; aucune
  donnée ne circule en clair.
- Le chiffrement s'applique de bout en bout : navigateur → application (TLS),
  puis application → base de données (TLS), et les données sont chiffrées au
  repos en AES-256 (voir §3).
- Les certificats couvrent également les environnements de pré-production.

## 5. Contrôle des accès

- Les dépôts de code sont **privés**, hébergés sur l'organisation GitHub
  d'INSKIP — jamais sur un compte individuel. L'accès est nominatif et limité
  aux intervenants du projet (moindre privilège).
- Les accès aux consoles d'administration (Vercel, Supabase, GitHub) sont
  restreints aux membres habilités de l'équipe INSKIP, chaque accès étant
  nominatif — aucun compte partagé, ce qui garantit l'imputabilité des actions.
- **Authentification à deux facteurs** : son activation est requise par notre
  politique interne sur l'ensemble des comptes d'administration ; sa
  généralisation et son application obligatoire au niveau de l'organisation
  sont en cours de déploiement.
- Côté applicatif, le contrôle d'accès aux données repose sur
  l'authentification de la plateforme et des **politiques de sécurité au niveau
  des lignes** (Row Level Security), définies par application et versionnées
  avec le code.

### Authentification multifacteur des back-offices

**L'accès aux back-offices d'administration de nos applications est protégé par
une authentification multifacteur (MFA).** Concrètement :

- un compte disposant de privilèges d'administration ne peut pas accéder à
  l'interface d'administration avec un simple mot de passe : un second facteur
  est exigé à l'authentification ;
- le second facteur repose sur un code à usage unique généré par application
  d'authentification (TOTP, standard RFC 6238) — sans dépendance à un service
  tiers ni au réseau téléphonique ;
- cette exigence fait partie du référentiel technique appliqué à nos
  applications : elle est vérifiée avant mise en service, et contrôlée lors de
  nos audits internes périodiques.

Les comptes utilisateurs non privilégiés relèvent de la politique
d'authentification définie avec le client pour chaque application.

## 6. Gestion des secrets

- Aucun secret (clé d'API, identifiant, jeton) n'est stocké dans le code
  source : les secrets résident dans les variables d'environnement des
  plateformes, **séparées par environnement** (production ≠ pré-production).
- Les fichiers d'environnement locaux sont exclus du versionnement par
  configuration.
- En cas d'exposition suspectée : révocation et rotation immédiates, puis
  analyse d'impact.

## 7. Isolation des environnements et des données

- La pré-production dispose de sa **propre base de données**, distincte de
  celle de production : l'isolation est assurée au niveau de l'infrastructure,
  et non par une simple séparation logique.
- Les données réelles ne sont jamais copiées en pré-production ; les tests
  automatisés et jeux d'exemple utilisent des données fictives.

## 8. Développement sécurisé

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

## 9. Supervision et gestion des incidents

- Chaque application en production est supervisée automatiquement (sonde toutes
  les 15 minutes) ; toute anomalie déclenche une alerte horodatée à l'équipe,
  tracée jusqu'à résolution. Voir le document *Disponibilité et supervision*.
- En cas d'incident de sécurité : confinement, correction, puis analyse
  a posteriori documentée.
- Si des données d'un client sont concernées, celui-ci est informé dans les
  meilleurs délais, et les obligations réglementaires applicables — notamment
  la notification à l'autorité compétente sous 72 heures au titre du RGPD —
  sont respectées.

## 10. Sauvegardes

Sauvegardes automatiques quotidiennes des bases de données par l'hébergeur,
avec 7 jours de rétention sur la configuration actuelle de nos projets. La
restauration à un instant précis est disponible en option et souscrite par
projet lorsque le besoin le justifie.

## 11. Données personnelles

INSKIP applique les principes du RGPD : minimisation des données collectées,
finalités définies par application, sous-traitants encadrés par leurs accords
de traitement (DPA) et, pour les transferts hors Union européenne, par les
clauses contractuelles types. Les demandes d'exercice de droits sont traitées
via le contact ci-dessous.

## 12. Signalement d'une vulnérabilité

Toute vulnérabilité ou préoccupation de sécurité peut être signalée à
**matthieu.chereau@inskip.fr**. Nous accusons réception sous 2 jours ouvrés.

## 13. Révision

Cette politique est revue à chaque évolution significative de nos pratiques et
au minimum une fois par an. Chaque mise à jour est datée et son historique
conservé.
