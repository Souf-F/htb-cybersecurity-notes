# 🔗 Networking — Modèles OSI, TCP/IP & Proxies

> Notes personnelles issues de Hack The Box — Module Networking Fundamentals.
> Tout le contenu est traduit en français, les termes techniques restent en anglais.

---

## 📚 Table des matières

- [1. Proxies](#1-proxies)
- [2. Modèles Réseau OSI & TCP/IP](#2-modèles-réseau-osi--tcpip)
- [3. Le Modèle OSI en détail](#3-le-modèle-osi-en-détail)
- [4. Le Modèle TCP/IP en détail](#4-le-modèle-tcpip-en-détail)
- [5. Couche Réseau — Layer 3](#5-couche-réseau--layer-3)

---

## 1. Proxies

Un **proxy** est un appareil ou service qui se place **au milieu** d'une connexion et agit comme **médiateur**. La clé : il doit pouvoir **inspecter le contenu du trafic**. Sans cette capacité, c'est une **gateway**, pas un proxy.

> 💡 Les proxies opèrent presque toujours sur la **couche 7 du modèle OSI** (couche Application).

---

### Idée reçue courante

La plupart des gens pensent qu'un proxy = changer d'adresse IP. C'est en réalité un **VPN** qui fait ça. Un proxy fait bien plus : il inspecte et relaie le trafic.

---

### Les 3 types de proxies

#### Forward Proxy (Proxy Direct)
- Le client fait une requête → le proxy l'exécute pour lui
- Utilisé en entreprise pour **filtrer le trafic sortant** (web filter)
- Puissant contre les malwares : le malware doit bypasser le filtre ET être "proxy-aware"
- Exemple : **BurpSuite** (outil swiss-army pour HTTP)

> 💡 Firefox n'utilise pas WinSock (contrairement à IE/Edge/Chrome) mais **libcurl**, ce qui rend les malwares moins susceptibles de détecter le proxy Firefox.

#### Reverse Proxy
- Filtre le **trafic entrant** (inverse du Forward Proxy)
- Écoute sur une adresse et redirige vers un réseau fermé
- Exemples :
  - **Cloudflare** : protection DDoS + WAF
  - **ModSecurity** : WAF (Web Application Firewall)
- En pentest : configuré sur un endpoint infecté pour **bypasser les firewalls** et **éluder l'IDS**

#### Transparent Proxy
- Le client **ne sait pas** qu'il existe
- Intercepte les communications de manière transparente
- ↔️ **Non-transparent** : nécessite une configuration explicite côté client

---

## 2. Modèles Réseau OSI & TCP/IP

Deux modèles décrivent la communication réseau :

![Comparaison OSI vs TCP/IP](1773583368900_image.png)

![Comparaison OSI vs TCP/IP avec PDU](1773583376283_image.png)

---

### Comparaison OSI vs TCP/IP

| OSI | TCP/IP | PDU |
|-----|--------|-----|
| 7. Application (FTP, HTTP) | 4. Application | Data |
| 6. Présentation (JPG, SSL, TLS) | 4. Application | Data |
| 5. Session (NetBIOS) | 4. Application | Data |
| 4. Transport (TCP, UDP) | 3. Transport | Segment / Datagram |
| 3. Réseau (Router, L3 Switch) | 2. Internet | Packet |
| 2. Data-Link (Switch, Bridge) | 1. Link | Frame |
| 1. Physique (Carte réseau) | 1. Link | Bit |

---

### Transfert de paquets (Encapsulation)

![Packet Transfer](1773583353746_image.png)

- À l'**envoi** : les données descendent de la couche 7 → couche 1, chaque couche **ajoute un header** (encapsulation)
- À la **réception** : les données remontent de la couche 1 → couche 7, chaque couche **retire son header** (décapsulation)
- Ce processus s'appelle **PDU (Protocol Data Unit)**

```
Couche 7-5 → Data
Couche 4   → TCP Header + Data = Segment
Couche 3   → IP Header + TCP + Data = Packet
Couche 2   → MAC Header + IP + TCP + Data = Frame
Couche 1   → Bits (transmission binaire)
```

> 💡 En pentest : TCP/IP permet de comprendre rapidement une connexion ; OSI permet de la décomposer couche par couche pour l'analyser en détail.

---

## 3. Le Modèle OSI en détail

Le modèle **OSI** (Open Systems Interconnection) a été publié par l'**ITU** et l'**ISO**. Il comporte **7 couches** hiérarchiques.

![Modèle OSI détaillé](1773583418821_image.png)

| Couche | Nom | Fonction | Orientation |
|--------|-----|----------|-------------|
| **7** | Application | Contrôle entrées/sorties des données, fonctions applicatives | Application |
| **6** | Présentation | Convertit la représentation des données (indépendante du système) | Application |
| **5** | Session | Gère la connexion logique entre deux systèmes, évite les coupures | Application |
| **4** | Transport | Contrôle end-to-end, détection de congestion, segmentation | Transport |
| **3** | Réseau | Établit les connexions, forwarde les paquets, routage | Transport |
| **2** | Data Link | Transmission fiable et sans erreur sur le médium, divise en frames | Transport |
| **1** | Physique | Transmission via signaux électriques, optiques ou ondes EM | Transport |

- Couches **2-4** → orientées **transport**
- Couches **5-7** → orientées **application**

### Flux de communication
- **Envoi** : couche 7 → couche 1
- **Réception** : couche 1 → couche 7
- Les 7 couches sont traversées **au moins 2 fois** (une fois chez l'émetteur, une fois chez le récepteur)

---

## 4. Le Modèle TCP/IP en détail

**TCP/IP** = famille de protocoles (pas seulement TCP et IP). Comprend aussi ICMP, UDP, etc.

| Couche | Nom | Fonction |
|--------|-----|----------|
| **4** | Application | Accès aux services des autres couches, définit les protocoles d'échange |
| **3** | Transport | Services de session (TCP) et datagramme (UDP) |
| **2** | Internet | Adressage, packaging, routage |
| **1** | Link | Place/récupère les paquets TCP/IP sur le médium réseau |

### Tâches principales TCP/IP

| Tâche | Protocole | Description |
|-------|-----------|-------------|
| Adressage logique | IP | Structure la topologie réseau, classes, subnetting, CIDR |
| Routage | IP | Détermine le prochain nœud pour chaque paquet |
| Contrôle erreurs/flux | TCP | Messages de contrôle via connexion virtuelle |
| Support applicatif | TCP/UDP | Ports = abstraction logicielle pour distinguer les apps |
| Résolution de noms | DNS | Traduit les FQDN en adresses IP |

### OSI vs TCP/IP — Différences clés

| | OSI | TCP/IP |
|--|-----|--------|
| Couches | 7 | 4 |
| Rôle | Modèle de référence (plus détaillé) | Protocole de communication (plus flexible) |
| Règles | Strictes | Plus souples |
| Usage pentest | Analyse détaillée couche par couche | Compréhension rapide de la connexion |

---

## 5. Couche Réseau — Layer 3

La **couche réseau** (Layer 3 OSI) contrôle l'échange de paquets à travers le réseau.

### Fonctions principales
- **Logical Addressing** : identification des nœuds réseau
- **Routing** : construction et utilisation des tables de routage

### Protocoles de la couche 3

| Protocole | Description |
|-----------|-------------|
| `IPv4` / `IPv6` | Adressage IP principal |
| `IPsec` | Sécurité et chiffrement IP |
| `ICMP` | Messages de contrôle (ping, traceroute) |
| `IGMP` | Gestion des groupes multicast |
| `RIP` | Protocole de routage par vecteur de distance |
| `OSPF` | Protocole de routage par état de lien |

### Fonctionnement du routage
- Les paquets ne sont **pas toujours envoyés directement** au destinataire
- Ils passent de **nœud en nœud** (routeurs) jusqu'à destination
- Si les sous-réseaux source et destination sont **incompatibles**, les paquets sont redirigés via la **passerelle par défaut**
- Les paquets forwardés **ne remontent pas** aux couches supérieures dans les routeurs intermédiaires

---

*📝 Notes personnelles — Source : Hack The Box, Module Networking Fundamentals*
