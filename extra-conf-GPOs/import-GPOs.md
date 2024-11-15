# How to import GPOs

I've uploaded for you my working GPOs based on French Orgz ANSSI and based on the book written by Andrei Miroshnikov - Windows Security Monitoring - Scenarios and Patterns

# Logging GPO

You can find the Windows Documentation, here: https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-10/security/threat-protection/auditing/advanced-security-auditing-faq

Il y a 3 GPOs concernant la journalisation, la première nous assurant que le service de journalisation est démarré et ne pouvant être arrêté.
La deuxième et troisième est l'activation de la journalisation avancée. 
Une GPO à destination des controleurs de domaine. Une GPO à destinations des ordinateurs/serveurs.

Je me suis basé en partie sur le guide de l'ANSSI mais aussi sur le livre Windows Security Monitoring - Scenarios and Patterns de Andrei Miroshnikov.

## Forcer l'audit avancé

On configure la GPO afin de forcer les paramètres d'audit avancé et ce même ci les paramètres standards sont activés.


- `Configuration Ordinateur > Stratégies > Paramètres Windows > Paramètres de sécurité > Stratégie Locale > Option de sécurités > Audit: force les paramètres de sous catégories de stratégie d'audit (Activé)`

## Configuration avancée de l'audit

Dans cette partie de la GPO, nous allons donc activer pour chaque élément, le succès ou l'échèc de journalisation.

> Veuillez définir les paramètres dans une GPO séparé  en fonction du scope d'application (une a destination des DC et une à destination des ordinateurs/serveurs) 

- `Configuration Ordinateur > Stratégies > Paramètres Windows > Paramètres de sécurité > Configuration avancée de la stratégies d'audit > Stratégies d'audit`
  
### **Connexion de compte**

Cette catégorie contient les protocoles de la famille LAN Manager (LM, NTLM, NTLMv2) et Kerberos.

| Sous-catégorie | Event ID | Description | Ordinateurs | Serveurs | Controleur de domaine |
| ----- | ------ | ------ | ------ | ------ | ------ |
| Auditer la validation des informations d'identification | Surveille l'évènement ID 4774, 4775, 4776 et 4777 | Ces évènements sont générés seulement sur l'hôte qui stocke les mots de passe. | Succès et Echec | Succès et Echec | Succès et Echec |
| Auditer le service d'authentification Kerberos | Surveille les évènements ID 4768, 4771 et 4772 | Fournis les infos détaillées sur requêtes AS_REQ pour l'acquisition de TGTs (Ticket-granting Tickets). Seul les Active Directory gèrent ces requêtes. | Pas d'Audit | Pas d'Audit | Succès et Echec |
| Auditer les opérations de ticket du service Kerberos | Surveille les évènements ID 4769, 4770 et 4773 |  Fourni les infos sur les requêtes TGS_REQ et AP_REQ envoyés au DC, utile à des fin de résolution de problème ou info sur les accès compte AD. N'apporte pas de valeur pour les réponses à incident car cette catégorie est plus souvent utilisée lors de résolution de problème et surtout car cela génère énormément d'evenment | Pas d'Audit | Pas d'Audit | Succès et Echec |
|   Auditer d'autre évènements d'ouverture de session    |  Cette sous catégorie ne génère aucun évenements     |  ------  | Pas d'audit | Pas d'audit | Pas d'audit |

### **Gestion du compte**

Cette catégorie contient les sous catégories qui affichent les activitées liées  au gestionnaire d'autorisation (AzMan), les groupes d'application, les groupes de sécurités windows, de distribution et les comptes ordinateurs et utilisateurs.

