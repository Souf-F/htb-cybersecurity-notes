# 🔒 Networking — VPN, VLAN, Cisco IOS & Cryptographie

> Notes personnelles issues de Hack The Box — Module Networking Fundamentals.
> Tout le contenu est traduit en français, les termes techniques restent en anglais.

---

## 📚 Table des matières

- [1. VPN — Virtual Private Network](#1-vpn--virtual-private-network)
- [2. Cisco IOS & VLANs](#2-cisco-ios--vlans)
- [3. Attaques VLAN](#3-attaques-vlan)
- [4. Protocoles d'Authentification](#4-protocoles-dauthentification)
- [5. Cryptographie](#5-cryptographie)

---

## 1. VPN — Virtual Private Network

Un **VPN** permet une connexion sécurisée et chiffrée entre un réseau privé et un appareil distant. L'appareil distant accède au réseau privé comme s'il y était physiquement connecté.

### Composants requis

| Composant | Description |
|-----------|-------------|
| **VPN Client** | Installé sur l'appareil distant (ex: OpenVPN client) |
| **VPN Server** | Accepte les connexions VPN et route le trafic |
| **Chiffrement** | AES, IPsec — protège les données en transit |
| **Authentification** | Secret partagé, certificat ou autre méthode |

### Ports VPN

| Protocole | Port | Usage |
|-----------|------|-------|
| PPTP | TCP/1723 | Ancien VPN (obsolète) |
| IKEv1 / IKEv2 | UDP/500 | Négociation de clés IPsec |
| ESP | UDP/4500 | Chiffrement trafic IPsec |
| IP | UDP/50-51 | Protocole de base IPsec |

---

### IPsec — Internet Protocol Security

Protocole de sécurité réseau qui fournit **chiffrement + authentification**.

#### Deux protocoles IPsec

| Protocole | Sigle | Fonction |
|-----------|-------|----------|
| **Authentication Header** | AH | Intégrité et authenticité des paquets IP — **pas de chiffrement** |
| **Encapsulating Security Payload** | ESP | Chiffrement + authentification optionnelle |

#### Deux modes IPsec

| Mode | Description | Usage |
|------|-------------|-------|
| **Transport** | Chiffre uniquement le payload (pas le header IP) | Communication end-to-end entre deux hôtes |
| **Tunnel** | Chiffre le paquet IP entier (header inclus) | Tunnel VPN entre deux réseaux |

---

### PPTP — Point-to-Point Tunneling Protocol

- Extension du protocole **PPP**
- Supporté par de nombreux OS
- ⚠️ **Obsolète depuis 2012** — vulnérable
- Méthode d'auth **MSCHAPv2** utilise le chiffrement **DES** (facilement crackable)
- Remplacé par : **L2TP/IPsec**, **IPsec/IKEv2**, **OpenVPN**

---

## 2. Cisco IOS & VLANs

### Cisco IOS

Système d'exploitation des équipements Cisco (routeurs, switches).

**Fonctionnalités principales :**
- Support IPv6
- QoS (Quality of Service)
- Sécurité (chiffrement, authentification)
- Virtualisation (VPLS, VRF)

**Protocoles supportés :**

| Type | Exemples |
|------|---------|
| Routage | OSPF, BGP |
| Switching | VTP, STP |
| Services réseau | DHCP |
| Sécurité | ACL (Access Control Lists) |

**Types de mots de passe Cisco IOS :**

| Type | Description |
|------|-------------|
| **User** | Connexion à l'appareil |
| **Enable Password** | Accès au mode "enable" (fonctions avancées) |
| **Secret** | Sécurise certains services (management distant) |
| **Enable Secret** | Mot de passe chiffré pour le mode enable |

```shell
# Connexion Telnet à un appareil Cisco
telnet 10.129.10.2
# Réponse typique Cisco IOS :
# User Access Verification
# Password:
```

---

### VLANs — Virtual Local Area Networks

Un **VLAN** est un regroupement logique d'endpoints connectés à des ports définis sur un switch, créant des **domaines de broadcast logiques**.

**Avantages :**
- Meilleure organisation (par département, fonction, équipe)
- Sécurité accrue (isolation du trafic)
- Administration simplifiée (indépendant de la localisation physique)
- Meilleures performances (réduction du trafic broadcast)

**Exemple de segmentation par département :**

| Département | VLAN ID | Subnet |
|-------------|---------|--------|
| Serveurs | VLAN 10 | 192.168.1.0/24 |
| Direction | VLAN 20 | 192.168.2.0/24 |
| Finance | VLAN 30 | 192.168.3.0/24 |
| RH | VLAN 40 | 192.168.4.0/24 |
| Marketing | VLAN 50 | 192.168.5.0/24 |
| Support | VLAN 60 | 192.168.6.0/24 |

**Plages d'IDs VLAN Cisco :**
- `1` : VLAN par défaut (ne pas modifier/supprimer)
- `2-1001` : VLANs normaux (sauvegardés dans `vlan.dat`)
- `1002-1005` : Réservés Token Ring / FDDI
- `1006-4094` : VLANs étendus (non sauvegardés)

---

### Types de ports VLAN

| Type | Description |
|------|-------------|
| **Access Port** | Appartient à **un seul VLAN** — trafic entrant assigné à ce VLAN |
| **Trunk Port** | Transporte **plusieurs VLANs** simultanément — relie deux switches ou switch/routeur |

---

### IEEE 802.1Q — Standard VLAN

Le standard 802.1Q ajoute un **header VLAN** dans la trame Ethernet :

![Structure trame 802.3 vs 802.1Q](1773583753010_image.png)

Structure du header 802.1Q :

| Champ | Taille | Description |
|-------|--------|-------------|
| **TPID** | 2 octets (0x8100) | Identifie la trame comme 802.1Q |
| **PCP** | 3 bits | Priorité (QoS) |
| **DEI** | 1 bit | Indicateur d'éligibilité au drop |
| **VID** | 12 bits | VLAN ID (0-4095, soit 4094 utilisables) |

---

### Assigner un VLAN sous Linux

```shell
# Charger le module 802.1Q
sudo modprobe 8021q

# Vérifier le chargement
lsmod | grep 8021

# Voir les interfaces disponibles
ip a

# Créer une interface VLAN (méthode moderne)
sudo ip link add link eth0 name eth0.20 type vlan id 20

# Assigner une IP à l'interface VLAN
sudo ip addr add 192.168.1.1/24 dev eth0.20

# Activer l'interface
sudo ip link set up eth0.20

# Vérifier
ip a | grep eth0.20
```

---

### Assigner un VLAN sous Windows (PowerShell)

```powershell
# Lister les adaptateurs réseau
Get-NetAdapter | Format-Table -AutoSize

# Voir le VLAN ID d'un adaptateur
Get-NetAdapterAdvancedProperty -DisplayName "vlan id"

# Assigner un VLAN ID
Set-NetAdapter -Name "Ethernet 2" -VlanID 10
```

**Via interface graphique :**

![Device Manager - Recherche](1773583764564_image.png)

![Device Manager - Propriétés](1773583769852_image.png)

![Device Manager - VLAN ID](1773583775258_image.png)

---

### Analyser le trafic VLAN avec Wireshark & tshark

```shell
# Filtrer tous les paquets VLAN dans Wireshark
vlan

# Filtrer les paquets d'un VLAN spécifique
vlan.id == 10

# Lister tous les VLAN IDs d'un fichier pcap
tshark -r "capture.pcapng" -T fields -e vlan.id | sort -n -u
```

![Wireshark - Filtre vlan](1773583782188_image.png)

![Wireshark - Filtre vlan.id == 10](1773583788070_image.png)

---

### CDP & STP

**CDP (Cisco Discovery Protocol)** — Layer 2, découverte des équipements Cisco directement connectés :

```shell
# Exemple de trafic CDP capturé
CDPv2, ttl: 180s
Device-ID: 'router.inlanefreight.loc'
IPv4: 10.129.100.1
Port-ID: 'Ethernet0/0'
Capability: Router
Version: 'Cisco IOS Software, C880 Software'
Platform: 'Cisco 881 (MPC8300) processor'
```

**STP (Spanning Tree Protocol)** — Évite les boucles dans les réseaux avec connexions redondantes :

```shell
# Exemple de trafic STP
STP 802.1w, Rapid STP, Flags [Learn, Forward]
bridge-id 8001.00:11:22:33:44:55.8000
root-id 8001.AA:AA:AA:AA:AA:AA
```

---

### VXLAN

**VXLAN** (Virtual eXtensible LAN) — Solution aux limitations des VLANs traditionnels :
- VLANs limités à 4094 → VXLAN supporte **16 millions de segments**
- Conçu pour les datacenters et environnements multi-tenant
- Identifiant : **VNI** (VXLAN Network Identifier) sur **24 bits**
- "Layer 2 overlay sur un réseau Layer 3"

---

## 3. Attaques VLAN

### VLAN Hopping

Permet à un attaquant d'accéder à des VLANs auxquels il ne devrait pas avoir accès, **sans routeur**.

**Mécanisme :** Exploite le protocole **DTP (Dynamic Trunking Protocol)** de Cisco qui négocie automatiquement les trunk links.

**Condition :** L'attaquant doit être connecté à un port switch avec DTP activé.

**Outil :** Yersinia

![Yersinia - VLAN Hopping DTP](1773583796929_image.png)

---

### Double-Tagging VLAN Hopping

Attaque plus sophistiquée : insère un **tag 802.1Q caché** à l'intérieur d'une trame déjà taguée.

**Condition :** L'attaquant doit être dans le même VLAN que le **native VLAN** du trunk port.

**Étapes :**
1. L'attaquant envoie une trame double-taguée (outer tag = native VLAN, inner tag = VLAN cible)
2. Le premier switch retire le outer tag (native VLAN) et forward
3. Le second switch lit le inner tag et forward vers le VLAN cible

**Outils :** Yersinia, Scapy

![Yersinia - Double Tagging](1773583805674_image.png)

---

## 4. Protocoles d'Authentification

| Protocole | Description |
|-----------|-------------|
| **Kerberos** | Basé sur tickets (KDC) — utilisé en environnement domaine |
| **SSL** | Protocole cryptographique pour communication sécurisée |
| **TLS** | Successeur de SSL — plus sécurisé |
| **OAuth** | Standard ouvert d'autorisation (accès tiers sans partager le mot de passe) |
| **OpenID** | Authentification décentralisée (un compte pour plusieurs sites) |
| **SAML** | Échange XML d'authentification/autorisation entre parties |
| **2FA** | Deux facteurs de vérification |
| **FIDO** | Standards ouverts pour authentification forte |
| **PKI** | Système clés publiques/privées + signatures digitales |
| **SSO** | Un seul jeu d'identifiants pour plusieurs applications |
| **MFA** | Plusieurs facteurs (mot de passe + téléphone + biométrie) |
| **PAP** | Envoie le mot de passe **en clair** ⚠️ |
| **CHAP** | Three-way handshake pour vérifier l'identité |
| **EAP** | Framework supportant plusieurs méthodes d'auth |
| **SSH** | Communication sécurisée client-serveur, accès distant |
| **HTTPS** | HTTP + SSL/TLS — communication chiffrée sur internet |
| **SRP** | Auth par mot de passe avec protection eavesdropping/MITM |

---

### LEAP vs PEAP (Authentification WiFi)

| | LEAP | PEAP |
|--|------|------|
| **Développeur** | Cisco | Standard industrie |
| **Chiffrement** | RC4 (faible) ⚠️ | TLS (fort) ✅ |
| **Vulnérabilités** | Attaques par dictionnaire | Moins vulnérable |
| **Statut** | Largement remplacé | Largement utilisé en entreprise |
| **Chiffre MSCHAPv2** | ❌ Non | ✅ Oui |

> 💡 Les deux sont en cours de remplacement par **EAP-TLS** (le plus sécurisé).

---

## 5. Cryptographie

### Chiffrement Symétrique vs Asymétrique

| | Symétrique | Asymétrique |
|--|-----------|-------------|
| **Clés** | 1 clé (même pour chiffrer/déchiffrer) | 2 clés (publique + privée) |
| **Exemples** | AES, DES, 3DES | RSA, PGP, ECC |
| **Usage** | Chiffrement de grandes quantités de données | Échanges sécurisés, signatures digitales |
| **Problème** | Distribution sécurisée de la clé | Plus lent |

---

### DES → 3DES → AES (Évolution)

| Algorithme | Longueur de clé | Statut |
|-----------|----------------|--------|
| **DES** | 56 bits | ❌ Obsolète (crackable) |
| **3DES** | 3 × 56 bits | ⚠️ Déprécié |
| **AES** | 128 / 192 / 256 bits | ✅ Standard actuel |

**AES (Advanced Encryption Standard) :**
- Plus rapide que DES (traitement de plusieurs blocs simultanément)
- Utilisé dans : WLAN 802.11i, IPsec, SSH, VoIP, PGP, OpenSSL

---

### Modes de chiffrement AES

| Mode | Sigle | Usage |
|------|-------|-------|
| **Electronic Code Book** | ECB | ⚠️ Déconseillé (vulnérable à l'analyse statistique) |
| **Cipher Block Chaining** | CBC | Chiffrement disque, e-mails, TLS/SSL, VeraCrypt |
| **Cipher Feedback** | CFB | Chiffrement temps réel, flux réseau, BitLocker |
| **Output Feedback** | OFB | Stream temps réel, PKCS, SSH |
| **Counter** | CTR | Stream temps réel, IPsec, BitLocker |
| **Galois/Counter** | GCM | Confidentialité + intégrité, WiFi, VPNs |

---

### Chiffrement Asymétrique — Clé publique/privée

- **Clé publique** → chiffre les données (accessible à tous)
- **Clé privée** → déchiffre les données (gardée secrète)

**Applications :**

| Application | Protocole |
|-------------|-----------|
| Signatures électroniques | RSA, ECC |
| Communication sécurisée web | SSL/TLS |
| VPN | IPsec |
| Accès distant sécurisé | SSH |
| Infrastructure de clés | PKI |
| Cloud | Divers |

---

*📝 Notes personnelles — Source : Hack The Box, Module Networking Fundamentals*
