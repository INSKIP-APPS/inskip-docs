# Conditions de disponibilité

**INSKIP** · Version 1.0 · 27 juillet 2026
S'applique par défaut aux applications opérées par INSKIP ; des conditions
spécifiques peuvent être convenues contractuellement par projet et prévalent
alors sur le présent document.

## 1. Architecture et principes

Les applications INSKIP sont hébergées sur des infrastructures managées et
redondantes (Vercel pour l'applicatif, distribué par CDN ; Supabase pour les
données), qui portent leurs propres engagements de disponibilité. INSKIP ne
s'appuie sur aucun serveur auto-administré : la disponibilité de la plateforme
bénéficie directement de celle de ces fournisseurs spécialisés.

## 2. Objectif de niveau de service

- **Objectif indicatif de disponibilité : 99,5 % par mois calendaire**, hors
  maintenances planifiées et exclusions du §6.
- Il s'agit d'un objectif de service (SLO), pas d'un engagement contractuel
  assorti de pénalités, sauf stipulation contraire dans le contrat du projet.
- La disponibilité s'entend comme la capacité de l'application à répondre
  normalement aux requêtes (page d'accueil et parcours principaux).

## 3. Supervision et alertes

- Chaque application en production fait l'objet d'un **contrôle automatique de
  disponibilité toutes les 15 minutes**.
- Toute réponse anormale déclenche immédiatement une alerte à l'équipe INSKIP,
  tracée dans un registre jusqu'à résolution ; la clôture est automatique dès
  le retour à la normale, ce qui fournit un historique horodaté des incidents.

## 4. Maintenances et mises à jour

- Les déploiements applicatifs sont **sans interruption de service** :
  la nouvelle version est construite puis substituée atomiquement à
  l'ancienne ; en cas de problème, le retour à la version précédente
  s'effectue en quelques minutes.
- Les rares maintenances susceptibles d'affecter le service (ex. migration de
  base de données majeure) sont planifiées en dehors des heures ouvrées et
  annoncées au client au moins 48 h à l'avance.

## 5. Sauvegardes et reprise

- Sauvegardes automatiques quotidiennes des bases de données par le
  fournisseur ; restauration possible sur demande ou sur incident.
- Profondeur de rétention et granularité de restauration : selon le plan
  souscrit pour le projet, précisées contractuellement. *(Ordres de grandeur
  par défaut : rétention 7 jours, restauration au dernier point de sauvegarde
  quotidien — à confirmer par projet.)*

## 6. Exclusions

Ne sont pas comptabilisées comme indisponibilités imputables : les pannes
majeures des fournisseurs d'infrastructure dépassant leurs propres
engagements, les maintenances planifiées annoncées, les dysfonctionnements
causés par des systèmes tiers intégrés à la demande du client, les cas de
force majeure, et les usages anormaux ou malveillants (déni de service).

## 7. Support et traitement des demandes

| Sévérité | Exemple | Prise en compte visée |
|---|---|---|
| Critique — service indisponible | L'application ne répond plus | 4 h ouvrées |
| Majeure — fonction clé dégradée | Un parcours principal échoue | 1 jour ouvré |
| Mineure — gêne sans blocage | Anomalie d'affichage, question d'usage | 3 jours ouvrés |

Canal : email au contact projet (par défaut matthieu.chereau@inskip.fr).
Heures ouvrées : jours ouvrés en France, 9 h – 18 h (heure de Paris). Ces
délais sont des cibles de prise en compte, ajustables contractuellement.

## 8. Révision

Ces conditions sont versionnées et datées ; toute évolution est publiée au
même emplacement, l'historique restant consultable.
