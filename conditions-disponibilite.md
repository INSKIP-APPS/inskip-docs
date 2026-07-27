# Disponibilité et supervision des applications

**INSKIP** · Version 2.0 · 27 juillet 2026

> **Nature de ce document.** Il décrit **comment** la disponibilité de nos
> applications est supervisée et mesurée, et ce que nous communiquons à ce
> sujet. Il ne constitue **pas un engagement de niveau de service (SLA)** :
> aucun taux de disponibilité garanti, aucune pénalité ni compensation n'en
> découle. Tout engagement de cette nature relève exclusivement du contrat
> propre à chaque projet, qui prévaut alors sur le présent document.

## 1. Architecture et dépendances

Les applications INSKIP reposent sur des infrastructures managées et
redondantes — **Vercel** (hébergement applicatif, distribution par CDN) et
**Supabase** (base de données PostgreSQL managée). INSKIP n'administre aucun
serveur : la disponibilité du service dépend donc directement de celle de ces
fournisseurs, qui publient leurs propres engagements et états de service.

Il en résulte que la disponibilité effective d'une application n'est pas
entièrement sous notre maîtrise. Nous nous engageons en revanche sur la
**transparence de sa mesure** (§2) et sur notre **réactivité** (§4).

## 2. Comment la disponibilité est mesurée

Chaque application en production est supervisée par un dispositif automatisé,
intégré à son dépôt de code :

- **Sonde HTTP toutes les 15 minutes** : la page principale de l'application
  est appelée ; toute réponse anormale est traitée comme une indisponibilité.
- **Journal d'incidents horodaté** : une indisponibilité ouvre automatiquement
  une alerte datée, refermée automatiquement au retour à la normale avec la
  durée constatée. Ces horodatages sont produits et conservés par la
  plateforme (GitHub), sans intervention manuelle possible.
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

## 3. Communication de la disponibilité

Sur demande, nous transmettons pour une application donnée : le relevé mensuel
de disponibilité, la liste horodatée des incidents de la période et leur durée.
Ces éléments sont extraits du journal décrit au §2 — ils ne sont pas
reconstitués a posteriori.

## 4. Maintenances, incidents et support

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

## 5. Sauvegardes et reprise

- Sauvegardes automatiques **quotidiennes** de la base de données par
  l'hébergeur, avec **7 jours de rétention** sur la configuration actuelle de
  nos projets.
- Restauration possible au dernier point de sauvegarde quotidien.
- Une restauration à un instant précis (*point-in-time recovery*) est
  disponible en option chez l'hébergeur ; elle n'est pas activée par défaut et
  peut être souscrite par projet lorsque le besoin le justifie.

## 6. Facteurs hors de notre maîtrise

Ne relèvent pas de notre responsabilité : les indisponibilités des
fournisseurs d'infrastructure (Vercel, Supabase) et de leurs propres
sous-jacents, celles des systèmes tiers intégrés à la demande du client, les
cas de force majeure, ainsi que les usages anormaux ou malveillants
(attaques par déni de service notamment).

## 7. Révision

Ce document est versionné et daté ; toute évolution est publiée au même
emplacement, l'historique restant consultable.
