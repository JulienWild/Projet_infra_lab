# Projet_infra_lab
Projet de déploiement d'un laboratoire pour les élèves Wilder.


[![forthebadge](https://forthebadge.com/images/badges/made-with-markdown.svg)](https://forthebadge.com) 

---


<h2 align="center">
🅆🄴🄻🄲🄾🄼🄴
</h2>



## Table of contents

- [Présentation projet](#Présentation projet)
- 


# Présentation projet


# Projet Fil Rouge : Laboratoire Virtuel pour la Formation AIS

## Table des matières
1. [Contexte](#contexte)
2. [Besoins](#besoins)
3. [L'existant](#lexistant)
4. [Problématique](#problématique)
5. [Contraintes](#contraintes)
6. [Solutions étudiées](#solutions-étudiées)
    - [Avantages et inconvénients](#avantages-et-inconvénients)
7. [Solution retenue](#solution-retenue)
8. [Architecture du laboratoire](#architecture-du-laboratoire)
9. [Note pour les lecteurs](#note-pour-les-lecteurs)

---

## Contexte
La Wild Code School propose une formation en alternance qui prépare à la certification RNCP de niveau 6 : **Administrateur d’Infrastructures Sécurisées (AIS)**, reconnue par l'État sous le numéro [RNCP31113](https://certifpro.francecompetences.fr/api/fiches/refActivity/24214/465506). 

Cette formation est dispensée en distanciel et en alternance (contrat de professionnalisation et contrat d’apprentissage). 

En tant que formateur en infrastructures et cybersécurité, je suis responsable de :
- L’animation pédagogique des cours et ateliers.
- La rédaction des supports d’apprentissage.
- Le suivi pédagogique des élèves, appelés **Wilders**.

Lors de leur certification, les Wilders doivent présenter un ou plusieurs projets couvrant les compétences exigées par le [REAC](https://certifpro.francecompetences.fr/api/fiches/refActivity/24214/465506). Ces projets prennent la forme de présentations, de dossiers professionnels et de projets concrets. Dans le cas où leur expérience en entreprise ne couvre pas toutes les compétences, des projets complémentaires réalisés en formation sont acceptés.

---

## Besoins
Pour s’assurer que chaque Wilder puisse valider l’ensemble des compétences nécessaires, un **projet fil rouge** a été conçu. 

Ce projet repose sur le cas fictif d’une entreprise, **EcoSolar Solutions**, et consiste à :
- Administrer une infrastructure sécurisée selon les bonnes pratiques.
- Identifier et répondre aux besoins techniques et organisationnels.
- Concevoir des solutions innovantes alignées sur la stratégie d’entreprise.
- Garantir un haut niveau de sécurité.

Cependant, travailler en distanciel impose une contrainte majeure : il n’est pas possible de déployer physiquement l’infrastructure de cette entreprise fictive. Une solution d’hébergement doit être mise en place pour permettre aux Wilders de simuler et faire évoluer cette infrastructure.

---

## L'existant
Actuellement, nous disposons d’un hyperviseur **Proxmox VE**, hébergé chez Hetzner, administré par @Aurélien DAZY, formateur DevOps (Merci Aurélien 😜) . 

Cet environnement permet aux Wilders de déployer des machines virtuelles pour tester leurs solutions. 
Cependant, il reste insuffisant pour couvrir les besoins suivants :
- L’unification du déploiement des machines virtuelles et des environnements réseau (commutateurs, routeurs, etc.).
- L’étude des interactions complexes entre ces composants dans un environnement simulé.

---

## Problématique
Nous avons besoin d’une solution complète permettant de simuler l’infrastructure d’EcoSolar Solutions dans un environnement virtuel tout en répondant aux contraintes pédagogiques et techniques.

---

## Contraintes
L’environnement de laboratoire doit :
- **Permettre un travail collaboratif sécurisé** avec une gestion des droits d’accès.
- **Simuler divers équipements** :
  - Machines réseau (commutateurs, routeurs, etc.).
  - Serveurs (Linux, Windows Server, clients Windows).
  - Hyperviseurs (Proxmox, ESXi, etc.).
- **Inclure des fonctionnalités avancées** :
  - Spanning Tree, VPN, firewalling, proxy/reverse proxy.
  - Active Directory, DNS, DMZ, stockage distant.
  - Clustering d’hyperviseurs, PRA/PCA, etc.
- **Supporter IPv4 et IPv6**.
- **Être open source** et facile à maintenir, reproduire et faire évoluer.
- **S’intégrer à l’hyperviseur Proxmox** existant.
- Proposer une **interface graphique intuitive** pour éviter une abstraction excessive dans les interactions.

---

## Solutions étudiées
Plusieurs solutions open source ont été envisagées :
- **GNS3**
- **EVE-NG**
- **PnetLab** (fork d’EVE-NG)
- **Cisco CML**

### Avantages et inconvénients
| Solution   | Limites                 | Fiabilité   | Ressources nécessaires |
|------------|-------------------------|-------------|-------------------------|
| **GNS3**   | Configuration complexe  | Moyenne     | Modérée                |
| **EVE-NG** | Licence payante (PRO)   | Élevée      | Modérées à élevées     |
| **PnetLab**| Open source limité      | Moyenne     | Modérée                |
| **Cisco CML** | Licence coûteuse     | Très élevée | Élevées                |

---

## Solution retenue
La solution **PnetLab** a été choisie pour répondre à nos besoins. Elle sera déployée comme une machine virtuelle sur l’hyperviseur Proxmox.

---

## Architecture du laboratoire
- **Laboratoire virtuel déployé derrière un firewall pfSense**.
- Accès sécurisé via **VPN host-to-site WireGuard**.
- Environnement de simulation unifié avec PnetLab, intégrant :
  - Machines réseau et serveurs divers.
  - Outils avancés pour les compétences AIS (spanning tree, firewall, PRA, etc.).


![[Architecture_Laboratoire.png]]



---

## Étapes du projet
Les étapes suivantes sont prévues pour le déploiement de la solution, chacune représentée par un dossier dédié :

### 1. Préparation Proxmox
- **1.1. Contexte**
- **1.2. Contraintes**
- **1.3. Stockage local**
- **1.4. Réseau**
- **1.5. Règles de sécurité et routage**

### 2. Préparation pfSense
- **2.1. Adressage IPv4 et IPv6**
- **2.2. Service DHCP et DHCPv6**
- **2.3. Règles de sécurité**

### 3. VPN
- **3.1. Serveur VPN WireGuard - coté pfSense**
	- Installation du paquet WireGuard
	- Configuration du serveur
	- Création des règles de sécurité
- **3.2. Client VPN WireGuard - Automatisation du processus**
* **3.3. Configuration peers sur le serveur**
### 4. PnetLab
- **4.1. Présentation PnetLab**
- **4.2. Contraintes**
- **4.3. Phases de déploiement**
- **4.4. Déploiement des modèles de machines**
- **4.5. Gestion des droits d’accès**

---

## Note pour les lecteurs
Ce projet vise à offrir une base de travail pour quiconque souhaitant déployer un laboratoire virtuel répondant à des problématiques similaires. 

Bien que cette solution soit adaptée à nos besoins pédagogiques, elle n’a pas vocation à être universelle. Certains aspects pourraient être optimisés ou automatisés, et des choix alternatifs sont toujours envisageables.

Vos retours et suggestions sont les bienvenus à cette adresse : julien.gregoire@wildcodeschool.com.



