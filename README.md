# ♟️ Robot Joueur d'Échecs — PST-19

<div align="center">

**Projet Scientifique et Technique 4A — ESIEA**
Semestre 2 · Année 2024–2025

---

*Un robot physique entièrement autonome qui joue aux échecs — du calcul du meilleur coup à la saisie de la pièce sur le plateau.*

</div>

---

## Le projet en une phrase

Un portique cartésien, piloté par une Raspberry Pi et le moteur Stockfish, déplace physiquement les pièces sur un échiquier réel. Le joueur interagit via une interface web accessible par QR Code depuis son smartphone.

---

## Démonstration

![IMG_7889](https://github.com/user-attachments/assets/85642f63-51ac-42aa-9c5a-23bdfc7e3b7f)

---

## Comment ça fonctionne

Le système repose sur trois blocs qui communiquent en permanence :

```
┌──────────────────┐        WebSocket        ┌──────────────────────────┐
│  Smartphone /    │ ◄─────────────────────► │  Raspberry Pi            │
│  Navigateur      │    Interface Next.js     │  Stockfish 16  →  G-Code │
└──────────────────┘                          └───────────┬──────────────┘
                                                          │ USB série
                                                          ▼
                                               ┌──────────────────────┐
                                               │  MKS Gen V1.4        │
                                               │  Firmware Marlin      │
                                               └──────┬──────────┬─────┘
                                                      │          │
                                               Moteurs X/Y    Servo (pince)
                                               (portique)     M280 P0 S168/S12
```

1. **Le joueur** entre son coup sur l'interface web (ou scanne le QR Code).
2. **Stockfish** calcule la meilleure réponse et la transmet au contrôleur.
3. **Le robot** exécute le coup : il survole la case, descend, saisit la pièce, la déplace, et la dépose.
4. Si une pièce est capturée, elle est déposée dans une zone de stockage latérale.

---

## Ce que nous avons construit

### La structure mécanique

Inspirée d'une imprimante 3D, la structure cartésienne XY repose sur des moteurs pas-à-pas NEMA 17. L'axe Z — conçu sur-mesure — intègre un système de crémaillère guidée par roulements à billes, modélisé sous Fusion 360. Le tout est fixé sur un socle en bois usiné et assemblé par l'équipe, qui dissimule l'électronique dans un double fond.

### L'interface web

L'interface a été migrée d'une application desktop Python/Pygame vers une application web moderne **Next.js + FastAPI**, hébergée sur la Raspberry Pi elle-même. Elle offre :

- 🎮 **Mode PvE** — affronter Stockfish à différents niveaux d'ELO (1 350 à 3 200)
- 🔗 **Mode PvP** — jouer à deux depuis le réseau local
- 📱 **Accès par QR Code** — aucune installation requise côté joueur
- 🤖 **Connexion robot** — contrôle du bras directement depuis le navigateur
- 🛠️ **Page admin** — gestion des sessions et de la file d'attente

### L'intelligence artificielle

Le moteur **Stockfish 16** est intégré via le protocole UCI. Le niveau de difficulté est configurable à la volée depuis l'interface. Chaque coup est validé par `python-chess` avant d'être transmis au robot.

---

## Équipe

| | Membre | Rôle principal |
|---|---|---|
| 👷 | **Matthieu Hamon** |  Impression 3D · Pince de préhension |
| 🖨️ | **Andréas Auffray** |  Conception mécanique · Assemblage · Validation des règles d'échecs |
| 💻 | **Romain Bouchez** | Développement logiciel · IA · Interface web · Contrôle robot |

**Encadrant :** M. Franck Crison

---

## Chronologie

| Période | Étape |
|---|---|
| Sept. – Oct. 2025 | Cadrage, choix de l'architecture, premiers essais UCI, commandes matériel |
| Nov. – Déc. 2025 | Assemblage PVC, configuration Marlin, premières impressions 3D |
| Janv. – Févr. 2026 | Migration vers Next.js, QR Code, nouveau guidage crémaillère |
| Mars – Avr. 2026 | Démontage, socle en bois, remontage, tests de cinématique finaux |

---

## Stack technique

| Domaine | Technologie |
|---|---|
| Interface web | Next.js 16 · React 19 · TypeScript · Tailwind CSS |
| Temps réel | Socket.IO (WebSocket) |
| Backend | FastAPI · Python 3.12 · uvicorn |
| Moteur d'échecs | Stockfish 16 · python-chess |
| Contrôle robot | pySerial · G-Code · Marlin 2.1.2.5 |
| Matériel | Raspberry Pi · MKS Gen V1.4 · NEMA 17 · Servo moteur |
| Modélisation | Fusion 360 · Impression 3D PLA/PETG |

---
## Git du code 

> Le code de l'interface web et du backend est disponible sur le dépôt complémentaire :
> **[github.com/RomainBouchez/chess-vs-stockfish](https://github.com/RomainBouchez/chess-vs-stockfish)**
---

<div align="center">
  <sub>ESIEA — Projet Scientifique et Technique 4A · 2024–2025 · PST-19</sub>
</div>
