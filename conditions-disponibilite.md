# Disponibilité et supervision des applications

**INSKIP** — s'applique par défaut aux applications opérées par INSKIP.
*Dispositif en vigueur depuis juillet 2026 · dernière mise à jour : juillet 2026*

> **Nature de ce document.** Il énonce notre **engagement de disponibilité**,
> décrit **comment** celle-ci est mesurée, et ce que nous communiquons à ce
> sujet. Les modalités de compensation éventuelle relèvent du contrat propre à
> chaque projet, qui prévaut alors sur le présent document.

## 1. Notre engagement

**INSKIP s'engage sur une disponibilité de 99,5 % par mois calendaire** pour
les applications qu'il opère, hors maintenances planifiées et hors facteurs
listés au §7.

Concrètement, ce seuil tolère environ **3 h 40 d'indisponibilité par mois**.
Cet engagement est mesuré par le dispositif décrit au §3, dont les relevés
mensuels sont conservés et communicables — nous ne demandons pas qu'on nous
croie sur parole.

En cas de mois passant sous ce seuil : analyse a posteriori documentée et
transmise au client, avec le plan d'action correctif. Les éventuelles
modalités de compensation financière sont définies contractuellement, projet
par projet.

## 2. Architecture et dépendances

Les applications INSKIP reposent sur des infrastructures managées et
redondantes — **Vercel** (hébergement applicatif, distribution par CDN) et
**Supabase** (base de données PostgreSQL managée). INSKIP n'administre aucun
serveur : la disponibilité du service dépend donc directement de celle de ces
fournisseurs, qui publient leurs propres engagements et états de service.

Notre engagement du §1 s'appuie sur la robustesse de ces plateformes, dont les
niveaux de service propres sont supérieurs au seuil que nous retenons. Il
s'accompagne de deux garanties complémentaires : la **transparence de la
mesure** (§3) et notre **réactivité** en cas d'incident (§5).

## 3. Comment la disponibilité est mesurée

Chaque application en production est supervisée par un dispositif automatisé,
intégré à son dépôt de code :

- **Sonde HTTP toutes les 15 minutes** : la page principale de l'application
  est appelée ; toute réponse anormale est traitée comme une indisponibilité.
- **Journal d'incidents horodaté** : une indisponibilité ouvre automatiquement
  une alerte datée, refermée automatiquement au retour à la normale avec la
  durée constatée. Ces horodatages sont générés par la plateforme
  d'automatisation (GitHub) et ne peuvent pas être modifiés par édition.
- **Rapport mensuel automatique** : le 1er de chaque mois, la disponibilité du
  mois écoulé est calculée à partir de ce journal (nombre d'incidents, durée
  cumulée, taux de disponibilité) et **versionnée dans le dépôt de
  l'application**. L'historique complet est ainsi auditable, mois par mois.

Ce dispositif est identique sur toutes nos applications : il fait partie du
standard technique appliqué par INSKIP.

### Limites assumées de la mesure

Nous préférons énoncer ces limites plutôt que d'annoncer une précision que la
mesure n'a pas :

- **Granularité de 15 minutes** : une interruption plus courte survenant entre
  deux sondes peut ne pas être détectée. Les durées sont donc données à
  ±15 minutes.
- **Périmètre** : la sonde vérifie que l'application répond, pas le bon
  fonctionnement de chaque fonctionnalité interne.
- Le dispositif s'appuie sur l'infrastructure d'automatisation de GitHub :
  une indisponibilité de celle-ci peut décaler une mesure.

## 4. Communication de la disponibilité, et portée de cette transparence

Sur demande, nous transmettons pour une application donnée : le relevé mensuel
de disponibilité, la liste horodatée des incidents de la période et leur durée.
Ces éléments sont extraits du journal décrit au §3 — ils ne sont pas
reconstitués a posteriori.

**Ce que cette transparence garantit — et ce qu'elle ne garantit pas.** Les
mesures sont produites automatiquement, horodatées par la plateforme et
conservées dans le dépôt de l'application, dont l'historique est versionné :
toute divergence entre le relevé transmis et le journal source serait
détectable par un audit ayant accès au dépôt. En revanche, ces données étant
hébergées dans nos dépôts privés, **elles ne sont pas vérifiables de façon
indépendante par un tiers** : la mesure est produite par nos propres outils.

Lorsqu'un projet requiert une garantie plus forte, deux options sont
disponibles et se décident au démarrage :

1. **Accès en lecture au journal de supervision** de l'application accordé au
   client ou à son auditeur : il consulte alors directement la source, sans
   passer par nos relevés.
2. **Supervision par un tiers indépendant** (service de monitoring externe avec
   page de statut publique) : la mesure n'est plus produite par nous. C'est la
   seule configuration qui rend la disponibilité vérifiable sans nous faire
   confiance ; elle implique un service supplémentaire, dont le coût est chiffré
   avec le projet.

## 5. Maintenances, incidents et support

- **Déploiements sans interruption** : chaque mise à jour est construite puis
  substituée à la version précédente ; un retour arrière est possible en
  quelques minutes.
- **Maintenances susceptibles d'affecter le service** (rares, ex. migration de
  base majeure) : planifiées hors heures ouvrées et annoncées au client à
  l'avance.
- **Traitement des incidents** : toute alerte de supervision notifie
  immédiatement l'équipe INSKIP. Nous intervenons dans les meilleurs délais
  pendant les heures ouvrées (jours ouvrés en France, 9 h – 18 h, heure de
  Paris), en priorisant les indisponibilités totales. Les délais d'intervention
  garantis, l'astreinte hors heures ouvrées et les niveaux de priorité formels
  peuvent être définis contractuellement, projet par projet.
- **Canal** : contact projet, à défaut matthieu.chereau@inskip.fr.

## 6. Sauvegardes et reprise

- Sauvegardes automatiques **quotidiennes** de la base de données par
  l'hébergeur, avec **7 jours de rétention** sur la configuration actuelle de
  nos projets.
- Restauration possible au dernier point de sauvegarde quotidien.
- Une restauration à un instant précis (*point-in-time recovery*) est
  disponible en option chez l'hébergeur ; elle n'est pas activée par défaut et
  peut être souscrite par projet lorsque le besoin le justifie.

## 7. Facteurs hors de notre maîtrise

Ne relèvent pas de notre responsabilité : les indisponibilités des
fournisseurs d'infrastructure (Vercel, Supabase) et de leurs propres
sous-jacents, celles des systèmes tiers intégrés à la demande du client, les
cas de force majeure, ainsi que les usages anormaux ou malveillants
(attaques par déni de service notamment).

## 8. Révision

Ce document est mis à jour au fil de l'évolution de nos pratiques ; chaque mise
à jour est publiée au même emplacement, l'historique restant consultable.
