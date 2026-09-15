<aside>
🚨

## Login et MDP :

guacadmin

</aside>

https://www.it-connect.fr/tuto-apache-guacamole-bastion-rdp-ssh-debian/#B_Ajouter_une_connexion_RDP

!image.png

---

Le navigateur de l'utilisateur communique en **WebSocket** avec le client Guacamole, qui transmet les requêtes à guacd. Ce dernier établit la connexion vers la machine cible via le protocole approprié. L'utilisateur ne voit qu'une page web — toute la complexité est masquée.

https://www.vaultaura.com/blog/apache-guacamole-acces-distant-securise-sans-vpn-client

### **2. Sécurité renforcée par design**

Contrairement à un VPN classique qui ouvre un tunnel réseau complet vers votre infrastructure, Guacamole n'expose **aucun port réseau** directement sur Internet. Seul le serveur Guacamole est accessible (via HTTPS), et c'est lui qui établit les connexions internes.

Les bénéfices en termes de sécurité :

- **Pas de tunnel réseau ouvert** : l'utilisateur n'a pas accès au réseau interne, uniquement à la machine autorisée
- **Chiffrement HTTPS de bout en bout** entre le navigateur et le serveur Guacamole
- **Authentification centralisée** : intégration LDAP, Active Directory, TOTP (MFA), SAML, OpenID Connect
- **Journalisation complète** : chaque session est tracée, avec possibilité d'enregistrer les sessions en vidéo
- **Contrôle granulaire** : permissions par utilisateur, par groupe, par connexion

https://www.vaultaura.com/blog/apache-guacamole-acces-distant-securise-sans-vpn-client

### **4. Expérience utilisateur fluide**

L'interface web de Guacamole est intuitive. L'utilisateur se connecte, voit la liste de ses machines autorisées, clique, et le bureau distant s'affiche. Les fonctionnalités incluent :

- Copier-coller entre la machine locale et la session distante
- Transfert de fichiers (upload/download) via SFTP intégré
- Partage d'écran et collaboration en temps réel
- Redimensionnement dynamique de la fenêtre
- Support du son et de l'impression distante (via RDP)

<aside>
💡

## a faire absolument a l’interieur de l’hyperviseur

a installer sur docker pour economiser des perf et pour les maj c’est mieux 

si on utilise pas LXC c’est psk avec LXC on a besoin d’une config total du guacamol /zabbix 

NE PAS OUBLIÉ DE SEPARER LES DEUX VM DANS LES VLAN 

</aside>

### Les arguments à placer dans votre rapport (Pour les points bonus)

Pour justifier ce choix devant un jury, voici le vocabulaire technique à employer :

