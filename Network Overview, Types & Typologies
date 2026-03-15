# 🌐 Networking — Bases, Types de Réseaux & Topologies

> Notes personnelles issues de Hack The Box — Module Networking Fundamentals.
> Tout le contenu est traduit en français, les termes techniques restent en anglais.

---

## 📚 Table des matières

- [1. Introduction au Networking](#1-introduction-au-networking)
- [2. Types de Réseaux](#2-types-de-réseaux)
- [3. Topologies Réseau](#3-topologies-réseau)

---

## 1. Introduction au Networking

Un réseau permet à deux ordinateurs de communiquer entre eux. Il existe une grande variété de **topologies** (mesh/tree/star), de **médiums** (ethernet/fibre/coaxial/sans-fil) et de **protocoles** (TCP/UDP/IPX).

> 💡 En tant que professionnel de la sécurité, comprendre le réseau est essentiel : quand un réseau tombe en panne, l'erreur peut être silencieuse et nous faire rater quelque chose.

---

### Réseau plat vs Réseau segmenté

Un **réseau plat** (flat network) est simple à mettre en place mais peu sécurisé — comme une maison avec seulement un verrou sur la porte.

En créant des **réseaux plus petits** qui communiquent entre eux, on ajoute des **couches de défense** :

| Analogie | Sécurité réseau |
|----------|----------------|
| Clôture avec points d'entrée/sortie | ACL (Access Control Lists) autour des sous-réseaux |
| Lumières autour de la propriété | Documentation et cartographie des réseaux |
| Buissons devant les fenêtres | IDS comme Suricata ou Snort |

---

### Architecture réseau recommandée (entreprise)

Un réseau d'entreprise bien conçu devrait idéalement comporter **5 réseaux séparés** :

| Zone | Réseau séparé | Raison |
|------|--------------|--------|
| **Serveur Web** | ✅ DMZ (Zone Démilitarisée) | Les clients internet peuvent initier des connexions |
| **Postes de travail** | ✅ Réseau dédié | Évite le spoofing et les attaques MITM |
| **Switch/Routeur** | ✅ Réseau d'administration | Empêche les workstations d'espionner les communications |
| **Téléphones IP** | ✅ Réseau dédié | Sécurité + priorité QoS pour éviter la latence |
| **Imprimantes** | ✅ Réseau dédié | Les imprimantes sont quasi impossibles à sécuriser |

> ⚠️ **Anecdote réelle (pentest) :** Une imprimante compromise a été expédiée dans une entreprise avec un reverse shell intégré. Dès qu'elle a été branchée, l'administrateur de domaine lui a envoyé ses identifiants (NTLMv2). Si le réseau avait été correctement segmenté, cette attaque n'aurait pas fonctionné.

---

### Schéma réseau Home vs Company

![Schéma réseau Home vs Company](1773583197087_image.png)

> Le schéma illustre deux réseaux connectés via un **ISP** (Fournisseur d'accès Internet) :
> - **Home Network** : Router → Smartphone, Notebook, PC
> - **Company Network** : Router → Switch → Webserver, IP Phone, Printer, Client Hosts
> - Le **DNS** (Domain Name Server) est externe, accessible via Internet

---

### URL vs FQDN

| Type | Exemple | Description |
|------|---------|-------------|
| **FQDN** | `www.hackthebox.eu` | Identifie l'adresse du "bâtiment" |
| **URL** | `https://www.hackthebox.eu/example?floor=2&office=dev` | Identifie aussi "l'étage", "le bureau" et "l'employé" |

**Analogie postale :**
- Notre **routeur** = notre bureau de poste local
- L'**ISP** = le bureau de poste principal
- Le **DNS** = le registre d'adresses / annuaire téléphonique (traduit les noms en adresses IP)

---

### Erreur classique de Pentester (histoire vraie)

La plupart des réseaux utilisent un sous-réseau `/24`, à tel point que beaucoup de pentesteurs configurent ce masque sans vérifier. Exemple d'erreur :

```
Gateway Serveur : 10.20.0.1/25
Contrôleur de Domaine : 10.20.0.10/25
Gateway Client : 10.20.0.129/25
Workstation Client : 10.20.0.200/25
IP Pentester : 10.20.0.252/24 ← ERREUR (mauvais masque)
```

Le pentesteur pensait que le Contrôleur de Domaine était hors-ligne alors qu'il était simplement sur un autre réseau (`/25` divise le `/24` en deux moitiés).

---

## 2. Types de Réseaux

### Termes courants

| Type | Définition |
|------|-----------|
| **WAN** (Wide Area Network) | Internet — grand réseau reliant plusieurs LAN |
| **LAN** (Local Area Network) | Réseau interne (maison, bureau) |
| **WLAN** (Wireless LAN) | Réseau interne accessible en Wi-Fi |
| **VPN** (Virtual Private Network) | Connecte plusieurs sites réseau en un seul LAN |

---

### WAN — Wide Area Network

- Communément appelé **Internet**
- Un WAN est un grand nombre de LANs connectés ensemble
- Les grandes entreprises peuvent avoir un **WAN interne** (aussi appelé Intranet, Airgap Network)
- Identifiable via le protocole de routage **BGP** et des IPs hors RFC 1918

---

### LAN / WLAN

- Utilisent des adresses IP **privées** (RFC 1918) :
  - `10.0.0.0/8`
  - `172.16.0.0/12`
  - `192.168.0.0/16`
- Différence LAN vs WLAN : uniquement le support (câble vs sans-fil)

---

### VPN — Virtual Private Network

Il existe **3 types principaux** de VPN :

#### Site-to-Site VPN
- Client et serveur sont des **équipements réseau** (routeurs/firewalls)
- Relie des réseaux d'entreprise entre eux via Internet
- Permet à plusieurs sites distants de communiquer comme s'ils étaient locaux

#### Remote Access VPN
- Crée une **interface virtuelle** sur l'ordinateur du client
- Hack The Box utilise **OpenVPN** (adaptateur TUN)
- **Split-Tunnel VPN** : seul le trafic vers le réseau cible passe par le VPN (le reste passe par Internet directement)
  - ✅ Bien pour HTB (accès lab sans surveiller internet)
  - ❌ Pas idéal en entreprise (malware peut passer inaperçu)

#### SSL VPN
- VPN dans le **navigateur web**
- Exemple : HackTheBox Pwnbox
- Stream d'applications ou sessions desktop dans le navigateur

---

### Termes "théoriques" (utiles pour les certifications)

| Type | Définition |
|------|-----------|
| **GAN** (Global Area Network) | Internet mondial + réseaux câbles sous-marins/satellites |
| **MAN** (Metropolitan Area Network) | Réseau régional connectant plusieurs LANs (fibre haute perf) |
| **PAN** (Personal Area Network) | Réseau personnel (câble USB) |
| **WPAN** (Wireless PAN) | Réseau personnel sans fil (Bluetooth, Piconet) |

> 💡 Les protocoles **Insteon**, **Z-Wave** et **ZigBee** ont été conçus spécifiquement pour les maisons intelligentes (IoT/WPAN).

---

## 3. Topologies Réseau

Une **topologie réseau** est l'arrangement physique ou logique des appareils dans un réseau.

### Types de connexions

| Filaires | Sans-fil |
|---------|----------|
| Câble coaxial | Wi-Fi |
| Fibre optique | Cellulaire |
| Paire torsadée | Satellite |

### Nœuds réseau (Network Nodes)

Répéteurs, Hubs, Bridges, **Switches**, **Routeurs/Modems**, Gateways, **Firewalls**

---

### Les 8 topologies de base

#### 1. Point-to-Point (Point à Point)
- Connexion **directe** entre deux hôtes uniquement
- Base de la téléphonie traditionnelle
- ⚠️ Ne pas confondre avec P2P (Peer-to-Peer)

#### 2. Bus
- Tous les hôtes connectés sur **un même médium** de transmission
- Un seul hôte peut envoyer à la fois
- Médium typique : câble coaxial

![Topologie Bus](1773583317246_image.png)

---

#### 3. Star (Étoile)
- Tous les hôtes connectés à **un composant central** (routeur, hub ou switch)
- Le composant central gère tout le transfert de données
- Trafic central peut devenir très élevé

![Topologie Star](1773583311021_image.png)

---

#### 4. Ring (Anneau)
- Chaque hôte connecté avec **2 câbles** (un entrant, un sortant)
- Transmission dans **une direction prédéfinie**
- Utilise un **token** (jeton) qui circule en permanence
- Topologie logique en anneau peut être basée sur une topologie physique en étoile

![Topologie Ring](1773583304945_image.png)

---

#### 5. Mesh (Maillage)

Deux structures :
- **Full Mesh** : chaque hôte connecté à **tous les autres** → haute fiabilité (utilisé en WAN/MAN)
- **Partial Mesh** : certains nœuds connectés à plusieurs, d'autres à un seul

> Si un routeur tombe, les autres continuent de fonctionner grâce aux multiples connexions.

![Topologie Mesh](1773583288574_image.png)

---

#### 6. Tree (Arbre)
- Extension de la topologie étoile
- Utilisé dans les **grands bâtiments d'entreprise**
- Utilisé pour les réseaux haut débit et réseaux urbains (MAN)

![Topologie Tree](1773583294005_image.png)

---

#### 7. Hybrid (Hybride)
- Combinaison de **deux topologies ou plus**
- Exemple : réseaux en étoile connectés via des bus → topologie hybride

![Topologie Hybrid](1773583282451_image.png)

---

#### 8. Daisy Chain
- Hôtes connectés **en série** (un après l'autre)
- Chaque signal passe par les nœuds précédents
- Souvent utilisé en **technologie d'automatisation (CAN)**

![Topologie Daisy Chain](1773583299460_image.png)

---

### Récapitulatif des topologies

| Topologie | Avantage | Inconvénient | Usage |
|-----------|----------|--------------|-------|
| Point-to-Point | Simple | Limité à 2 hôtes | Téléphonie |
| Bus | Simple | Un seul émetteur à la fois | Ancien LAN |
| Star | Centralisation facile | Point de défaillance unique | LAN moderne |
| Ring | Pas de composant central requis | Lent, sens unique | Anciens réseaux |
| Mesh | Haute redondance | Coûteux | WAN/MAN |
| Tree | Scalable | Complexe | Grandes entreprises |
| Hybrid | Flexible | Complexe à gérer | Réseaux mixtes |
| Daisy Chain | Simple à câbler | Latence si longue chaîne | Automatisation |

---

*📝 Notes personnelles — Source : Hack The Box, Module Networking Fundamentals*
