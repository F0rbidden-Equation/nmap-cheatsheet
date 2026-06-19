# nmap-cheatsheet
````markdown
# Nmap - Notes, Commandes et Scripts NSE

## Présentation

Ce dépôt regroupe mes notes personnelles sur **Nmap**, un outil essentiel pour la reconnaissance réseau et l'audit de sécurité.

Nmap permet de :

- découvrir des machines actives sur un réseau ;
- scanner des ports ouverts ;
- identifier des services ;
- détecter certaines versions ;
- utiliser des scripts NSE pour automatiser l'énumération ;
- sauvegarder les résultats dans différents formats.

> Ce dépôt est réalisé dans un objectif pédagogique, dans le cadre de mon apprentissage en cybersécurité.

---

## Avertissement légal

Les commandes présentes dans ce dépôt doivent être utilisées uniquement :

- sur mes propres machines ;
- dans un laboratoire local ;
- sur des machines volontairement vulnérables ;
- avec une autorisation explicite.

L'utilisation de Nmap sur un système tiers sans autorisation peut être illégale.

---

## Installation

### Debian / Ubuntu / Kali Linux

```bash
sudo apt update
sudo apt install nmap -y
````

### Vérifier l'installation

```bash
nmap --version
```

---

## Infographies

### Commandes Nmap essentielles


![Nmap - Commandes essentielles](./nmap-commandes-essentielles.png)

### Scripts NSE essentiels

```markdown
![Nmap - Scripts NSE essentiels](assets/nmap-scripts-nse.png)
```

---

## Commandes Nmap de base

### Scanner une adresse IP

```bash
nmap 192.168.1.10
```

### Scanner un réseau local

```bash
nmap 192.168.1.0/24
```

### Découvrir les machines actives

```bash
nmap -sn 192.168.1.0/24
```

### Scanner un port précis

```bash
nmap -p 22 192.168.1.10
```

### Scanner plusieurs ports

```bash
nmap -p 22,80,443 192.168.1.10
```

### Scanner tous les ports TCP

```bash
nmap -p- 192.168.1.10
```

### Détecter les versions des services

```bash
nmap -sV 192.168.1.10
```

### Lancer les scripts par défaut

```bash
nmap -sC 192.168.1.10
```

### Scan classique utile

```bash
nmap -sC -sV 192.168.1.10
```

### Scan plus complet

```bash
sudo nmap -A 192.168.1.10
```

---

## Options importantes

| Option | Description                                       |
| ------ | ------------------------------------------------- |
| `-sn`  | Découverte d'hôtes sans scan de ports             |
| `-p`   | Spécifier un ou plusieurs ports                   |
| `-p-`  | Scanner tous les ports TCP                        |
| `-sV`  | Détecter les versions des services                |
| `-sC`  | Exécuter les scripts NSE par défaut               |
| `-O`   | Tenter de détecter le système d'exploitation      |
| `-A`   | Scan agressif : OS, versions, scripts, traceroute |
| `-sU`  | Scan UDP                                          |
| `-Pn`  | Considérer la cible comme active                  |
| `-T4`  | Accélérer le scan                                 |
| `-oN`  | Sauvegarde au format texte                        |
| `-oX`  | Sauvegarde au format XML                          |
| `-oA`  | Sauvegarde dans plusieurs formats                 |

---

# Nmap Scripting Engine - NSE

## Présentation

Le **Nmap Scripting Engine**, abrégé **NSE**, permet d'utiliser des scripts avec Nmap.

Les scripts NSE permettent d'automatiser certaines tâches comme :

* l'énumération ;
* la découverte d'informations ;
* la récupération de bannières ;
* la détection de versions ;
* la recherche de mauvaises configurations ;
* certains contrôles de sécurité.

Les scripts NSE sont écrits en **Lua**.

---

## Commande NSE de base

```bash
nmap --script <nom_du_script> <cible>
```

Exemple :

```bash
nmap --script http-title 192.168.1.10
```

Avec un port précis :

```bash
nmap --script http-title -p 80 192.168.1.10
```

Avec plusieurs scripts :

```bash
nmap --script http-title,http-headers -p 80,443 192.168.1.10
```

---

## Utiliser les scripts par défaut

L'option `-sC` permet d'exécuter les scripts NSE par défaut.

```bash
nmap -sC 192.168.1.10
```

Souvent, on l'utilise avec `-sV` :

```bash
nmap -sC -sV 192.168.1.10
```

Sur des ports précis :

```bash
nmap -sC -sV -p 22,80,443 192.168.1.10
```

---

## Catégories principales des scripts NSE

| Catégorie   | Rôle                                      |
| ----------- | ----------------------------------------- |
| `default`   | Scripts lancés avec l'option `-sC`        |
| `safe`      | Scripts à faible impact                   |
| `discovery` | Découverte d'informations                 |
| `version`   | Amélioration de la détection de versions  |
| `auth`      | Tests liés à l'authentification           |
| `vuln`      | Recherche de vulnérabilités connues       |
| `intrusive` | Scripts plus agressifs                    |
| `external`  | Scripts utilisant des ressources externes |
| `malware`   | Détection d'indices liés à des malwares   |
| `dos`       | Tests pouvant perturber un service        |

---

## Scripts NSE utiles par service

## HTTP / HTTPS

| Script               | Description                                    |
| -------------------- | ---------------------------------------------- |
| `http-title`         | Récupère le titre d'une page web               |
| `http-headers`       | Affiche les en-têtes HTTP                      |
| `http-methods`       | Liste les méthodes HTTP autorisées             |
| `http-enum`          | Recherche des chemins web courants             |
| `http-server-header` | Récupère l'en-tête serveur                     |
| `ssl-cert`           | Affiche les informations du certificat SSL/TLS |
| `ssl-enum-ciphers`   | Analyse les suites de chiffrement SSL/TLS      |

### Exemples HTTP

```bash
nmap --script http-title -p 80 192.168.1.10
```

```bash
nmap --script http-headers -p 80,443 192.168.1.10
```

```bash
nmap --script ssl-cert -p 443 192.168.1.10
```

```bash
nmap --script ssl-enum-ciphers -p 443 192.168.1.10
```

---

## SSH

| Script             | Description                               |
| ------------------ | ----------------------------------------- |
| `ssh-auth-methods` | Liste les méthodes d'authentification SSH |
| `ssh-hostkey`      | Affiche les clés publiques du serveur SSH |
| `ssh2-enum-algos`  | Énumère les algorithmes SSH supportés     |

### Exemples SSH

```bash
nmap --script ssh-auth-methods -p 22 192.168.1.10
```

```bash
nmap --script ssh-hostkey -p 22 192.168.1.10
```

```bash
nmap --script ssh2-enum-algos -p 22 192.168.1.10
```

---

## FTP

| Script       | Description                             |
| ------------ | --------------------------------------- |
| `ftp-anon`   | Vérifie si l'accès anonyme est autorisé |
| `ftp-syst`   | Récupère des informations système FTP   |
| `ftp-bounce` | Teste la vulnérabilité FTP bounce       |

### Exemples FTP

```bash
nmap --script ftp-anon -p 21 192.168.1.10
```

```bash
nmap --script ftp-syst -p 21 192.168.1.10
```

---

## SMB

| Script              | Description                                |
| ------------------- | ------------------------------------------ |
| `smb-os-discovery`  | Détecte l'OS, le nom machine et le domaine |
| `smb-protocols`     | Liste les versions SMB supportées          |
| `smb-security-mode` | Analyse le mode de sécurité SMB            |
| `smb-enum-shares`   | Énumère les partages SMB                   |
| `smb-enum-users`    | Énumère les utilisateurs SMB si autorisé   |

### Exemples SMB

```bash
nmap --script smb-os-discovery -p 445 192.168.1.10
```

```bash
nmap --script smb-protocols -p 445 192.168.1.10
```

```bash
nmap --script smb-security-mode -p 445 192.168.1.10
```

---

## DNS

| Script                  | Description                             |
| ----------------------- | --------------------------------------- |
| `dns-service-discovery` | Recherche des services DNS              |
| `dns-recursion`         | Vérifie si la récursion DNS est activée |
| `dns-zone-transfer`     | Teste un transfert de zone DNS autorisé |

### Exemples DNS

```bash
nmap --script dns-recursion -p 53 192.168.1.10
```

```bash
nmap --script dns-zone-transfer -p 53 example.local
```

---

## SNMP

| Script            | Description                           |
| ----------------- | ------------------------------------- |
| `snmp-info`       | Récupère des informations SNMP        |
| `snmp-interfaces` | Liste les interfaces réseau           |
| `snmp-processes`  | Liste les processus visibles via SNMP |
| `snmp-sysdescr`   | Récupère la description système SNMP  |

### Exemples SNMP

```bash
nmap -sU --script snmp-info -p 161 192.168.1.10
```

```bash
nmap -sU --script snmp-interfaces -p 161 192.168.1.10
```

---

## Recherche de vulnérabilités avec NSE

La catégorie `vuln` permet de rechercher certaines vulnérabilités connues.

```bash
nmap --script vuln 192.168.1.10
```

Avec détection de version :

```bash
nmap -sV --script vuln 192.168.1.10
```

Sur des ports précis :

```bash
nmap -sV --script vuln -p 80,443,445 192.168.1.10
```

> Attention : certains scripts peuvent générer du trafic plus intrusif. À utiliser uniquement en laboratoire ou avec autorisation.

---

## Rechercher les scripts installés

Lister les scripts NSE :

```bash
ls /usr/share/nmap/scripts/
```

Rechercher les scripts HTTP :

```bash
ls /usr/share/nmap/scripts/ | grep http
```

Rechercher les scripts SMB :

```bash
ls /usr/share/nmap/scripts/ | grep smb
```

Rechercher les scripts SSH :

```bash
ls /usr/share/nmap/scripts/ | grep ssh
```

---

## Obtenir de l'aide sur un script

```bash
nmap --script-help http-title
```

Exemple :

```bash
nmap --script-help smb-os-discovery
```

---

## Mettre à jour la base des scripts

```bash
sudo nmap --script-updatedb
```

---

## Méthodologie simple avec Nmap et NSE

### 1. Découverte du réseau

```bash
nmap -sn 192.168.1.0/24
```

### 2. Scan des ports ouverts

```bash
nmap -p- 192.168.1.10
```

### 3. Détection des services

```bash
nmap -sV -p 22,80,443,445 192.168.1.10
```

### 4. Scripts par défaut

```bash
nmap -sC -sV -p 22,80,443,445 192.168.1.10
```

### 5. Scripts ciblés selon les services

```bash
nmap --script http-title,http-headers -p 80,443 192.168.1.10
```

```bash
nmap --script ssh-auth-methods -p 22 192.168.1.10
```

```bash
nmap --script smb-os-discovery,smb-protocols -p 445 192.168.1.10
```

### 6. Exporter les résultats

```bash
nmap -sC -sV -oA reports/scan-nse 192.168.1.10
```

---

## Sauvegarder les résultats

### Format texte

```bash
nmap -sV 192.168.1.10 -oN scan.txt
```

### Format XML

```bash
nmap -sV 192.168.1.10 -oX scan.xml
```

### Tous les formats

```bash
nmap -sV 192.168.1.10 -oA scan-result
```

Cela génère :

```bash
scan-result.nmap
scan-result.xml
scan-result.gnmap
```

---

## Exemple de rapport simple

````markdown
# Rapport Nmap

## Cible

Adresse IP : `192.168.1.10`

## Commande utilisée

```bash
nmap -sC -sV -p 22,80,443 192.168.1.10
````

