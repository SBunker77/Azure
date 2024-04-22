## Modèle de Déploiement Azure - Infrastructure Réseau et Machines Virtuelles

Ce modèle Azure Resource Manager (ARM) est conçu pour déployer une infrastructure réseau avec des machines virtuelles dans Microsoft Azure. Le modèle déploie les ressources suivantes :

- **Serveur SQL**
  - Crée un serveur SQL avec un nom dynamique basé sur l'ID du groupe de ressources.
  - Définit le nom d'utilisateur administrateur et le mot de passe.
  - Crée une base de données associée au serveur SQL.

- **Réseau Virtuel (VNET)**
  - Crée un réseau virtuel avec un espace d'adressage spécifié et deux sous-réseaux (web et business).

- **Groupes de Sécurité Réseau (NSG)**
  - Crée deux groupes de sécurité réseau pour les sous-réseaux web et business avec des règles de sécurité spécifiques.

- **Interfaces Réseau (NIC)**
  - Crée deux interfaces réseau pour les machines virtuelles associées aux sous-réseaux web et business.

- **Machines Virtuelles (VM)**
  - Crée deux machines virtuelles Linux (Ubuntu Server 18.04) dans les sous-réseaux web et business.
  - Configure les disques système, les configurations réseau, les profils OS, et déploie des scripts personnalisés pour chaque machine virtuelle.

---

### Paramètres

- `username` : Nom d'utilisateur administrateur pour les machines virtuelles (par défaut : `adminaccount`).
- `password` : Mot de passe administrateur pour les machines virtuelles (chiffré).
- `sqlServerName` : Nom du serveur SQL à déployer (par défaut : `SHOP-DB-SERVER` suivi d'une chaîne unique).

### Dépendances

Ce déploiement crée des dépendances entre les ressources pour garantir leur déploiement correct :

- Les machines virtuelles dépendent des interfaces réseau (NIC) et du réseau virtuel (VNET).
- Les règles de sécurité réseau (NSG) dépendent du déploiement du réseau virtuel (VNET).

---

### Détails des Composants

- **Serveur SQL**
  - Crée un serveur SQL avec une base de données appelée `SHOP-DATABASE`.
  - L'administrateur de la base de données est défini avec les informations spécifiées dans les paramètres.

- **Réseau Virtuel**
  - Crée un réseau virtuel nommé `SHOP-VNET` avec l'adresse `10.1.0.0/16`.
  - Inclut deux sous-réseaux : `demo-web-subnet` (adresse `10.1.0.0/24`) et `demo-biz-subnet` (adresse `10.1.1.0/24`).

- **Groupes de Sécurité Réseau**
  - `SHOP-NSG-1` (pour le sous-réseau web) : Permet le trafic HTTP entrant sur le port 80.
  - `SHOP-NSG` (pour le sous-réseau business) : Permet le trafic HTTP entrant sur le port 80 uniquement depuis le sous-réseau web.

- **Machines Virtuelles (VM)**
  - Crée deux machines virtuelles Linux avec une image Ubuntu Server 18.04.
  - Les machines sont configurées avec des adresses IP statiques dans les sous-réseaux correspondants.

---

### Déploiement

Pour déployer cette infrastructure via Azure Resource Manager :

1. Utilisez le portail Azure ou les outils CLI/PowerShell pour déployer ce modèle.
2. Assurez-vous de fournir les valeurs requises pour les paramètres `username`, `password`, et `sqlServerName`.
3. Suivez les étapes de déploiement et attendez la fin du processus.

Une fois le déploiement terminé, les machines virtuelles seront accessibles via leurs adresses IP publiques, et la base de données SQL sera prête à être utilisée avec les informations d'administration spécifiées.
