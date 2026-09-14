# SAE 5.01

---

# Page du projet

Rapport SAE 501

---

# Conception et déploiement d’un Cyber-Lab virtuel automatisé

## 🎯 Objectifs

À l'issue du projet, vous devrez être capables de :

1. **Concevoir une architecture réseau virtuelle** permettant d'isoler plusieurs environnements de TP.
2. Installer et configurer un hyperviseur et les réseaux virtuels associés.
3. Créer des **machines virtuelles modèles** correspondant aux différents rôles du cyber-lab.
4. Mettre en place un mécanisme de **déploiement reproductible** d'un environnement complet.
5. Permettre l'accès distant aux machines depuis une interface web ou un service sécurisé.
6. Mettre en œuvre des mécanismes de **segmentation et de contrôle des accès**.
7. Mettre en place une supervision minimale de l'infrastructure.
8. Documenter l'architecture et fournir des procédures d'exploitation.

> 💡 **Note :** Les choix techniques pour la réalisation du projet sont laissés libres aux étudiants.
> 

---

## 📖 Scénario

L'IUT souhaite mettre à disposition des enseignants une plateforme permettant de créer rapidement des **environnements de TP réseau/cybersécurité isolés**.

Un enseignant doit pouvoir demander la création d'un environnement de TP pour un groupe.

### Exemple d'architecture :

```
**RT-SAE5.01 :**                     INTERNET
                        │
                 ┌──────┴──────┐
                 │   Firewall  │
                 │   virtuel   │
                 └──────┬──────┘
                        │
                ┌───────┴────────┐
                │ Réseau du TP 01│
                └───────┬────────┘
                        │
       ┌────────────────┼────────────────┐
       │                │                │
   ┌───▼───┐        ┌───▼───┐        ┌───▼────┐
   │ Client│        │ Serveur│       │ Vuln.  │
   │ Linux │        │ Linux  │       │   VM   │
   └───────┘        └────────┘       └────────┘
```

Chaque groupe doit pouvoir travailler dans son propre environnement **sans pouvoir accéder aux machines des autres groupes**.

## 🗂️ Plan du projet

### Phase 1 — Analyse et conception

**Objectifs :**

- Analyser le besoin
- Définir l'architecture générale
- Choisir l'hyperviseur
- Définir le nombre et le rôle des VM
- Concevoir le plan d'adressage IP
- Définir les réseaux virtuels
- Identifier les besoins de sécurité
- Établir un planning

**Livrables :**

- Cahier des charges
- Schéma d'architecture
- Plan d'adressage
- Justification des choix techniques

### Phase 2 — Mise en place de l'infrastructure de virtualisation

Installation et configuration de l'hyperviseur.

**Objectifs :**

- Installer un hyperviseur : Proxmox / KVM / Hyper-V / VMware
- Créer les bridges ou réseaux virtuels
- Configurer les ressources CPU / RAM / disque
- Créer les VLAN ou réseaux isolés nécessaires
- Mettre en place les comptes d'administration
- Réaliser un premier environnement de test

**Livrables :**

- Infrastructure fonctionnelle + documentation d'installation

### Phase 3 — Création du Cyber-Lab

Création des différentes VM modèles : Clients, Serveurs, etc.

**Objectifs :**

- Installer les OS
- Configurer le réseau
- Installer les services nécessaires
- Créer les comptes
- Préparer les snapshots / templates
- Vérifier la reproductibilité du déploiement

> 🎯 **Objectif clé :** Pouvoir **recréer rapidement un environnement complet**.
> 

### Phase 4 — Automatisation du déploiement

Vous devez automatiser au maximum le déploiement en utilisant une petite interface web.

*Exemple avec Ansible :*

Plaintext

```
                 Playbook Ansible
                        │
                        ▼
              ┌─────────────────┐
              │ Infrastructure  │
              └────────┬────────┘
                       │
          ┌────────────┼────────────┐
          ▼            ▼            ▼
       Client        Server      Vulnérable
```

> « Je lance une commande/script et un environnement TP complet est créé et configuré. »
> 

**Éléments à automatiser :**

- Création / clonage des VM
- Configuration réseau
- Création des utilisateurs
- Installation des paquets
- Configuration des services
- Configuration du firewall
- Attribution des adresses IP
- Création des règles de filtrage

### Phase 5 — Sécurité et isolation

Vous devez démontrer que deux environnements de TP sont réellement isolés.

**À mettre en œuvre :**

- Segmentation réseau
- Règles firewall
- Comptes avec privilèges limités
- Authentification
- Contrôle des accès
- Limitation des communications inter-TP
- Sécurisation de l'administration de l'infrastructure

> ⚠️ **Test à réaliser :** Effectuer des **tests d'intrusion ou de vérification de configuration** dans leur propre environnement.
> 
> 
> *Exemple :* Un étudiant du TP 01 doit pouvoir accéder à son serveur mais **ne doit pas** pouvoir atteindre le serveur du TP 02.
> 

### Phase 6 — Supervision et documentation

**Supervision minimale à mettre en place :**

- État des VM
- Utilisation CPU / RAM
- Espace disque
- Disponibilité des services
- Éventuellement gestion des logs

**Documentation à fournir :**

- Documentation d'installation
- Documentation d'exploitation
- Procédure de création d'un nouveau TP
- Procédure de dépannage
- Schémas réseau
- Inventaire des machines

### Phase 7 — Tests, démonstration et livraison

Démonstration de bout en bout selon le scénario suivant :

1. Création d'un nouvel environnement
2. Déploiement automatique des VM
3. Configuration automatique du réseau
4. Connexion à une VM
5. Vérification des services
6. Démonstration de l'isolation entre deux TP
7. Arrêt / suppression de l'environnement
8. Recréation de l'environnement

## 📊 Barème proposé

| **Phase** | **Description** | **Points** |
| --- | --- | --- |
| **Phase 1** | Conception | 10 |
| **Phase 2** | Virtualisation | 20 |
| **Phase 3** | Création du Cyber-Lab | 20 |
| **Phase 4** | Automatisation | **25** |
| **Phase 5** | Sécurité et isolation | 15 |
| **Phase 6** | Supervision / documentation | 5 |
| **Phase 7** | Tests et soutenance | 5 |
| **TOTAL** |  | **100** |
