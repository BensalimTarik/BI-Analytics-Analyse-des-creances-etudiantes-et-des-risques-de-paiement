# Spécifications fonctionnelles – Analyse des créances étudiantes et du risque de paiement

## 1. Présentation du projet

Ce projet vise à mettre en place un dispositif structuré et pérenne de **suivi des créances étudiantes** et d'**analyse du risque de paiement** (Credit & Collection Risk) pour un réseau de **14 écoles** proposant plusieurs formations.

Il permet de centraliser les données de facturation, d'échéancier et d'encaissement, de mesurer l'encours et son ancienneté, d'évaluer la performance du recouvrement et d'identifier les étudiants à risque de retard de paiement. Les résultats aident la direction financière et les équipes de recouvrement à prioriser leurs actions et à sécuriser la trésorerie.

## 2. Objectifs métier

L'objectif général est d'automatiser la chaîne de traitement, de l'extraction des données de facturation jusqu'à leur restitution dans des tableaux de bord décisionnels.

Les objectifs spécifiques sont :

- **O1 – Visibilité :** consolider l'encours et les impayés des 14 écoles dans une vue unique.
- **O2 – Anticipation :** suivre l'échéancier et les montants à échoir pour prévoir les encaissements.
- **O3 – Recouvrement :** mesurer la performance du recouvrement et le délai moyen de paiement.
- **O4 – Risque :** identifier les étudiants et les créances à risque de retard grâce à un score.
- **O5 – Fiabilité :** garantir la cohérence entre le système de facturation, la base analytique et les tableaux de bord.
- **O6 – Pilotage :** couvrir les besoins analytiques prioritaires de la direction, des directeurs d'école et des équipes de recouvrement.

## 3. Périmètre fonctionnel et processus

Le dispositif couvre six étapes principales :

1. **Extraction des données sources :** récupération des factures, échéanciers, paiements, étudiants et formations depuis Dynamics 365 / le logiciel de facturation.
2. **Centralisation :** chargement dans une base SQL Server dédiée (zone de transit puis base analytique).
3. **Transformation et préparation :** nettoyage, normalisation, contrôles qualité et calcul des indicateurs.
4. **Modélisation :** construction d'un modèle dimensionnel (tables de faits et dimensions) adapté à Power BI.
5. **Scoring du risque :** calcul par un module Python d'un score de risque de retard de paiement par étudiant ou par créance.
6. **Reporting :** création de cinq tableaux de bord Power BI interactifs.

## 4. Tableaux de bord

| N° | Tableau de bord | Objectif | Principaux contenus |
|---|---|---|---|
| 1 | Aperçu financier de direction | Donner une vue consolidée au Groupe | Montants facturés, encaissés, encours total restant, taux d'impayés échus, évolution mensuelle, comparaison des écoles |
| 2 | Créances et échéancier | Suivre l'ancienneté et les échéances | Balance âgée (0–30, 31–60, 61–90, > 90 jours), échéances à venir, détail par école, formation et statut |
| 3 | Performance de recouvrement | Mesurer l'efficacité du recouvrement | Taux de recouvrement, délai moyen de paiement, tendances, comparaison par école, formation et mode de paiement |
| 4 | Segmentation des risques d'impayé | Prioriser les actions de relance | Niveaux de risque (faible, moyen, élevé) issus du score Python, caractéristiques des segments, liste des dossiers prioritaires |
| 5 | Rapprochement des données | Garantir la fiabilité des chiffres | Écarts entre facturation, base analytique et rapports, enregistrements orphelins, doublons, anomalies de saisie |

## 5. Indicateurs clés de performance (KPI)

