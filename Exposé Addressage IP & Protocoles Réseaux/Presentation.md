# Exposé : Adressage IP et Protocoles Réseaux

**Matière :** Fondamentaux des Réseaux et Systèmes  
**Niveau :** IA / Première Année de Licence Informatique  
**Professeur :** [KEKE Mahuvivi Turibio](https://github.com/turibiok)
---
***[ESCAE-BENIN](https://escaebenin.com/) - Site de Tankpè***

---

## Table des Matières

1. Introduction
2. Adressage IP
   - 2.1 Qu'est-ce qu'une adresse IP ?
   - 2.2 IPv4
   - 2.3 Masques de sous-réseau et CIDR
   - 2.4 Classes d'adresses IP
   - 2.5 Adresses privées et publiques
   - 2.6 IPv6
   - 2.7 NAT – Network Address Translation
3. Les Protocoles Réseaux
   - 3.1 Le modèle OSI
   - 3.2 Le modèle TCP/IP
   - 3.3 Protocoles de la couche Réseau
   - 3.4 Protocoles de la couche Transport
   - 3.5 Protocoles de la couche Application
   - 3.6 Protocoles de sécurité
4. Conclusion
5. Glossaire

---

## 1. Introduction

Les réseaux informatiques constituent aujourd'hui l'épine dorsale des systèmes d'information modernes. Qu'il s'agisse d'Internet, d'un réseau d'entreprise ou d'un simple réseau domestique, leur bon fonctionnement repose sur deux piliers fondamentaux :

- **L'adressage IP**, qui permet d'identifier et de localiser chaque machine de manière unique sur un réseau.
- **Les protocoles réseaux**, qui définissent les règles de communication entre les équipements.

Cet exposé présente de façon structurée ces deux notions essentielles, en couvrant aussi bien les concepts théoriques que les aspects pratiques.

---

## 2. Adressage IP

### 2.1 Qu'est-ce qu'une adresse IP ?

Une **adresse IP** (Internet Protocol) est un identifiant numérique unique attribué à chaque interface réseau d'un équipement (ordinateur, serveur, smartphone, routeur, etc.) connecté à un réseau.

Elle remplit deux fonctions principales :

| Fonction | Description |
|----------|-------------|
| **Identification** | Elle identifie de manière unique un hôte sur le réseau |
| **Localisation** | Elle permet de déterminer où se trouve cet hôte (dans quel réseau) |

Il existe deux versions principales du protocole IP : **IPv4** (la plus répandue) et **IPv6** (le successeur moderne).

---

### 2.2 IPv4

#### Structure d'une adresse IPv4

Une adresse **IPv4** est composée de **32 bits**, représentés sous la forme de **4 octets** séparés par des points (notation décimale pointée).

```
Exemple :  192.168.1.10
           │   │   │  │
          192 168   1  10  ← valeurs décimales (0 à 255)
```

Chaque octet peut prendre une valeur de **0 à 255**, soit 2⁸ = 256 valeurs possibles.  
Le nombre total d'adresses IPv4 est de **2³² soit environ ≈ 4,3 milliards**.

#### Représentation binaire

```
192       .  168       .  1         .  10
11000000     10101000     00000001     00001010
```

---

### 2.3 Masques de sous-réseau et CIDR

#### Masque de sous-réseau

Le **masque de sous-réseau** est une valeur de 32 bits qui permet de distinguer la **partie réseau** et la **partie hôte** d'une adresse IP.

```
Adresse IP  :  192.168.1.10   →  11000000.10101000.00000001.00001010
Masque      :  255.255.255.0  →  11111111.11111111.11111111.00000000
                                 |_________________________|_________|
                                       Partie réseau       Partie hôte
```

- **Partie réseau** : bits à `1` dans le masque → identifie le réseau
- **Partie hôte** : bits à `0` dans le masque → identifie la machine dans le réseau

#### Notation CIDR

La notation **CIDR** (Classless Inter-Domain Routing) est une notation compacte qui indique le nombre de bits à `1` dans le masque.

```
192.168.1.0/24   ←  /24 signifie que les 24 premiers bits sont le réseau
                     Équivalent à un masque de 255.255.255.0
```

**Exemples courants :**

| Notation CIDR | Masque          | Nb d'hôtes max | Usage typique         |
|---------------|-----------------|----------------|-----------------------|
| /8            | 255.0.0.0       | 16 777 214     | Très grands réseaux   |
| /16           | 255.255.0.0     | 65 534         | Grands réseaux        |
| /24           | 255.255.255.0   | 254            | Réseaux locaux (LAN)  |
| /30           | 255.255.255.252 | 2              | Liaisons point à point|

> **Remarque :** Dans chaque sous-réseau, deux adresses sont réservées :  
> - **Adresse réseau** (hôte tout à 0) : ex. 192.168.1.**0**  
> - **Adresse de broadcast** (hôte tout à 1) : ex. 192.168.1.**255**

---

### 2.4 Classes d'adresses IP

Avant l'adoption du CIDR, les adresses IPv4 étaient divisées en **classes** :

| Classe | Plage (1er octet) | Masque par défaut | Nb de réseaux | Nb d'hôtes/réseau |
|--------|-------------------|-------------------|---------------|-------------------|
| **A**  | 1 – 126           | /8 (255.0.0.0)    | 126           | 16 777 214        |
| **B**  | 128 – 191         | /16 (255.255.0.0) | 16 384        | 65 534            |
| **C**  | 192 – 223         | /24 (255.255.255.0)| 2 097 152    | 254               |
| **D**  | 224 – 239         | —                 | —             | Multicast         |
| **E**  | 240 – 255         | —                 | —             | Réservé           |

> 127.x.x.x est réservée à la **boucle locale (loopback)** : `127.0.0.1` représente la machine elle-même.

---

### 2.5 Adresses privées et publiques

#### Adresses privées (RFC 1918)

Ces adresses ne sont **pas routables sur Internet**. Elles sont utilisées dans les réseaux locaux.

| Plage                         | Notation CIDR    | Classe |
|-------------------------------|------------------|--------|
| 10.0.0.0 – 10.255.255.255     | 10.0.0.0/8       | A      |
| 172.16.0.0 – 172.31.255.255   | 172.16.0.0/12    | B      |
| 192.168.0.0 – 192.168.255.255 | 192.168.0.0/16   | C      |

#### Adresses publiques

Les adresses publiques sont **uniques sur Internet** et attribuées par les organismes de régulation (IANA, RIPE NCC, ARIN, etc.).

#### Adresses spéciales

| Adresse         | Usage                              |
|-----------------|------------------------------------|
| 0.0.0.0         | Route par défaut / adresse indéfinie |
| 127.0.0.1       | Loopback (boucle locale)           |
| 255.255.255.255 | Broadcast limité                   |
| 169.254.x.x     | APIPA (attribution automatique sans DHCP) |

---

### 2.6 IPv6

Face à l'épuisement des adresses IPv4, le protocole **IPv6** a été développé. Il utilise des adresses de **128 bits**.

#### Structure d'une adresse IPv6

```
2001:0db8:85a3:0000:0000:8a2e:0370:7334
```

- **128 bits** divisés en **8 groupes de 16 bits** séparés par des deux-points `:`
- Représentation en **hexadécimal**
- Nombre total d'adresses : **2¹²⁸ ≈ 3,4 × 10³⁸** adresses

#### Simplification d'une adresse IPv6

```
2001:0db8:0000:0000:0000:0000:0000:0001
     ↓  Simplification
2001:db8::1    (les groupes de zéros consécutifs sont remplacés par ::)
```

#### Types d'adresses IPv6

| Type        | Description                                   | Préfixe          |
|-------------|-----------------------------------------------|------------------|
| Unicast     | Un seul destinataire                          | Varié            |
| Multicast   | Groupe de destinataires                       | ff00::/8         |
| Anycast     | Le plus proche d'un groupe                   | Varié            |
| Link-local  | Communication locale (même lien)             | fe80::/10        |
| Loopback    | Boucle locale                                 | ::1              |

#### Avantages d'IPv6

- Espace d'adressage quasi illimité
- Meilleure gestion de la sécurité (IPsec intégré)
- Autoconfig sans état (SLAAC)
- Pas de besoin de NAT
- Meilleure qualité de service (QoS)

---

### 2.7 NAT – Network Address Translation

Le **NAT** est un mécanisme permettant à plusieurs machines d'un réseau privé de **partager une seule adresse IP publique** pour accéder à Internet.

```
[ PC1: 192.168.1.2 ] ─┐
[ PC2: 192.168.1.3 ] ──┤  ROUTEUR NAT  ──── Internet
[ PC3: 192.168.1.4 ] ─┘  IP publique :
                          203.0.113.10
```

| Type de NAT    | Description                                     |
|----------------|--------------------------------------------------|
| **NAT statique** | Une IP privée ↔ Une IP publique fixe           |
| **NAT dynamique**| IP privée ↔ Pool d'IP publiques               |
| **PAT / NAPT**   | Plusieurs IP privées ↔ Une IP publique (ports) |

---

## 3. Les Protocoles Réseaux

Un **protocole réseau** est un ensemble de règles et de conventions définissant la façon dont les données sont formatées, transmises, reçues et interprétées entre équipements.

### 3.1 Le modèle OSI

Le modèle **OSI** (Open Systems Interconnection) est un modèle de référence à **7 couches** développé par l'ISO.

```
|------------------------------------------------------|
│  Couches  │     ROLES       │     Protocoles         │
|-----------|-----------------|------------------------|
│  Couche 7 │  APPLICATION    │  HTTP, FTP, DNS, SMTP  │
|------------------------------------------------------|
│  Couche 6 │  PRÉSENTATION   │  SSL/TLS, JPEG, ASCII  │
|------------------------------------------------------|
│  Couche 5 │  SESSION        │  NetBIOS, RPC          │
|------------------------------------------------------|
│  Couche 4 │  TRANSPORT      │  TCP, UDP              │
|------------------------------------------------------|
│  Couche 3 │  RÉSEAU         │  IP, ICMP, ARP         │
|------------------------------------------------------|
│  Couche 2 │  LIAISON        │  Ethernet, Wi-Fi, PPP  │
|------------------------------------------------------|
│  Couche 1 │  PHYSIQUE       │  Câbles, fibre, ondes  │
|------------------------------------------------------|
```


> **Voici un Petit Moyen mnémotechnique drôle pour garder les couches de 1 à 7 en tête:** "**P**artout **L**e **R**oi **T**rouve **S**a **P**lace **A**ssise"  
> (Physique – Liaison – Réseau – Transport – Session – Présentation – Application)

---

### 3.2 Le modèle TCP/IP

Le modèle **TCP/IP** (ou modèle Internet) est le modèle pratique sur lequel repose Internet. Il comporte **4 couches**.

```
Modèle OSI          Modèle TCP/IP         Exemples de protocoles
─────────────       ─────────────────     ──────────────────────
Application   ┐
Présentation  ├───► Application           HTTP, HTTPS, FTP, DNS,
Session       ┘                           SMTP, POP3, IMAP, SSH

Transport     ────► Transport             TCP, UDP

Réseau        ────► Internet              IP, ICMP, ARP, OSPF, BGP

Liaison       ┐
Physique      ├───► Accès réseau          Ethernet, Wi-Fi, PPP
              ┘
```

---

### 3.3 Protocoles de la couche Réseau

#### IP – Internet Protocol

C'est le protocole fondamental d'Internet. Il assure :
- L'**adressage** des hôtes
- Le **routage** des paquets
- La **fragmentation** et le **réassemblage** des données

IP est un protocole **non connecté et non fiable** (il ne garantit pas la livraison).

#### ICMP – Internet Control Message Protocol

Protocole de **messages de contrôle et d'erreur** utilisé pour diagnostiquer les problèmes réseaux.

```bash
# Exemples d'utilisation :
ping 8.8.8.8          ← Teste la connectivité (utilise ICMP Echo Request/Reply)
traceroute google.com ← Trace le chemin des paquets (utilise ICMP TTL Exceeded)
```

**Messages ICMP courants :**

| Type | Nom                  | Usage                     |
|------|----------------------|---------------------------|
| 0    | Echo Reply           | Réponse à un ping         |
| 3    | Destination Unreachable | Hôte/réseau inaccessible |
| 8    | Echo Request         | Envoi d'un ping           |
| 11   | Time Exceeded        | TTL expiré (traceroute)   |

#### ARP – Address Resolution Protocol

ARP fait la correspondance entre une **adresse IP** (couche 3) et une **adresse MAC** (couche 2).

```
PC1 veut envoyer un paquet à 192.168.1.2 mais ne connaît pas son adresse MAC.

PC1 envoie : "Qui a l'adresse 192.168.1.2 ? Répondez à AA:BB:CC:DD:EE:FF"
                         ← Broadcast ARP Request

PC2 répond : "C'est moi ! Mon adresse MAC est 11:22:33:44:55:66"
                         ← ARP Reply unicast

PC1 met en cache : 192.168.1.2 → 11:22:33:44:55:66
```

#### DHCP – Dynamic Host Configuration Protocol

Le DHCP permet l'**attribution automatique** des paramètres réseau (adresse IP, masque, passerelle, DNS).

**Processus DORA :**

```
Client                           Serveur DHCP
  │── DHCP Discover ─────────────────► │  (Je cherche un serveur DHCP)
  │◄── DHCP Offer ──────────────────── │  (Voici une IP disponible)
  │── DHCP Request ───────────────────► │  (Je veux cette IP)
  │◄── DHCP Acknowledge ─────────────── │  (IP confirmée, bail accordé)
```

#### Protocoles de routage

| Protocole | Type          | Usage                              |
|-----------|---------------|------------------------------------|
| **RIP**   | Distance vector | Petits réseaux, simple              |
| **OSPF**  | Link-state    | Grands réseaux d'entreprise        |
| **EIGRP** | Hybride (Cisco)| Réseaux Cisco                     |
| **BGP**   | Path vector   | Routage inter-domaines sur Internet|

---

### 3.4 Protocoles de la couche Transport

#### TCP – Transmission Control Protocol

TCP est un protocole **orienté connexion** qui garantit :
- La **fiabilité** (acquittement des données reçues)
- L'**ordre** des paquets
- Le **contrôle de flux** et de **congestion**

**Établissement de connexion (Three-Way Handshake) :**

```
Client                    Serveur
  │── SYN ───────────────► │   (Je veux me connecter, seq=x)
  │◄── SYN-ACK ──────────── │   (OK, seq=y, ack=x+1)
  │── ACK ───────────────► │   (Connexion établie, ack=y+1)
  │                         │
  │═══════ Données ════════ │   (Échange de données)
  │                         │
  │── FIN ───────────────► │   (Je veux fermer la connexion)
  │◄── FIN-ACK ──────────── │
```

**Ports TCP courants :**

| Port | Service         |
|------|-----------------|
| 20/21| FTP             |
| 22   | SSH             |
| 25   | SMTP            |
| 80   | HTTP            |
| 110  | POP3            |
| 143  | IMAP            |
| 443  | HTTPS           |
| 3306 | MySQL           |

#### UDP – User Datagram Protocol

UDP est un protocole **non connecté** et **non fiable** : il envoie les données sans établir de connexion préalable et sans garantir la livraison.

**Comparaison TCP vs UDP :**

| Caractéristique        | TCP                  | UDP                   |
|------------------------|----------------------|-----------------------|
| Connexion              | Oui (3-way handshake)| Non                   |
| Fiabilité              | Oui (accusés de réception)| Non              |
| Ordre des paquets      | Garantie             | Non garantie          |
| Vitesse                | Plus lent            | Plus rapide           |
| Contrôle de flux       | Oui                  | Non                   |
| Utilisations typiques  | Web, e-mail, FTP     | DNS, streaming, VoIP  |

---

### 3.5 Protocoles de la couche Application

#### DNS – Domain Name System

Le DNS traduit les **noms de domaine** en **adresses IP**.

```
Utilisateur tape : www.google.com
       ↓
Résolveur DNS local → Serveur DNS racine → Serveur TLD (.com) → Serveur autoritaire Google
       ↓
Résultat : 142.250.185.68
```

**Types d'enregistrements DNS :**

| Type  | Description                              | Exemple                       |
|-------|------------------------------------------|-------------------------------|
| **A**     | Nom → Adresse IPv4                  | example.com → 93.184.216.34  |
| **AAAA**  | Nom → Adresse IPv6                  | example.com → 2606:2800::1   |
| **CNAME** | Alias → Nom canonique               | www → example.com            |
| **MX**    | Serveur de messagerie               | mail.example.com              |
| **NS**    | Serveur de noms autoritaire         | ns1.example.com               |
| **PTR**   | IP → Nom (résolution inverse)       | 34.216.184.93.in-addr.arpa   |

#### HTTP / HTTPS – HyperText Transfer Protocol

HTTP est le protocole de transfert du Web. HTTPS est sa version sécurisée (avec chiffrement TLS).

**Méthodes HTTP principales :**

| Méthode  | Description                          |
|----------|--------------------------------------|
| GET      | Récupérer une ressource              |
| POST     | Envoyer des données                  |
| PUT      | Mettre à jour une ressource          |
| DELETE   | Supprimer une ressource              |
| HEAD     | Récupérer uniquement les en-têtes    |

**Codes de réponse HTTP :**

| Code | Signification               |
|------|-----------------------------|
| 200  | OK (succès)                 |
| 301  | Redirection permanente      |
| 400  | Mauvaise requête            |
| 401  | Non autorisé                |
| 403  | Accès interdit              |
| 404  | Page non trouvée            |
| 500  | Erreur interne du serveur   |

#### FTP – File Transfer Protocol

Protocole de transfert de fichiers entre client et serveur. Il utilise deux connexions :
- **Port 21** : connexion de contrôle (commandes)
- **Port 20** : connexion de données (transfert des fichiers)

> SFTP (SSH FTP) et FTPS (FTP + TLS) sont les versions sécurisées recommandées.

#### SMTP / POP3 / IMAP – Protocoles de messagerie

| Protocole | Port    | Rôle                                              |
|-----------|---------|---------------------------------------------------|
| **SMTP**  | 25/587  | Envoi de courriers électroniques                 |
| **POP3**  | 110/995 | Réception (télécharge et supprime du serveur)    |
| **IMAP**  | 143/993 | Réception (synchronise avec le serveur)          |

#### SSH – Secure Shell

SSH permet d'accéder à un terminal distant de façon **chiffrée et sécurisée** (port **22**). Il remplace Telnet (non sécurisé).

```bash
ssh utilisateur@192.168.1.10   # Connexion SSH
scp fichier.txt user@host:/tmp # Copie sécurisée de fichiers
```

#### SNMP – Simple Network Management Protocol

Protocole de **supervision réseau** permettant de surveiller et gérer les équipements (routeurs, switchs, serveurs).

---

### 3.6 Protocoles de sécurité

#### TLS/SSL – Transport Layer Security

TLS (et son prédécesseur SSL) chiffre les communications entre client et serveur. Il est utilisé par HTTPS, SMTPS, IMAPS, etc.

**Fonctionnement simplifié :**

```
1. Handshake TLS : échange de certificats et négociation des algorithmes
2. Échange de clés (Diffie-Hellman ou RSA)
3. Communication chiffrée symétrique (AES, ChaCha20...)
```

#### IPsec – Internet Protocol Security

Suite de protocoles assurant la sécurité au niveau de la **couche réseau** (IP). Utilisé dans les **VPN** (réseaux privés virtuels).

| Composant | Rôle                                          |
|-----------|-----------------------------------------------|
| **AH**    | Authentification et intégrité (sans chiffrement) |
| **ESP**   | Authentification, intégrité et chiffrement    |
| **IKE**   | Gestion des clés de chiffrement               |

#### Firewall et filtrage

Les pare-feu (firewalls) filtrent les paquets en fonction des protocoles et des ports :

```
RÈGLES EXEMPLE :
ACCEPTER  TCP  depuis ANY  vers 80   (HTTP entrant)
ACCEPTER  TCP  depuis ANY  vers 443  (HTTPS entrant)
REJETER   TOUT  depuis ANY  vers 22  (SSH interdit depuis l'extérieur)
REJETER   TOUT  (règle par défaut)
```

---

## 4. Conclusion

L'adressage IP et les protocoles réseaux forment le socle sur lequel repose toute communication numérique. En résumé :

- **IPv4** est encore largement utilisé mais souffre de la pénurie d'adresses, d'où la migration progressive vers **IPv6**.
- Les **masques de sous-réseau** et la notation **CIDR** permettent une organisation hiérarchique et efficace des adresses.
- Le modèle **OSI** offre un cadre théorique à 7 couches, tandis que le modèle **TCP/IP** est le modèle pratique utilisé sur Internet.
- **TCP** garantit la fiabilité des échanges, tandis qu'**UDP** privilégie la rapidité.
- Des protocoles comme **DNS**, **HTTP**, **SMTP** et **SSH** assurent les services applicatifs du quotidien.
- La sécurité est assurée par des protocoles comme **TLS**, **IPsec** et des mécanismes de filtrage.

La maîtrise de ces notions est indispensable pour tout professionnel des réseaux et des systèmes d'information.

---

## 5. Glossaire

| Terme       | Définition                                                             |
|-------------|------------------------------------------------------------------------|
| **ACK**     | Accusé de réception (Acknowledgment)                                   |
| **ARP**     | Protocole de résolution d'adresses MAC                                |
| **BGP**     | Protocole de routage inter-domaines                                    |
| **CIDR**    | Routage inter-domaines sans classe                                     |
| **DHCP**    | Attribution automatique d'adresses IP                                  |
| **DNS**     | Système de noms de domaine                                             |
| **FTP**     | Protocole de transfert de fichiers                                     |
| **HTTP**    | Protocole de transfert hypertexte                                      |
| **ICMP**    | Protocole de messages de contrôle Internet                            |
| **IP**      | Protocole Internet                                                     |
| **IPsec**   | Sécurité au niveau du protocole IP                                    |
| **LAN**     | Réseau local                                                           |
| **MAC**     | Adresse physique d'une carte réseau                                   |
| **NAT**     | Traduction d'adresses réseau                                          |
| **OSI**     | Modèle de référence à 7 couches                                       |
| **OSPF**    | Protocole de routage à état de lien                                   |
| **PAT**     | Traduction d'adresses et de ports                                     |
| **RFC**     | Document de référence des standards Internet                          |
| **SMTP**    | Protocole d'envoi de courriers électroniques                          |
| **SSH**     | Shell sécurisé (accès distant chiffré)                                |
| **TCP**     | Protocole de contrôle de transmission (fiable)                        |
| **TLS**     | Protocole de sécurité de la couche transport                          |
| **TTL**     | Durée de vie d'un paquet IP                                           |
| **UDP**     | Protocole de datagramme utilisateur (rapide, non fiable)              |
| **VPN**     | Réseau privé virtuel                                                   |
| **WAN**     | Réseau étendu                                                          |

---

*Exposé rédigé par [RUSTICO Charbel](https://github.com/charbelrustico/) – Cours ***Fondamentaux des Réseaux et Systèmes*** – Mars 2026*
