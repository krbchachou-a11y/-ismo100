# UqarMDD — Base de données multimodale pour la détection de la somnolence

> Université du Québec à Rimouski (Campus de Lévis)  
> Responsables : Yacine Yaddaden, Ph. D. & Ismael Adji Haman

---

## Table des matières

1. [À propos du projet](#à-propos-du-projet)
2. [Matériel et logiciels](#matériel-et-logiciels)
3. [Structure du dépôt](#structure-du-dépôt)
4. [Protocole expérimental](#protocole-expérimental)
5. [Convention de nommage des fichiers](#convention-de-nommage-des-fichiers)
6. [Synchronisation des modalités](#synchronisation-des-modalités)
7. [Critères de qualité](#critères-de-qualité)
8. [Considérations éthiques](#considérations-éthiques)
9. [Références](#références)

---

## À propos du projet

Ce dépôt héberge le code, les scripts et la documentation associés à la constitution de la base de données **UqarMDD** (*UQAR Multimodal Drowsiness Dataset*). L'objectif est de développer un système de détection automatique de la somnolence en s'appuyant sur des données multimodales synchronisées :

- **Physiologiques** : EEG, ECG, EMG
- **Comportementales** : vidéo RGB et NIR (infrarouge proche)
- **Cognitives** : test PVT (Psychomotor Vigilance Task)
- **Subjectives** : échelle de somnolence de Karolinska (KSS)

Les applications visées incluent la sécurité routière, le transport aérien et ferroviaire, l'industrie et les environnements médicaux.

**Population :** 20 participants volontaires en bonne santé (20–35 ans).  !!!!
**Design :** étude expérimentale intra-sujets, 3 conditions — moment de la journée (matin ou fin d’après-midi) - lumière - son. !!!!

### Objectifs : !!!!

**Objectif principal :** Évaluer la capacité de différentes modalités (EEG, ECG, EMG, RGB, NIR, PVT et KSS) à détecter et prédire les états de somnolence. !!!!

**Objectifs secondaire :**
- Comparer les performances des modalités individuelles et multimodales.
- Étudier les corrélations entre KSS, performances PVT et signaux physiologiques.
- Constituer une base de données reproductible. !!!!

---

## Matériel et logiciels

### Matériel

| Modalité | Dispositif |
|---|---|
| EEG / ECG / EMG | BITalino (r)evolution Board BLE & BT Kit |
| Vidéo RGB + NIR | Microsoft Kinect V2 (30 fps) |

### Logiciels d'acquisition

| Logiciel | Version | Rôle |
|---|---|---|
| **OpenSignals** (PLUX) | — | Acquisition des signaux physiologiques (EEG, ECG, EMG) |
| **Brekel Body 2** | v2.63 64-bit | Acquisition vidéo RGB et NIR via Kinect V2 |
| **OpenSesame** | 4.0.24-py311-win64 | Administration du test PVT (tâche cognitive) |

### Environnement de post-traitement

- Python / MATLAB — synchronisation, prétraitement, extraction de caractéristiques

---

## Structure du dépôt

```
UqarMDD/
├── data/
│   ├── Participant_01/
│   │   ├── Session_Repos/
│   │   │   ├── EEG/
│   │   │   ├── ECG/
│   │   │   ├── EMG/
│   │   │   ├── RGB/
│   │   │   ├── NIR/
│   │   │   ├── Task/
│   │   │   └── Metadata/
│   │   └── Session_Fatigue/
│   │       └── ...
│   └── Participant_02/
│       └── ...
├── scripts/
│   ├── synchronisation/
│   ├── preprocessing/
│   └── annotation/
├── docs/
│   ├── Description_protocole_experimental.docx
│   └── Protocole_collecte_donnees_V1_0.docx
└── README.md
```

---

## Protocole expérimental

### Participants

- **N = 20** volontaires sains (cible : ~10 hommes / 10 femmes) !!!!
- Âge : 20–35 ans
- Deux sessions par participant :
  - **Session de référence** — Session effectuée dans une pièce éclairée (éclairage artificiel) et silencieuse.
  - **Session lumière modifiée** — Session effectuée dans une pièce à l'éclairage tamisé et silencieuse.
  - **Session son modifié** - Session effectuée dans une pièce éclairée (éclairage artificiel) et bruyante (son de radio/playlist personnel du participant) !!!!!
 
### Déroulement avant-session
- Remplir le questionnaire des habitudes !!!!!

### Déroulement d'une session !!!!

| Phase | Durée approx. | Description |
|---|---|---|
| 1 — Préparation | 15–25 min | Accueil, consentement, pose des électrodes EEG/ECG/EMG, calibration Kinect, mise en palce d'une des conditions (avec ou sans son, avec ou sans lumière tamisée) |
| 2 — Questionnaire pré-acquisition | ~5 min | Auto-évaluation subjective de la somnolence (KSS) |
| 3 — Ligne de base (optionnel) | 2–5 min | Repos assis, yeux ouverts puis fermés |
| 4 — Tâche PVT | ~10 min | Acquisition multimodale continue (physiologie + vidéo + task) |
| 5 - Tâche monotone | 30-35 min | Simulation d'une somnolence, auto-évaluation subjective de la somnolence (KSS) toutes les 10 min |
| 6 — KSS final (optionnel) | ~2 min | Ré-évaluation subjective post-tâche |
| Fin | — | Retrait des électrodes, sauvegarde, fiche de session |

### Tâche PVT (OpenSesame)

Un stimulus visuel (visage schématique orienté à droite ou à gauche) est présenté au centre de l'écran. Une lettre cible (`L` ou `R`) apparaît d'un côté, un distracteur (`X`) de l'autre. Le participant indique la lettre cible le plus rapidement possible.

- **Condition congruente** : le regard du visage pointe vers la cible → facilite la réponse
- **Condition incongruente** : le regard du visage pointe vers le distracteur → ralentit la réponse

**Variables cognitives collectées :** temps de réaction, lapses, omissions, commissions, précision, variabilité intra-session.

### Tâche monotone !!!!!

- Lecture d'une vidéo monotone d'une durée de 30 min afin de simuler une fatigue, une somnolence. La vidéo retenue est celle d'une voiture en pleine la circulation du point de vu d'un conducteur.
---

## Convention de nommage des fichiers

```
<ParticipantID>_<Session>_<Condition>_<Modalité>_<Timestamp>
```

**Exemple :**
```
P03_S2_Fatigue_EEG_2026-03-17_09-42-10
```

| Champ | Valeurs possibles |
|---|---|
| ParticipantID | P01, P02, … P15 |
| Session | S1, S2 |
| Condition | Repos, Fatigue |
| Modalité | EEG, ECG, EMG, RGB, NIR, Task, KSS |
| Timestamp | YYYY-MM-DD_HH-MM-SS |

---

## Synchronisation des modalités

La synchronisation temporelle est assurée par une **horloge maîtresse centrale** commune à toutes les modalités.

**Méthode :**

1. Des **marqueurs sonores (bips)** sont émis par Brekel Body 2 au début et à la fin de chaque enregistrement vidéo.
2. Ces bips servent de **points d'ancrage** (*anchor points*) pour le recalage post-acquisition de l'ensemble des flux (EEG/ECG/EMG, RGB/NIR, Task).
3. Après acquisition, les timestamps de chaque modalité sont extraits, le point d'ancrage est détecté dans chaque flux, et les échantillons antérieurs au point d'ancrage sont supprimés afin d'aligner tous les signaux sur une origine temporelle commune.

> **Responsable :** Abakar

---

## Critères de qualité

### Contrôle avant collecte
- Test de fonctionnement de chaque capteur
- Vérification de connectivité (Bluetooth BLE) et d'espace disque
- Validation du pipeline de sauvegarde

### Contrôle pendant la collecte
- Surveillance visuelle de la qualité des signaux physiologiques
- Surveillance des pertes de contact électrode-peau
- Surveillance des pertes d'images ou de dégradations vidéo

### Critères d'exclusion d'une session
Une session sera exclue ou répétée en cas de :
- Perte importante de signal physiologique
- Vidéo inexploitable
- Défaut majeur de synchronisation
- Non-respect des consignes pré-expérimentales
- Interruption prématurée de la tâche

---

## Considérations éthiques

- Le protocole est soumis à l'approbation d'un **comité d'éthique de la recherche** avant tout recrutement.
- Un **consentement libre et éclairé** est obtenu de chaque participant.
- Les données sont **pseudonymisées** (identifiant numérique) ; aucune information permettant l'identification directe n'est incluse dans ce dépôt.
- Les participants en condition fatigue **ne doivent pas conduire** après la session ; un accompagnement ou une solution de retour sécuritaire est prévu.
- Les vidéos et signaux physiologiques sont stockés sur des supports institutionnels sécurisés, avec accès restreint aux membres autorisés du projet.

---

## Références

- BITalino (r)evolution — [https://www.pluxbiosignals.com](https://www.pluxbiosignals.com)
- Brekel Body 2 — [https://uqar01-my.sharepoint.com/:u:/g/personal/adji0001_uqar_ca/IQCVck9iUjzES5pl2VYB5iyaAV5bgNhuB5V-JqR0eElgRFs?e=8o6Tcy]
- OpenSesame — [https://osdoc.cogsci.nl](https://osdoc.cogsci.nl)
- OpenSignals (PLUX) — [https://www.pluxbiosignals.com/opensignals](https://www.pluxbiosignals.com/opensignals)
- Karolinska Sleepiness Scale (KSS) — Åkerstedt & Gillberg (1990)

---

*Les auteurs reconnaissent avoir fait appel à des outils d'IA générative pour améliorer la clarté rédactionnelle de certains documents. Le contenu scientifique, les analyses et les conclusions relèvent entièrement de la responsabilité des auteurs.*
