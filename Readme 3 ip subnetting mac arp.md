# 🔢 Networking — Adresses IP, Subnetting, MAC & ARP

> Notes personnelles issues de Hack The Box — Module Networking Fundamentals.
> Tout le contenu est traduit en français, les termes techniques restent en anglais.

---

## 📚 Table des matières

- [1. Adresses IPv4](#1-adresses-ipv4)
- [2. Subnetting](#2-subnetting)
- [3. Adresses MAC & ARP](#3-adresses-mac--arp)
- [4. Adresses IPv6](#4-adresses-ipv6)

---

## 1. Adresses IPv4

### Concept clé : IP vs MAC

| Adresse | Analogie | Rôle |
|---------|----------|------|
| **IPv4/IPv6** | Adresse postale + quartier du bâtiment | Identifie le réseau et l'hôte sur Internet |
| **MAC** | Étage et appartement exact | Identifie l'interface physique sur le réseau local |

---

### Structure d'une adresse IPv4

- **32 bits** combinés en **4 octets** (groupes de 8 bits)
- Valeurs : `0` à `255` par octet
- Notation décimale séparée par des points

```
Binaire  : 0111 1111.0000 0000.0000 0000.0000 0001
Décimal  : 127.0.0.1
```

- Permet **4 294 967 296** adresses uniques
- Divisée en **partie réseau** (assignée par l'admin réseau/IANA) et **partie hôte** (assignée par le routeur)

---

### Classes d'adresses IPv4

| Classe | Adresse réseau | Première IP | Dernière IP | Masque | CIDR | Subnets | IPs |
|--------|---------------|-------------|-------------|--------|------|---------|-----|
| **A** | 1.0.0.0 | 1.0.0.1 | 127.255.255.255 | 255.0.0.0 | /8 | 127 | 16 777 214 + 2 |
| **B** | 128.0.0.0 | 128.0.0.1 | 191.255.255.255 | 255.255.0.0 | /16 | 16 384 | 65 534 + 2 |
| **C** | 192.0.0.0 | 192.0.0.1 | 223.255.255.255 | 255.255.255.0 | /24 | 2 097 152 | 254 + 2 |
| **D** | 224.0.0.0 | 224.0.0.1 | 239.255.255.255 | Multicast | Multicast | — | — |
| **E** | 240.0.0.0 | 240.0.0.1 | 255.255.255.255 | Réservé | Réservé | — | — |

> ⚠️ Les **+2** correspondent à l'adresse réseau et l'adresse broadcast (non assignables aux hôtes).

---

### Adresses réservées dans un subnet

| Adresse | Rôle |
|---------|------|
| **Adresse réseau** | Première adresse du subnet (bits hôte tous à 0) |
| **Broadcast** | Dernière adresse du subnet (bits hôte tous à 1) — envoie à TOUS les hôtes |
| **Passerelle par défaut** | Généralement la première ou dernière IP assignable |

---

### Conversion Binaire ↔ Décimal

Chaque octet = 8 bits, chaque position a une valeur :

```
Position : 128  64  32  16  8  4  2  1
```

Exemple avec `192.168.10.39` :

| Octet | Calcul | Résultat |
|-------|--------|----------|
| 1er | 128 + 64 + 0 + 0 + 0 + 0 + 0 + 0 | = **192** |
| 2ème | 128 + 0 + 32 + 0 + 8 + 0 + 0 + 0 | = **168** |
| 3ème | 0 + 0 + 0 + 0 + 8 + 0 + 2 + 0 | = **10** |
| 4ème | 0 + 0 + 32 + 0 + 0 + 4 + 2 + 1 | = **39** |

```
Binaire  : 1100 0000 . 1010 1000 . 0000 1010 . 0010 0111
Décimal  : 192       . 168       . 10        . 39
```

---

### CIDR (Classless Inter-Domain Routing)

Le **CIDR** remplace les classes fixes. Le **suffixe CIDR** indique combien de bits appartiennent à la partie réseau.

```
IP      : 192.168.10.39
Masque  : 255.255.255.0
CIDR    : 192.168.10.39/24
```

Calcul du suffixe : **somme de tous les bits à 1** dans le masque de sous-réseau.

```
Binaire masque : 1111 1111 . 1111 1111 . 1111 1111 . 0000 0000
                    /8          /16         /24         /32
→ /24 = 24 bits à 1
```

---

## 2. Subnetting

Le **subnetting** divise une plage d'adresses IPv4 en plusieurs plages plus petites.

> 💡 Analogie : un subnet = une porte vitrée séparant des départements dans un couloir.

### Informations calculables depuis un subnet

- Adresse réseau
- Adresse broadcast
- Premier hôte
- Dernier hôte
- Nombre d'hôtes

---

### Exemple complet : `192.168.12.160/26`

**Masque** : `255.255.255.192`

```
IP      : 1100 0000 . 1010 1000 . 0000 1100 . 10|10 0000
Masque  : 1111 1111 . 1111 1111 . 1111 1111 . 11|00 0000
                                              ^^
                                         séparation réseau/hôte
```

| Adresse | Valeur | Calcul |
|---------|--------|--------|
| **Réseau** | 192.168.12.128 | Bits hôte → tous à 0 |
| **Broadcast** | 192.168.12.191 | Bits hôte → tous à 1 |
| **Premier hôte** | 192.168.12.129 | Réseau + 1 |
| **Dernier hôte** | 192.168.12.190 | Broadcast - 1 |
| **Nombre d'hôtes** | 62 | 64 - 2 (réseau + broadcast) |

---

### Diviser en sous-réseaux plus petits

Exemple : diviser `192.168.12.128/26` en **4 sous-réseaux**

- 4 = 2² → on étend le masque de **2 bits** : `/26` → `/28`
- 64 IPs ÷ 4 = **16 IPs par subnet**

| Subnet | Adresse réseau | Premier hôte | Dernier hôte | Broadcast | CIDR |
|--------|---------------|-------------|--------------|-----------|------|
| 1 | 192.168.12.128 | 192.168.12.129 | 192.168.12.142 | 192.168.12.143 | /28 |
| 2 | 192.168.12.144 | 192.168.12.145 | 192.168.12.158 | 192.168.12.159 | /28 |
| 3 | 192.168.12.160 | 192.168.12.161 | 192.168.12.174 | 192.168.12.175 | /28 |
| 4 | 192.168.12.176 | 192.168.12.177 | 192.168.12.190 | 192.168.12.191 | /28 |

---

### Mental Subnetting (calcul rapide)

**Étape 1** — Identifier quel octet change :

| /8 | /16 | /24 | /32 |
|----|-----|-----|-----|
| 1er octet | 2ème octet | 3ème octet | 4ème octet |

**Étape 2** — Calculer la taille du subnet via le modulo :

```
Reste = CIDR % 8
Taille = 2^(8 - reste)
```

| Reste | Taille | Forme exponentielle |
|-------|--------|-------------------|
| 0 | 256 | 2^8 |
| 1 | 128 | 2^7 |
| 2 | 64 | 2^6 |
| 3 | 32 | 2^5 |
| 4 | 16 | 2^4 |
| 5 | 8 | 2^3 |
| 6 | 4 | 2^2 |
| 7 | 2 | 2^1 |

**Exemple** : `/25` → `25 % 8 = 1` → taille = `2^7 = 128`
- Plage 1 : `192.168.1.0 - 127` (réseau = .0, broadcast = .127, hôtes = .1 à .126)
- Plage 2 : `192.168.1.128 - 255` (réseau = .128, broadcast = .255, hôtes = .129 à .254)

---

## 3. Adresses MAC & ARP

### Structure d'une adresse MAC

- **48 bits** / 6 octets en **hexadécimal**
- Formats : `DE:AD:BE:EF:13:37` ou `DE-AD-BE-EF-13-37` ou `DEAD.BEEF.1337`

```
Représentation : 1er    2ème   3ème   4ème   5ème   6ème  (octet)
Binaire        : 1101 1110  1010 1101  1011 1110  1110 1111  0001 0011  0011 0111
Hex            : DE         AD         BE         EF         13         37
```

| Partie | Octets | Nom | Assigné par |
|--------|--------|-----|-------------|
| **OUI** (Organization Unique Identifier) | 1-3 | Identifiant fabricant | IEEE |
| **NIC** (Network Interface Controller) | 4-6 | Identifiant unique interface | Fabricant |

---

### Types d'adresses MAC

| Type | Dernier bit 1er octet | Exemple | Description |
|------|----------------------|---------|-------------|
| **Unicast** | 0 | `DE:AD:BE:EF:13:37` | Vers un seul hôte |
| **Multicast** | 1 | `01:00:5E:EF:13:37` | Vers plusieurs hôtes (qui l'acceptent) |
| **Broadcast** | 1 (tous bits à 1) | `FF:FF:FF:FF:FF:FF` | Vers TOUS les hôtes du réseau |

> 💡 L'avant-dernier bit du 1er octet identifie : **OUI global** (défini par IEEE) ou **adresse MAC administrée localement**.

---

### ARP — Address Resolution Protocol

**ARP** résout une adresse **IP (couche 3)** en adresse **MAC (couche 2)**.

#### Fonctionnement

1. **ARP Request** (broadcast) : "Qui a l'IP `10.129.12.101` ? Répondez à `10.129.12.100`"
2. **ARP Reply** (unicast) : "`10.129.12.101` est à `AA:AA:AA:AA:AA:AA`"

```shell
# Capture tshark d'un échange ARP
1   10.129.12.100 -> 10.129.12.255  ARP  Who has 10.129.12.101? Tell 10.129.12.100
2   10.129.12.101 -> 10.129.12.100  ARP  10.129.12.101 is at AA:AA:AA:AA:AA:AA
```

> Si l'hôte destination est dans un **autre subnet**, le frame Ethernet est adressé au MAC du **routeur (passerelle par défaut)**.

---

### Vecteurs d'attaque MAC

| Attaque | Description |
|---------|-------------|
| **MAC Spoofing** | Modifier son adresse MAC pour usurper celle d'un autre appareil |
| **MAC Flooding** | Envoyer des milliers de MACs différents au switch pour saturer sa table MAC |
| **MAC Filtering Bypass** | Usurper un MAC autorisé pour accéder au réseau |

---

### ARP Spoofing / ARP Poisoning

Aussi appelé **ARP Cache Poisoning** ou **ARP Poison Routing**.

**But** : associer notre adresse MAC à l'IP d'un appareil légitime → intercepter le trafic (MITM).

**Outils** : Ettercap, Cain & Abel

```shell
# Exemple d'ARP Poisoning
1  10.129.12.100 -> 10.129.12.101  ARP  10.129.12.255 is at CC:CC:CC:CC:CC:CC  ← (faux, c'est l'attaquant)
2  10.129.12.101 -> Broadcast       ARP  Who has 10.129.12.100?
3  10.129.12.100 -> 10.129.12.101  ARP  10.129.12.100 is at CC:CC:CC:CC:CC:CC
4  10.129.12.101 -> 10.129.12.100  ARP  Who has 10.129.12.255? ← victime cherche la gateway
```

**Protections** : IPSec, SSL, firewalls, IDS

---

## 4. Adresses IPv6

### Pourquoi IPv6 ?

IPv4 permet ~4,3 milliards d'adresses → épuisement. IPv6 permet **~340 undecillions** d'adresses.

### Avantages d'IPv6 sur IPv4

- Espace d'adressage bien plus grand
- **SLAAC** (auto-configuration d'adresse sans DHCP)
- Plusieurs adresses IPv6 par interface
- Routage plus rapide
- **IPsec obligatoire** (optionnel en IPv4)
- Paquets jusqu'à **4 Go**
- Pas de broadcast → remplacé par **multicast**

---

### Comparaison IPv4 vs IPv6

| Caractéristique | IPv4 | IPv6 |
|----------------|------|------|
| Longueur | 32 bits | 128 bits |
| Représentation | Décimale | Hexadécimale |
| Exemple | `10.10.10.0/24` | `fe80::dd80:b1a9:6687:2d3b/64` |
| Adressage dynamique | DHCP | SLAAC / DHCPv6 |
| IPsec | Optionnel | Obligatoire |
| Couche OSI | Réseau (L3) | Réseau (L3) |

---

### Structure d'une adresse IPv6

- **128 bits** = **8 blocs** de 16 bits
- Chaque bloc = **4 chiffres hexadécimaux**
- Séparés par **`:`** (au lieu de `.` en IPv4)

```
IPv6 complet  : fe80:0000:0000:0000:dd80:b1a9:6687:2d3b/64
IPv6 abrégé   : fe80::dd80:b1a9:6687:2d3b/64
```

| Partie | Nom | Description |
|--------|-----|-------------|
| Premiers bits | **Network Prefix** | Identifie le réseau/subnet |
| Derniers bits | **Interface Identifier** | Dérivé de l'adresse MAC 48 bits → étendue à 64 bits |

> Préfixe par défaut : `/64`. Autres courants : `/32`, `/48`, `/56`.

---

### Système hexadécimal

| Décimal | Hex | Binaire |
|---------|-----|---------|
| 0 | 0 | 0000 |
| 9 | 9 | 1001 |
| 10 | A | 1010 |
| 11 | B | 1011 |
| 12 | C | 1100 |
| 13 | D | 1101 |
| 14 | E | 1110 |
| 15 | F | 1111 |

Exemple IPv4 en hex : `192.168.12.160` → `C0.A8.0C.A0`

---

### Types d'adresses IPv6

| Type | Description |
|------|-------------|
| **Unicast** | Pour une seule interface |
| **Anycast** | Pour plusieurs interfaces, mais une seule reçoit le paquet |
| **Multicast** | Pour plusieurs interfaces, toutes reçoivent le paquet |

> ⚠️ Il n'y a **pas de broadcast** en IPv6 — remplacé par le multicast.

---

### Règles d'abréviation IPv6 (RFC 5952)

1. Tous les caractères alphabétiques en **minuscules**
2. Les **zéros en tête** de chaque bloc sont omis
3. Un ou plusieurs blocs de **4 zéros consécutifs** sont remplacés par `::`
4. Le `::` ne peut être utilisé **qu'une seule fois** (en partant de la gauche)

```
Complet   : fe80:0000:0000:0000:dd80:b1a9:6687:2d3b/64
Abrégé    : fe80::dd80:b1a9:6687:2d3b/64
```

---

### Dual Stack

IPv4 et IPv6 peuvent coexister simultanément sur un même réseau → **Dual Stack**.

---

*📝 Notes personnelles — Source : Hack The Box, Module Networking Fundamentals*
