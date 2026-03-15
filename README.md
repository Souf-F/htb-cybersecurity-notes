# 🛡️ HTB Cybersecurity Notes — Networking Fundamentals

> Notes personnelles de cybersécurité issues du module **Networking Fundamentals** de [Hack The Box](https://www.hackthebox.com).  
> Ces notes ont été rédigées en français dans un but d'apprentissage personnel et de référence rapide.

---

## 📌 À propos de ces notes

Ces notes couvrent les **fondamentaux du networking** en cybersécurité, tels qu'enseignés sur la plateforme **Hack The Box (HTB)** — l'une des références mondiales pour l'apprentissage du hacking éthique et de la cybersécurité offensive et défensive.

Le networking est la **base absolue** de la cybersécurité. Comprendre comment les données circulent, comment les réseaux sont construits et comment les protocoles fonctionnent est indispensable avant d'aborder n'importe quel sujet plus avancé comme le pentest, l'exploitation de vulnérabilités ou la forensique réseau.

> Ces notes sont volontairement **exhaustives** — rien n'a été supprimé. Le but est de garder une trace complète de tout ce qui a été appris, même les notions qui paraissent évidentes au départ.

---

## 📂 Structure des notes

```
networking/
├── README_1_Networking_Bases.md         — Introduction, types de réseaux, topologies
├── README_2_OSI_TCPIP_Proxies.md        — Modèles OSI & TCP/IP, proxies
├── README_3_IP_Subnetting_MAC_ARP.md    — Adresses IP, subnetting, MAC, ARP, IPv6
├── README_4_VPN_VLAN_Crypto.md          — VPN, VLAN, Cisco IOS, cryptographie
└── README_5_Glossaire_Protocoles.md     — Glossaire complet des protocoles réseau
```

---

## 📚 Notions abordées

### 🌐 README 1 — Networking Bases & Topologies
Introduction générale au networking et à son importance en cybersécurité.

| Notion | Description |
|--------|-------------|
| Qu'est-ce qu'un réseau | Définition, médiums, protocoles, topologies |
| Réseau plat vs segmenté | Pourquoi la segmentation est essentielle en sécurité |
| Architecture réseau sécurisée | DMZ, workstations, admin network, VoIP, imprimantes |
| WAN / LAN / WLAN / VPN | Types de réseaux et leurs usages |
| GAN / MAN / PAN / WPAN | Termes avancés pour les certifications |
| VPN Site-to-Site / Remote / SSL | Les 3 types de VPN et leurs différences |
| Les 8 topologies réseau | Point-to-Point, Bus, Star, Ring, Mesh, Tree, Hybrid, Daisy Chain |
| URL vs FQDN | Différences et analogie postale |
| Cas réels de pentest | Erreur de masque /25, attaque via imprimante piégée |

---

### 🔗 README 2 — Modèles OSI, TCP/IP & Proxies
Comprendre comment les données transitent sur un réseau, couche par couche.

| Notion | Description |
|--------|-------------|
| Forward Proxy | Filtre le trafic sortant — BurpSuite, web filter entreprise |
| Reverse Proxy | Filtre le trafic entrant — Cloudflare, ModSecurity (WAF) |
| Transparent / Non-transparent Proxy | Visible ou invisible pour le client |
| Modèle OSI (7 couches) | Physical, Data-Link, Network, Transport, Session, Presentation, Application |
| Modèle TCP/IP (4 couches) | Link, Internet, Transport, Application |
| PDU (Protocol Data Unit) | Bit, Frame, Packet, Segment, Data |
| Encapsulation / Décapsulation | Comment les headers s'ajoutent et se retirent couche par couche |
| Couche réseau Layer 3 | Routage, adressage logique, protocoles IPv4/IPv6/OSPF/RIP |

---

### 🔢 README 3 — Adresses IP, Subnetting, MAC & ARP
Le cœur de l'adressage réseau — indispensable pour comprendre comment les paquets arrivent à destination.

| Notion | Description |
|--------|-------------|
| Structure IPv4 | 32 bits, 4 octets, notation décimale pointée |
| Classes A, B, C, D, E | Plages d'adresses, masques, CIDR, nombre d'hôtes |
| Adresses réservées | Adresse réseau, broadcast, passerelle par défaut |
| Conversion binaire ↔ décimal | Calcul des octets bit par bit |
| CIDR | Notation /24, /25, /26 et calcul du masque |
| Subnetting | Diviser un réseau en sous-réseaux plus petits |
| Mental Subnetting | Technique de calcul rapide sans calculatrice |
| Adresses MAC | Structure 48 bits, OUI vs NIC, Unicast/Multicast/Broadcast |
| ARP | Résolution IP → MAC, ARP Request/Reply, capture tshark |
| Attaques MAC | MAC Spoofing, MAC Flooding, MAC Filtering Bypass |
| ARP Spoofing / Poisoning | Attaque MITM via Ettercap ou Cain & Abel |
| IPv6 | Structure 128 bits, types d'adresses, SLAAC, Dual Stack, règles RFC 5952 |

---

### 🔒 README 4 — VPN, VLAN, Cisco IOS & Cryptographie
Sécurisation avancée des réseaux, segmentation logique et bases de la cryptographie.

| Notion | Description |
|--------|-------------|
| VPN — composants | Client, serveur, chiffrement, authentification |
| IPsec | AH + ESP, modes Transport et Tunnel, ports UDP/500 et UDP/4500 |
| PPTP | Protocole obsolète depuis 2012, remplacé par IKEv2/OpenVPN |
| Cisco IOS | OS des équipements Cisco, types de mots de passe, protocoles supportés |
| VLANs | Segmentation logique, IDs 1-4094, avantages sécurité/performance |
| Ports Access vs Trunk | Différence et rôle dans la gestion des VLANs |
| IEEE 802.1Q | Standard VLAN, structure TPID/PCP/DEI/VID |
| VLAN sous Linux | `modprobe 8021q`, `ip link add`, `vconfig` |
| VLAN sous Windows | Device Manager, `Get-NetAdapter`, `Set-NetAdapter` |
| Analyse VLAN Wireshark | Filtres `vlan`, `vlan.id == 10`, `tshark` |
| VLAN Hopping | Exploitation DTP avec Yersinia |
| Double-Tagging | Attaque 802.1Q imbriqué avec Scapy/Yersinia |
| VXLAN | Solution scalable pour datacenters (16 millions de segments) |
| CDP / STP | Protocoles Cisco de découverte et prévention des boucles |
| Protocoles d'authentification | Kerberos, TLS, OAuth, SAML, MFA, PKI, SSH, HTTPS, LEAP, PEAP |
| Chiffrement symétrique | AES (128/192/256 bits), DES, 3DES |
| Chiffrement asymétrique | RSA, PGP, ECC — clé publique/privée |
| Modes de chiffrement AES | ECB, CBC, CFB, OFB, CTR, GCM |

---

### 📖 README 5 — Glossaire des Protocoles
Référence rapide de plus de 40 protocoles réseau classés par catégorie.

| Catégorie | Protocoles couverts |
|-----------|-------------------|
| Sécurité | WEP, WPA, TKIP, PGP, IPsec, IKE, EAP, LEAP, PEAP, TACACS |
| Transfert & Communication | FTP, SSH, SMTP, HTTP, HTTPS, SMB, NFS, RSH |
| Réseau & Routage | RIP, OSPF, IGRP, EIGRP, STP, VTP, VLAN, CDP, HSRP, VRRP, NAT, GRE |
| VPN & Tunneling | VPN, PPTP, IPsec, IKE |
| Gestion & Supervision | SNMP, NTP, DHCP, DNS, SMS |
| Voix & Temps Réel | SIP, VoIP |
| Web & Développement | URI, URL, AJAX, ISAPI, CRLF |
| Industriel | SCADA |

---

## 🎯 À quoi servent ces notes ?

Ces notes servent de **référence personnelle** pour :

- Préparer les **certifications** en cybersécurité (CEH, OSCP, CompTIA Security+, etc.)
- Comprendre les **attaques réseau** et leurs mécanismes (MITM, ARP Spoofing, VLAN Hopping, etc.)
- Maîtriser les **outils d'analyse réseau** (Wireshark, tshark, nmap, etc.)
- Réaliser des **tests d'intrusion** (pentest) en comprenant l'infrastructure cible
- Concevoir des **architectures réseau sécurisées** (segmentation, DMZ, VLANs, etc.)
- Lire et interpréter des **captures réseau** (pcap, logs, traces)

---

## 🧰 Outils mentionnés dans ces notes

| Outil | Usage |
|-------|-------|
| **Wireshark** | Analyse de captures réseau, filtres VLAN, ARP |
| **tshark** | Analyse en ligne de commande de fichiers pcap |
| **BurpSuite** | Proxy HTTP, interception de requêtes web |
| **Ettercap** | ARP Spoofing, attaques MITM |
| **Cain & Abel** | ARP Poisoning, récupération de credentials |
| **Yersinia** | Attaques VLAN Hopping, DTP, Double-Tagging |
| **Scapy** | Crafting de paquets personnalisés |
| **OpenVPN** | Client VPN utilisé par Hack The Box |
| **Suricata / Snort** | IDS — détection d'intrusions réseau |
| **ModSecurity** | WAF — Reverse Proxy applicatif |

---

## 📖 Source

Ces notes sont basées sur le module **"Introduction to Networking"** de la plateforme **Hack The Box Academy**.

> [Hack The Box Academy](https://academy.hackthebox.com) — Plateforme d'apprentissage de la cybersécurité offensive et défensive, reconnue mondialement par les professionnels du secteur.

---

## ⚠️ Avertissement légal

Ces notes sont rédigées à des fins **éducatives uniquement**.
Toute technique d'attaque mentionnée ne doit être pratiquée que dans un **environnement légal et autorisé** (labs HTB, machines virtuelles personnelles, environnements de test avec accord écrit).
L'auteur décline toute responsabilité en cas d'usage malveillant de ces informations.

---

*📝 Notes personnelles — En cours de mise à jour au fil de l'apprentissage*
*🔗 Source : [Hack The Box Academy](https://academy.hackthebox.com)*