| Sous-catégorie | Event ID | Description | Ordinateurs | Serveurs | Controleur de domaine |
| ----- | ------ | ------ | ------ | ------ | ------ |
| Auditer la gestion des groupes d'applications | 4783-4792 | Utile seulement au niveau AD si le gestionnaire d'autorisation AZMan est utilisé (ce qui n'est généralement pas le cas). | Pas d'audit | Pas d'audit | Pas d'audit |
| Auditer la gestion des comptes d'ordinateur | Surveille les évènements ID 4741,4742 et 4743 | Ces évènements sont générés lors des opérations de création, de suppression ou de modification. Ils ne contiennent que des évènement Succès seulement sur les DC | Pas d'audit | Pas d'audit | Succès |
| Auditer la gestion des groupes de distribution | Surveille les évènements 4749-4753 (events ID principaux) et 4744-4748 (similaires aux 4749-4753 excepté qu'ils sont généré pour des groupes de distributions locaux) et 4759-4763 (similaires aux 4749-4753 excepté qu'ils sont généré pour des groupes de distributions universels  | Permet de surveiller la creation / supression / modification de groupe de distribution ainsi que les opération d'ajout/suppression de membre | Pas d'Audit | Pas d'Audit | Succès |
| Auditer d'autres évènement de gestion des comptes | Surveille les évènements ID 4782 et 4793 | Utile que si nous surveillons les appels de l'API Policy Checking coté serveurs.  | Pas d'Audit | Pas d'Audit | Succès |
|   Auditer la gestion des groupes de sécurité    | Surveille les évènements 4731-4735, 4764, 4799 et 4754-4758 et 4727, 4737, 4728, 4729, 4730 |  Permet de surveiller la creation / supression / modification de groupe de sécurité ainsi que les opérations d'ajout/suppression de membre  | Succès | Succès | Succès |
|   Auditer la gestion des comptes utilisateurs | Surveille les évènements 4720, 4722-4726, 4738, 4740, 4765-4767, 4780, 4781, 4794, 4798, 5376, 5377 |  Permet de surveiller les opérations locale et AD tel que (creation / supression / modification / reset pass / change pass / changement nom compte, etc..), ajout de SIDHistory, DSRM modification mot de passe, operation restauration et backup Windows Credential Manager, requete vide de mot de passe | Succès et Echec | Succès et Echec | Succès et Echec |

### **Suivi détaillé**

Cette catégoie contines les sous catégories qui fournissent les informations sur les opérations DPAPI (Data Protection API), PNP (Plug and Play), la création de process et les opérations d'extinctions. 

| Sous-catégorie | Event ID | Description | Ordinateurs | Serveurs | Controleur de domaine |
| ----- | ------ | ------ | ------ | ------ | ------ |
| Auditer l'activité DPAPI | Surveille les évènements 4692, 4693, 4694 et 4695 | Il est recommandé d'activer l'audit sur l'ensemble des types de machine. | Succès et Echec | Succès et Echec | Succès et Echec |
| Auditer l'activité Plug and Play |  Surveille les évènements 6416, 6419-6424 | Informe sur les nouveau appareil connecté au systeme, les appareil désactivé et activé et l'installation de restriction d'appareils | Succès  | Succès | Succès |
| Auditer la création du processus | Surveille les évènements 4688 et 4696 | Informe sur les processus démarré sur le systeme | Succès | Succès | Succès |
| Auditer la fin du processus | Surveille l'évènement 4689 | Informe sur les processus arrêté sur le systeme | Succès | Succès | Succès |
| Auditer les évènements RPC | Surveille l'évènement 5712, jamais utilisé ne pas activer | ------ | Pas d'audit | Pas d'audit | Pas d'audit |

### **Accès DS**

| Sous-catégorie | Event ID | Description | Ordinateurs | Serveurs | Controleur de domaine |
| ----- | ------ | ------ | ------ | ------ | ------ |
| Auditer la réplication du service d'annuaire détaillé | Surveille les évènements 4928-4931, 4934-4937 | Troubleshoot AD replication, mais pas utile a des fins de sécurité | Pas d'audit | Pas d'audit | Pas d'audit |
| Auditer l'accès au service d'annuaire | Surveille l'évènement 4661 et 4662  | Informe sur les accès objets tels que OU, utilisateur, contact, ordinateur, etc.. | Pas d'audit | Pas d'audit | Echec |
| Auditer les modifications du service d'annuaire | Surveille les évènements 5136-5139, 5141 | Informe sur la modification, creation, suppression, déplacement et l'annulation d'opération. Il ne remonte pas les accès de lecture | Pas d'audit | Pas d'audit | Succès |
| Auditer la réplication du service d'annuaire | Surveille les évènements 4932, 4933, utilisés principalement pour la résolution de problème |  Pas d'audit | Pas d'audit | Succès et Echec |


### **Ouvrir/fermer session**

| Sous-catégorie | Event ID | Description | Ordinateurs | Serveurs | Controleur de domaine |
| ----- | ------ | ------ | ------ | ------ | ------ |
| Auditer le verrouillage de compte | Surveille l'évènement 4625 | Informe quand un compte se verrouille après des tentatives de connexion  | Echec | Echec | Echec |
| Auditer les revendications utilisateur/de périphérique | Surveille l'évènement 4626  | Informe sur l'association utilisateur/ordinateur | Succès | Succès | Succès |
| Auditer l'appartenance à un groupe | Surveille l'évènement 4627  | Informe sur les groupes dont fait parti l'utilisateur qui se connecte | Succès | Succès | Succès |
| Auditer le mode étendu/princpal/rapide IPSec | Pas d'ID à surveiller | ---- | Pas d'audit | Pas d'audit | Pas d'audit |
| Auditer la fermeture de session | Surveille les évènements ID 4634, 4647 | Informe sur la fermeture de session, extinction ne log pas | Succès | Succès | Succès |
| Auditer l'ouverture de session | Surveille les évènements ID 4624, 4625, 4648, 4675 | Informe sur l'ouverture de session | Succès et Echec | Succès et Echec | Succès et Echec |
| Auditer le serveur NPS | Surveille les évènements ID 6272-6280 | Informe sur les status NPS (connexion etc) A appliquer que sur un serveur Windows NPS | Pas d'audit | Succès et Echec | Pas d'audit |
| Auditer d'autre évènements d'ouverture/fermeture de session | Surveille les évènements ID 4649, 4778-4779, 4800-4803, 5378, 5632, 5633 | Informe sur les reconection, déverouillage, ecran de veille etc et contient aussi les infos d'authentffication 802.1X  | Succès et Echec | Succès et Echec  | Succès et Echec |
| Auditer l'ouverture de session spéciale | Surveille les évènements 4964, 4672 | Informe sur les compte ajouté au groupe spéciaux qui est une option qui permet de génré des evenement supplementaire. | Succès | Succès | Succès |

### **Accès à l'objet**

| Sous-catégorie | Event ID | Description | Ordinateurs | Serveurs | Controleur de domaine |
| ----- | ------ | ------ | ------ | ------ | ------ |
| Auditer l'application générée | Surveille les évènements ID 4665-4668  | Seulement utilisé dans de rare cas  | Pas d'audit | Pas d'audit | Pas d'audit |
| Auditer les services de certification | Surveille les évènements ID 4868-4898 | 30 events ID à surveiller ! A configurer uniqueemnt pour des serveurs AD CS | Pas d'audit | Succès et Echec | Pas d'audit |
| Auditer le partage de fichiers détaillés | Surveille l'évènement 5145 | | Succès et Echec | Succès et Echec | Succès et Echec |
| Auditer le partage de fichiers | Surveille les évènements ID 5140 5142-5144, 5168 | ----- | Succès et Echec | Succès et Echec | Succès et Echec |
| Auditer le système de fichiers | Surveille les évènements ID 4656, 4658, 4660, 4663, 4664, 4985, 5051, 4670  | Par défaut, pas d'évènement généré avec le systeme de fichier SACLs  | Succès et Echec | Succès et Echec | Succès et Echec |
| Auditer la connexion de la plateforme de filtrage | Surveille les évènements 5031, 5150, 5151, 5154-5159 | Journalise quand des connexions sont autorisée ou bloquées | Succès et Echec | Succès et Echec | Succès et Echec |
| Auditer le rejet de paquet par la plateforme de filtrage | Surveille les évènements 5152 et 5153 |  | Succès et Echec | Succès et Echec | Succès et Echec |
| Auditer la manipulation de handle | Surveille les évènements 4690 et 4658 (comme pour la partie Audit system de fichier)|  | Succès et Echec | Succès et Echec  | Succès et Echec |
| Auditer l'objet de noyau | Surveille les évènements ID 4656, 4658, 4660, 4663  | Journaux généralement utile qu'aux développeurs | Pas d'audit | Pas d'audit | Pas d'audit |
| Auditer d'autre évènements d'accès à l'objet | Surveille les evenements ID 4671, 4691, 5148, 5149, 4698, 4699, 4700, 4701, 4702, 5888, 5889, 5890 | | Succès et Echec | Succès et Echec  | Succès et Echec |
| Auditer le registre | Surveille les evenements ID ID 4663, 4656, 4658, 4660, 4657, 5039, 4670 |  | Succès et Echec | Succès et Echec  | Succès et Echec |
| Auditer le stockage amovible | Surveille les evenements ID 4663, 4656, 4658 | | Succès et Echec | Succès et Echec  | Succès et Echec |
| Auditer SAM | Pas d'infos sur les evenemnt ID   | Ne pas auditer, car microsoft ne donne aucune reco | Pas d'audit | Pas d'audit | Pas d'audit |
| Auditer la stratégie d'accès centralisée intermédiaire | Surveille l'évènement 4818   | Ne pas auditer | Pas d'audit | Pas d'audit | Pas d'audit |

### **Changement de stratégie**

| Sous-catégorie | Event ID | Description | Ordinateurs | Serveurs | Controleur de domaine |
| ----- | ------ | ------ | ------ | ------ | ------ |
| Auditer la modification de stratégie d'audit |  Surveille les evenements ID (4902, 4907, 4904, 4905) et (4715, 4719, 4817, 4902, 4906, 4907, 4908, 4912, 4904, 4905)  | (Journaux si Succes activé) et (Journaux peut importe si Succes activé)  | Succès | Succès | Succès |
| Auditer la modification de stratégie d'authentification | Surveille les evenements ID 4670, 4706, 4707, 4716, 4713, 4717, 4718, 4739, 4864, 4865, 4866, 4867 | Ce paramètre est utile pour suivre les modifications apportées à la confiance et aux privilèges au niveau du domaine et de la forêt qui sont accordés aux comptes d'utilisateurs ou aux groupes.  | Succès | Succès | Succès |
| Auditer la modification de la stratégie d'autorisation | Surveille les évènements 4703, 4704, 4705, 4670, 4911, 4913| | Succès | Succès | Succès |
| Auditer la modification de la stratégie de plateforme de filtrage | ID a surveiller non utile  | Non utile pour la sécurité | Pas d'audit | Pas d'audit | Pas d'audit |
| Auditer la modification de la stratégie de niveau règle MPSSVC | Surveille les évènements 4944-4954, 4956-4958  | Les modifications apportées aux règles du pare-feu sont importantes pour comprendre l’état de sécurité de l’ordinateur et son degré de protection contre les attaques réseau | Succès et Echec | Succès et Echec | Succès et Echec |
| Auditer d'autres évènement de modification de stratégie | Surveille les évènements 4714, 4819, 4826, 4909, 4910, 5063-5070, 5447, 6144, 6145 | Permet de connaitre les modification de stratégies de la plateforme filtrage Windows, l'état des maj des paramètres de strategie de sécurité pour les GPO locales, les modification de stratégies d'accès centralisé | Succès et Echec | Succès et Echec | Succès et Echec |

### **Utilisation de privilège**

| Sous-catégorie | Event ID | Description | Ordinateurs | Serveurs | Controleur de domaine |
| ----- | ------ | ------ | ------ | ------ | ------ |
| Auditer l'utilisation de privilèges non sensibles | Surveille les évènements ID 4673, 4674 et 4985  | Microsoft ne recommande pas d'activer pour les privilege non sensible car le volume d'évènement est très élevé et ils ne sont pas aussi important que les evenement des privilèges sensible.  | Succès et Echec | Succès et Echec | Succès et Echec |
| Auditer d'autre évènements d'utilisation de privilèges | ID 4985  |  Cette catégorie de journalise aucun évènement de sécurité et n'est pas utile selon microsoft | Pas d'audit | Pas d'audit | Pas d'audit |
| Auditer l'utilisation de privilèges sensibles | Surveille les évènements 4673, 4674, 4985 | Cette partie est la plus utile et doit être activée | Succès et Echec | Succès et Echec | Succès et Echec |


### **Système**

| Sous-catégorie | Event ID | Description | Ordinateurs | Serveurs | Controleur de domaine |
| ----- | ------ | ------ | ------ | ------ | ------ |
| Auditer le pilote IPSec | Pas d'évènement ID  | Non utile car IP Sec n'est pas utilisé sur Windows  | Pas d'audit | Pas d'audit | Pas d'audit |
| Auditer d'autre évènements système | Surveilles les évènements 5024,5025, 5027-5030, 5032-5035, 5037, 5058,5059, 6400-6409 | Auditer d’autres événements système détermine si le système d’exploitation audite divers événements système (démarrage/arret du service/pilote pare-feu Windows) | Succès et Echec | Succès et Echec | Succès et Echec |
| Auditer la modification de l'état de la sécurité | Surveille les évènements 4608, 4616, 4621 | L'audit des modifications de l'état de sécurité contient les événements de démarrage, de récupération et d'arrêt de Windows, ainsi que des informations sur les modifications de l'heure système. | Succès | Succès | Succès |
| Auditer l'extension du système de sécurité | Surveille les évènements 4610,4611,4614,4622,4697 | | Succès | Succès | Succès |
| Auditer l'intégrité du système | Surveille les évènements 4612, 4615, 4618, 4816, 5038, 5056, 5062, 5057, 5060, 5061, 6281, 6410  | L'intégrité du système d'audit détermine si le système d'exploitation audite les événements qui violent l'intégrité du sous-système de sécurité. | Succès et Echec | Succès et Echec | Succès et Echec |

### Autre Evenements

| Sous-catégorie | Event ID | Description | Ordinateurs | Serveurs | Controleur de domaine |
| ----- | ------ | ------ | ------ | ------ | ------ |
| Autre Evenements | Surveilles les évènements 1100, 1102, 1104, 1105, 1108  | NLes événements de cette section sont générés automatiquement et sont activés par défaut.  | Actif par défaut | Actif par défaut | Actif par défaut |

### **Audit de l'accès global aux objets**

Non configuré.

## Augmentation taille journaux

Pour certain fichiers journaux, nous augmentons la taille de ceux-ci:

Le chemin de configuration se trouve: `Stratégies > Modèles d'administration > Composants windows > Service Journal des évènements`

Les journaux cibles sont: 

- Application: 102400
- Installation: 102400
- Sécurité: 1048576
- Système: 102400
- Windows PowerShell: 102400

### 2.2.4 Activation de certains journaux désactivé par défaut

Les chemins d'accès regedit sont:

`HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\WINEVT\Channels\`


Nous activons via GPO certains journaux avec un paramètre regedit a Enabled =1 :

- Microsoft-Windows-Authentication/AuthenticationPolicyFailures-DomainController
- Microsoft-Windows-Authentication/ProtectedUser-Client
- Microsoft-Windows-Authentication/ProtectedUserFailures-DomainController
- Microsoft-Windows-Authentication/ProtectedUserSuccesses-DomainController
- Microsoft-Windows-DriverFrameworks-UserMode/Operational
- Microsoft-Windows-PrintService/Operational
- Microsoft-Windows-DNS-Client/Operational
- Microsoft-Windows-CAPI2/Operational
- Microsoft-Windows-LSA/Operational


### 2.2.5 Augmentation taille des journaux à 20 Mo

Nous augmentons via GPO certains journaux avec un paramètre regedit a MaxSize = 0x1312D00:

- Microsoft-Windows-Application-Experience/Program-Inventory
- Microsoft-Windows-Bits-Client/Operational
- Microsoft-Windows-CodeIntegrity/Operational
- Microsoft-Windows-DeviceGuard/Operational
- Microsoft-Windows-DriverFrameworks-UserMode/Operational
- Microsoft-Windows-Kernel-PnP/Configuration
- Microsoft-Windows-NetworkProfile/Operational
- Microsoft-Windows-NTLM/Operational
- Microsoft-Windows-PowerShell/Operational
- Microsoft-Windows-PrintService/Operational
- Microsoft-Windows-SmartCard-Audit/Authentication
- Microsoft-Windows-TaskScheduler/Operational
- Microsoft-Windows-VPN-Client/Operational
- Microsoft-Windows-Win32k/Operational
- Microsoft-Windows-Windows Firewall With Advanced Security/Firewall
- Microsoft-Windows-WindowsUpdateClient/Operational



- Windows Firewall Log GPO

Configure a GPO to setting like this below (template soon)

  - Domain Profile
  <img width="532" alt="image" src="https://github.com/s0p4L1n3/Graylog_Content_Pack_Windows_Security/assets/126569468/0ec47d2e-c4c4-4516-b936-233e7f9399d6">

  - Private Profile
  <img width="532" alt="image" src="https://github.com/s0p4L1n3/Graylog_Content_Pack_Windows_Security/assets/126569468/50ab0d61-7701-44cf-8cf1-96abf9e77e3f">

 
  - Public Profile
  <img width="532" alt="image" src="https://github.com/s0p4L1n3/Graylog_Content_Pack_Windows_Security/assets/126569468/856e7269-6c5d-4fe6-aa8b-0288fd0e9d35">