## Ports ouverts

| Port | Service | Information                           |
| ---- | ------- | ------------------------------------- |
| 22   | SSH     | Méthodes d'authentification détectées |
| 80   | HTTP    | Titre de page récupéré                |
| 443  | HTTPS   | Certificat SSL/TLS analysé            |

## Analyse

Les scripts NSE ont permis de compléter l'énumération des services exposés.

## Recommandations

* Vérifier les versions des services.
* Désactiver les services inutiles.
* Restreindre les accès distants.
* Corriger les mauvaises configurations.
* Relancer un scan après correction.

```

---

## Bonnes pratiques

- Toujours vérifier le périmètre autorisé.
- Commencer par un scan simple.
- Utiliser `-sV` pour identifier les versions.
- Utiliser `-sC` pour les scripts par défaut.
- Cibler les ports avec `-p`.
- Sauvegarder les résultats avec `-oA`.
- Lire les résultats avant de conclure.
- Éviter les scripts intrusifs sans autorisation.
- Documenter les commandes utilisées.

---

## À retenir

Nmap permet de découvrir des hôtes, des ports ouverts, des services et parfois le système d'exploitation d'une cible.

Les scripts NSE permettent d'aller plus loin en automatisant la découverte, l'énumération et certains contrôles de sécurité.

Nmap ne remplace pas l'analyse humaine : les résultats doivent toujours être compris, vérifiés et replacés dans leur contexte.
```
