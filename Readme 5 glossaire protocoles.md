# 📖 Networking — Glossaire des Protocoles Réseau

> Notes personnelles issues de Hack The Box — Module Networking Fundamentals.
> Référence rapide à consulter régulièrement et à enrichir au fil de l'apprentissage.

---

## 📚 Table des matières

- [Protocoles de Sécurité](#protocoles-de-sécurité)
- [Protocoles de Transfert & Communication](#protocoles-de-transfert--communication)
- [Protocoles Réseau & Routage](#protocoles-réseau--routage)
- [Protocoles WiFi & Authentification](#protocoles-wifi--authentification)
- [Protocoles VPN & Tunneling](#protocoles-vpn--tunneling)
- [Protocoles de Gestion & Supervision](#protocoles-de-gestion--supervision)
- [Protocoles Voix & Temps Réel](#protocoles-voix--temps-réel)
- [Protocoles Web & Développement](#protocoles-web--développement)
- [Protocoles Industriels & Spéciaux](#protocoles-industriels--spéciaux)

---

## Protocoles de Sécurité

| Protocole | Sigle | Description |
|-----------|-------|-------------|
| Wired Equivalent Privacy | **WEP** | Ancien protocole de sécurité WiFi — ⚠️ obsolète et vulnérable |
| Wi-Fi Protected Access | **WPA** | Protocole de sécurité WiFi avec mot de passe |
| Temporal Key Integrity Protocol | **TKIP** | Protocole de sécurité WiFi — moins sécurisé que WPA2 |
| Pretty Good Privacy | **PGP** | Chiffrement d'e-mails, fichiers et données |
| Internet Protocol Security | **IPsec** | Chiffrement et authentification des communications IP — utilisé dans les VPNs |
| Internet Key Exchange | **IKE** | Établit une connexion sécurisée entre deux ordinateurs (base Diffie-Hellman) |
| Extensible Authentication Protocol | **EAP** | Framework d'authentification supportant plusieurs méthodes |
| Lightweight EAP | **LEAP** | Auth WiFi Cisco — ⚠️ vulnérable aux attaques par dictionnaire |
| Protected EAP | **PEAP** | Tunnel sécurisé TLS pour auth WiFi et réseaux filaires |
| Terminal Access Controller Access-Control System | **TACACS** | Auth, autorisation et accounting centralisés pour accès réseau |
| Microsoft Baseline Security Analyzer | **MBSA** | Outil gratuit Microsoft pour détecter les vulnérabilités Windows |

---

## Protocoles de Transfert & Communication

| Protocole | Sigle | Description |
|-----------|-------|-------------|
| File Transfer Protocol | **FTP** | Transfert de fichiers entre systèmes |
| Secure Shell | **SSH** | Connexion sécurisée distante + exécution de commandes + transfert de fichiers |
| Simple Mail Transfer Protocol | **SMTP** | Envoi et réception d'e-mails |
| Hypertext Transfer Protocol | **HTTP** | Protocole client-serveur pour données web |
| Hypertext Transfer Protocol Secure | **HTTPS** | HTTP + SSL/TLS — chiffrement et authentification web |
| Server Message Block | **SMB** | Partage de fichiers, imprimantes et ressources réseau |
| Network File System | **NFS** | Accès à des fichiers sur un réseau |
| Network News Transfer Protocol | **NNTP** | Distribution et récupération de messages dans des newsgroups |
| Remote Shell | **RSH** | Exécution de commandes sur un ordinateur distant (Unix) |

---

## Protocoles Réseau & Routage

| Protocole | Sigle | Description |
|-----------|-------|-------------|
| Routing Information Protocol | **RIP** | Protocole de routage par vecteur de distance (LAN/WAN) |
| Open Shortest Path First | **OSPF** | Protocole de routage par état de lien (IGP, AS interne) |
| Interior Gateway Routing Protocol | **IGRP** | Protocole de routage Cisco propriétaire |
| Enhanced IGRP | **EIGRP** | Protocole de routage avancé par vecteur de distance |
| Spanning Tree Protocol | **STP** | Assure une topologie sans boucle sur les réseaux Ethernet Layer 2 |
| VLAN Trunking Protocol | **VTP** | Gère les VLANs sur plusieurs switches (Layer 2) |
| Virtual Local Area Network | **VLAN** | Segmentation logique d'un réseau en plusieurs domaines broadcast |
| Cisco Discovery Protocol | **CDP** | Découverte des équipements Cisco directement connectés |
| Hot Standby Router Protocol | **HSRP** | Redondance de routeur Cisco en cas de panne |
| Virtual Router Redundancy Protocol | **VRRP** | Assignation automatique de routeurs IP disponibles aux hôtes |
| Generic Routing Encapsulation | **GRE** | Encapsule les données dans un tunnel VPN |
| Network Address Translation | **NAT** | Permet à plusieurs appareils privés de partager une IP publique |

---

## Protocoles WiFi & Authentification

| Protocole | Sigle | Description |
|-----------|-------|-------------|
| Wi-Fi Protected Access | **WPA** | Sécurité WiFi avec mot de passe |
| Temporal Key Integrity Protocol | **TKIP** | Sécurité WiFi — obsolète |
| LEAP | **LEAP** | Auth WiFi Cisco — ⚠️ vulnérable |
| PEAP | **PEAP** | Tunnel TLS pour auth WiFi — recommandé |

---

## Protocoles VPN & Tunneling

| Protocole | Sigle | Description |
|-----------|-------|-------------|
| Virtual Private Network | **VPN** | Connexion sécurisée et chiffrée vers un réseau privé |
| Point-to-Point Tunneling Protocol | **PPTP** | Création de VPN — ⚠️ obsolète depuis 2012 |
| Internet Protocol Security | **IPsec** | Communication sécurisée et chiffrée — utilisé dans les VPNs |
| Internet Key Exchange | **IKE** | Établit des connexions sécurisées pour VPNs (Diffie-Hellman) |

---

## Protocoles de Gestion & Supervision

| Protocole | Sigle | Description |
|-----------|-------|-------------|
| Simple Network Management Protocol | **SNMP** | Gestion des équipements réseau |
| Network Time Protocol | **NTP** | Synchronisation de l'heure entre ordinateurs du réseau |
| Dynamic Host Configuration Protocol | **DHCP** | Attribution automatique d'adresses IP aux clients |
| Domain Name Service | **DNS** | Résolution de noms de domaine (FQDN → IP) |
| Systems Management Server | **SMS** | Solution de gestion des réseaux, systèmes et appareils mobiles |

---

## Protocoles Voix & Temps Réel

| Protocole | Sigle | Description |
|-----------|-------|-------------|
| Session Initiation Protocol | **SIP** | Établit et termine des sessions voix/vidéo/multimédia sur IP |
| Voice Over IP | **VoIP** | Appels téléphoniques via Internet |

---

## Protocoles Web & Développement

| Protocole | Sigle | Description |
|-----------|-------|-------------|
| Uniform Resource Identifier | **URI** | Syntaxe pour identifier une ressource sur Internet |
| Uniform Resource Locator | **URL** | Sous-ensemble d'URI — identifie une page web (protocole + domaine) |
| Asynchronous JavaScript and XML | **AJAX** | Technique web pour pages dynamiques (JS + XML/JSON) |
| Internet Server Application Programming Interface | **ISAPI** | Crée des extensions web hautes performances pour serveurs web |
| Carriage Return Line Feed | **CRLF** | Fin de ligne pour certains formats de fichiers texte |

---

## Protocoles Industriels & Spéciaux

| Protocole | Sigle | Description |
|-----------|-------|-------------|
| Supervisory Control and Data Acquisition | **SCADA** | Système de contrôle industriel (fabrication, énergie, eau, déchets) |

---

## 🔑 Protocoles à connaître absolument en Cybersécurité

| Priorité | Protocole | Pourquoi |
|----------|-----------|----------|
| ⭐⭐⭐ | **SSH** | Accès distant sécurisé — utilisé partout |
| ⭐⭐⭐ | **HTTPS/TLS** | Communication web sécurisée |
| ⭐⭐⭐ | **DNS** | Résolution de noms — vecteur d'attaque courant |
| ⭐⭐⭐ | **SMB** | Partage Windows — vulnérabilités célèbres (EternalBlue) |
| ⭐⭐⭐ | **HTTP** | Base du web — nombreuses vulnérabilités applicatives |
| ⭐⭐⭐ | **FTP** | Transfert de fichiers — souvent mal configuré |
| ⭐⭐⭐ | **SMTP** | E-mails — phishing, open relay |
| ⭐⭐ | **SNMP** | Gestion réseau — souvent mal sécurisé |
| ⭐⭐ | **RDP** | Accès distant Windows — nombreuses vulnérabilités |
| ⭐⭐ | **IPsec** | VPN sécurisé — important pour comprendre les tunnels |
| ⭐⭐ | **OSPF** | Routage — vulnérable aux man-in-the-middle (OSPF poisoning) |
| ⭐⭐ | **NTP** | Synchronisation temps — attaques par amplification DDoS |
| ⭐ | **VoIP/SIP** | Téléphonie — eavesdropping possible |
| ⭐ | **SCADA** | Systèmes industriels — cibles critiques |

---

*📝 Notes personnelles — Source : Hack The Box, Module Networking Fundamentals*
*💡 Liste incomplète — à enrichir au fil de l'apprentissage*