| KPI | Définition / règle de calcul | Cible |
|---|---|---|
| Total facturé | Somme des montants facturés sur la période | Suivi évolutif |
| Total encaissé | Somme des paiements reçus sur la période | Suivi évolutif |
| Encours total restant | Σ (montant facturé − montant encaissé) des factures non soldées | Suivi évolutif |
| Taux d'impayés échus | Montants échus non réglés / montants échus × 100 | À définir (proposition : < 10 %) |
| Balance âgée | Répartition de l'encours échu par ancienneté de retard : 0–30, 31–60, 61–90, > 90 jours | Réduction de la tranche > 90 jours |
| Délai moyen de paiement | Moyenne (date de paiement − date d'échéance) en jours, pondérée par les montants | À définir (proposition : ≤ 15 jours) |
| Taux de recouvrement | Montants encaissés / montants échus × 100 | À définir (proposition : ≥ 90 %) |
| Étudiants à risque élevé | Nombre et part des étudiants dont le score dépasse le seuil de risque élevé | Suivi évolutif |

Les cibles sont indicatives et doivent être validées et ajustées avec la direction financière.

## 6. Architecture des données et sources

### Sources de données

| Source | Description | Accès |
|---|---|---|
| Dynamics 365 / logiciel de facturation | Factures, échéanciers, paiements, étudiants, formations, écoles | Lecture selon autorisations |
| Base SQL Server (zone de transit) | Copie brute des données sources | Lecture et écriture contrôlées |
| Base SQL Server Data Analytics | Données transformées et modèle dimensionnel pour Power BI | Lecture et écriture contrôlées |

Les données sont extraites depuis le système source et transférées vers la base analytique selon une fréquence définie par les besoins du projet.

### Modèle dimensionnel (proposition)

| Type | Table | Contenu |
|---|---|---|
| Fait | Fact_Factures | Montant facturé, date de facture, date d'échéance, statut |
| Fait | Fact_Paiements | Montant payé, date de paiement, mode de paiement |
| Fait | Fact_Echeancier | Échéances prévues, montants à échoir, retards |
| Dimension | Dim_Etudiant | Identifiant anonymisé de l'étudiant, profil, statut |
| Dimension | Dim_Ecole | École, ville, région |
| Dimension | Dim_Formation | Formation, niveau, durée, année académique |
| Dimension | Dim_Date | Calendrier (jour, mois, trimestre, année) |
| Dimension | Dim_Statut | Statuts de facture et de paiement |
| Résultat | Score_Risque | Score de risque de retard, niveau de risque, date de calcul |

### Qualité des données

Les contrôles portent sur six dimensions :

- **Complétude :** présence des informations obligatoires (identifiants, montants, dates d'échéance).
- **Exactitude :** validité des montants et des dates.
- **Cohérence :** concordance entre factures, échéanciers et paiements.
- **Unicité :** identification unique des factures et des paiements, contrôle des doublons.
- **Fraîcheur :** actualisation des données selon une fréquence documentée.
- **Traçabilité :** documentation des sources, des autorisations et des traitements.

## 7. Règles de gestion

| ID | Règle | Description | Statut |
|---|---|---|---|
| RG-DEF-001 | Créance échue | Une créance est échue lorsque sa date d'échéance est dépassée et que son solde est supérieur à zéro. | Brouillon |
| RG-CAL-001 | Balance âgée | L'ancienneté se calcule en jours de retard par rapport à la date d'échéance, selon les tranches 0–30, 31–60, 61–90 et > 90 jours. | Brouillon |
| RG-CAL-002 | Taux de recouvrement | Calculé sur les montants échus à la date d'arrêté du rapport. | Brouillon |
| RG-PER-001 | Périmètre | Les écoles, formations et périodes retenues sont incluses ; les factures annulées et les avoirs sont traités séparément. | Brouillon |
| RG-QUA-001 | Qualité | Les identifiants, montants et dates sont contrôlés avant chargement dans la base analytique. | Brouillon |
| RG-EXC-001 | Anomalies | Les paiements non associés à une facture, les doublons et les montants incohérents sont signalés et vérifiés avant réintégration. | Brouillon |
| RG-SCO-001 | Scoring | Le score de risque est recalculé à une fréquence définie ; les seuils des niveaux faible, moyen et élevé sont validés par la direction financière. | Brouillon |
| RG-SEC-001 | Sécurité | Les accès aux données nominatives et aux résultats sont limités selon les rôles et périmètres autorisés (sécurité au niveau des lignes par école). | Brouillon |

## 8. Valeur ajoutée du projet

Le dispositif met à disposition un référentiel d'indicateurs et des tableaux de bord interactifs permettant notamment de :

- Suivre l'encours, les impayés et la balance âgée à l'échelle du réseau.
- Comparer les écoles, formations et promotions.
- Anticiper les encaissements grâce à l'échéancier.
- Mesurer la performance du recouvrement et le délai moyen de paiement.
- Cibler les relances sur les dossiers à plus fort risque de retard.
- Vérifier la cohérence entre le système de facturation et les rapports.
- Accompagner le pilotage financier et la sécurisation de la trésorerie.

---

**Projet :** Analyse des créances étudiantes et du risque de paiement  
**Périmètre :** Réseau de 14 écoles  
**Outil de reporting :** Microsoft Power BI