1. **"Évitement du paradoxe du moniteur" :** *« J'ai isolé Zabbix sur sa propre VM. Si je l'avais mis sur la même machine que le reste, un crash de l'OS m'aurait privé de mon service ET de mon système d'alerte en même temps. »*
2. **"Sécurité en profondeur (Defense in Depth)" :** *« Guacamole est une passerelle d'accès (Bastion). Par définition, elle est plus exposée aux requêtes externes. En la séparant de Zabbix, je m'assure qu'une faille de sécurité sur la passerelle ne donne pas un accès direct aux données de supervision de toute l'infrastructure. »*
3. **"Scalabilité (Évolutivité)" :** *« Dans une vraie entreprise, la base de données de Zabbix va grossir très vite avec l'historique des métriques. En ayant une VM dédiée, je peux augmenter le disque ou la RAM de Zabbix à chaud depuis Proxmox, sans impacter les utilisateurs connectés sur Guacamole. »*
4. **"Infrastructure as Code" :** *« J'ai choisi Docker pour les deux services afin de pouvoir recréer toute mon infrastructure en quelques secondes grâce aux fichiers docker-compose, garantissant une reprise d'activité rapide (Plan de Reprise d'Activité - PRA). »*

---

Oui, c'est exactement la méthode standard pour ce type de projet. C'est ce qu'on appelle déployer une **Appliance Virtuelle** (ou NFV : *Network Functions Virtualization*).

Pour le jury, utiliser une VM dédiée avec un système d'exploitation de pare-feu (comme OPNsense ou pfSense) montre que vous maîtrisez la virtualisation réseau. Le système d'exploitation installé dans cette VM croira qu'il se trouve dans un véritable boîtier physique.

**Comment simuler ce boîtier dans Proxmox**

Pour que cette VM agisse comme un véritable routeur physique, vous devez lui attribuer **deux cartes réseaux virtuelles** connectées à deux commutateurs virtuels (Bridges) différents dans Proxmox :

| **Carte réseau de la VM (Firewall)** | **Connectée au Pont Proxmox** | **Rôle (Comme sur une vraie box)** |
| --- | --- | --- |
| **Carte 1 (WAN / Extérieur)** | `vmbr0` (Relié à la vraie box internet de l'école/chez vous) | Le port "Internet". C'est par là que les menaces arrivent. Le pare-feu bloque tout ce qui entre ici par défaut. |
| **Carte 2 (LAN / Intérieur)** | `vmbr1` (Un réseau purement interne, déconnecté physiquement) | Le port "Local". C'est ici que vous connectez votre VM Guacamole, votre VM Zabbix, et vos autres environnements de travail. |

**Les avantages pour votre TP**

- **Environnement bac à sable :** Vos machines de travail (LAN) seront dans une bulle totalement isolée du réseau de votre école. Le pare-feu virtuel distribuera ses propres adresses IP (serveur DHCP) à vos autres VMs.
- **Sécurité parfaite de l'hyperviseur :** Proxmox ne sera accessible que depuis le réseau LAN (derrière le pare-feu). Si quelqu'un scanne le réseau de l'école, il ne verra que le port WAN de votre pare-feu, qui repoussera l'attaque.
- **Tolérance à l'erreur :** Si vous vous trompez dans une règle de pare-feu et que vous coupez la connexion, vous pouvez utiliser la console de Proxmox pour entrer dans la VM Firewall et corriger l'erreur, ou simplement restaurer un snapshot d'avant la modification.

**Le flux logique de votre environnement de travail :**

La connexion de l'utilisateur arrive sur le port physique du serveur Proxmox ➔ Elle est aspirée directement par la VM **Pare-feu** ➔ Le Pare-feu filtre et route vers la VM **Guacamole** ➔ L'utilisateur utilise Guacamole pour accéder à la VM **Zabbix** ou aux autres **VMs de travail**.

aide conf IA 

<aside>
💡 le suricata est implementer dans OPNsens                                                                        si on peut mettre un suricata pour la surveillance réseau c’est prime

Suricata 

</aside>

PK OPNsens plutot que PFsens

| Feature | OPNsense | pfSense |
| --- | --- | --- |
| License | Fully Open Source | CE: Open, Plus: Proprietary |
| UI Style | Modern sidebar navigation | Traditional top menu |
| Update Frequency | Bi-yearly major releases | Annual major releases |
| Built-in IDS | Yes (Suricata) | Requires package install |
| WireGuard | Built-in by default | Requires plugin |
| Security Patches | Days after FreeBSD | CE waits for Plus first |
| Best For | Modern workflows, open-source advocates | Conservative updates, legacy setups |

Pour transformer ce très bon projet technique en une véritable architecture de niveau ingénieur, voici les axes d'amélioration majeurs qui impressionneront un jury. Ces points peuvent être intégrés au projet ou ajoutés dans la section "Perspectives d'évolution" de votre rapport.

**1. Sécurisation avancée des accès (Hardening)**

- **MFA (Authentification Multifacteur) :** Un bastion (Guacamole) sans 2FA est une hérésie en entreprise. Activez l'extension TOTP sur Apache Guacamole pour exiger un code (Google Authenticator, Authy) à chaque connexion.
- **Chiffrement (SSL/TLS) :** Les jurys détestent les alertes "Site non sécurisé" des navigateurs. Déployez un Reverse Proxy (comme Nginx Proxy Manager ou Traefik) dans votre zone DMZ pour gérer des certificats SSL (Let's Encrypt) et chiffrer les flux web en HTTPS de bout en bout.

**2. Plan de Reprise d'Activité (PRA)**

- **Stratégie de sauvegarde :** Proxmox intègre un outil de sauvegarde natif (Vzdump). Automatisez des sauvegardes régulières de vos VMs vers un stockage externe, et documentez la procédure exacte pour remonter toute l'infrastructure en cas de crash du serveur physique.
- **Versionnement de l'infrastructure :** Les fichiers `docker-compose.yml` de Guacamole et Zabbix, ainsi que le fichier de sauvegarde XML du pare-feu pfSense/OPNsense, doivent être stockés sur un dépôt Git (GitHub/GitLab). Cela prouve que votre architecture est reproductible en cas de sinistre total.

**3. Supervision à 360 degrés**

- **Le monitoring de l'hyperviseur :** Zabbix ne doit pas surveiller uniquement vos VMs de travail. Il doit impérativement surveiller le serveur matériel Proxmox lui-même (via SNMP ou un agent Zabbix) pour anticiper une panne critique (usure des disques, saturation de la RAM hôte).

**4. Automatisation (Le niveau Supérieur)**

- **Ansible ou Terraform :** Indiquez dans votre conclusion que l'étape suivante de ce projet serait de déployer les VMs et les configurations réseau non pas via l'interface web (clics manuels), mais via des scripts d'automatisation. C'est la compétence système la plus recherchée sur le marché du travail actuel.

Ansible ou Terraform : Indiquez dans votre conclusion que l'étape suivante de ce projet serait de déployer les VMs et les configurations réseau non pas via l'interface web (clics manuels), mais via des scripts d'automatisation. C'est la compétence système la plus recherchée sur le marché du travail actuel.

OPNsens 
