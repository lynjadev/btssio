# EXEMPLE DE FICHE DE SITUATION PROFESSIONNELLE COMPLÉTÉE — BLOC 1 (ÉPREUVE E5)

## 1. IDENTIFICATION GÉNÉRALE
- **Titre de la réalisation :** Déploiement d'un agent de supervision et inventaire GLPI avec remontée automatisée
- **Cadre de réalisation :** [x] Stage 1  [ ] Stage 2  [ ] Atelier de professionnalisation  [ ] Projet de cours
- **Période de réalisation :** Mai 2025
- **Modalité :** [ ] Individuel  [x] En équipe (binôme avec l'administrateur système tuteur)
- **Localisation / Organisation cliente :** Clinique Sainte-Anne (Montpellier)

## 2. CONTEXTE ET OBJECTIFS
- **Contexte organisationnel :** Établissement de santé privé disposant d'un parc hétérogène de 140 postes de travail (Windows 10/11) et de 12 serveurs virtualisés sous Proxmox VE.
- **Problématique / Besoin exprimé :** Le suivi du matériel et des licences s'effectuait manuellement via un tableur obsolète, entraînant des retards dans le renouvellement des postes et l'impossibilité de tracer les versions logicielles vulnérables.
- **Objectifs fixés :** Automatiser la collecte de l'inventaire matériel et logiciel à 100 % sur le réseau interne et remonter les alertes sur le serveur GLPI déjà en place.

## 3. DÉMARCHE ET ENVIRONNEMENT TECHNIQUE
- **Environnement technologique mobilisé :**
  - *Systèmes d'exploitation :* Serveur Debian 12 (hébergeant GLPI 10.0), clients Windows 10/11 Pro
  - *Logiciels et outils :* GLPI Agent v1.7, Active Directory (GPO), PowerShell, MariaDB
  - *Infrastructure :* Contrôleur de domaine Windows Server 2022
- **Démarche suivie étape par étape :**
  1. *Préparation et tests en bac à sable :* Installation manuelle de l'agent GLPI sur un PC test et un serveur de recette pour vérifier la communication chiffrée (HTTPS) avec le serveur GLPI.
  2. *Automatisation du déploiement :* Écriture d'un script de déploiement silencieux (`.bat` / PowerShell) appelant l'installeur `.msi` avec les arguments de configuration requis (URL serveur, tag site, fréquence).
  3. *Mise en production par GPO :* Création et liaison d'un objet de stratégie de groupe (GPO) au niveau de l'OU « Postes » pour appliquer l'installation au démarrage machine.
  4. *Recette et contrôle :* Vérification dans la console GLPI de l'apparition progressive des postes et des données matérielles (RAM, CPU, numéro de série).
- **Gestion des imprévus / Incidents rencontrés :**
  - *Problème :* Le pare-feu local des postes bloquait les requêtes directes envoyées par le serveur pour forcer un inventaire immédiat.
  - *Solution apportée :* Ajout d'une règle entrante spécifique sur le port TCP 62354 via la même GPO pour autoriser le flux venant exclusivement de l'IP du serveur GLPI.

## 4. RÔLE ET CONTRIBUTION PERSONNELLE
- **Responsabilité précise :** En charge de la rédaction du script d'installation, du paramétrage de l'agent sur les postes cibles et de la validation des remontées d'inventaire.
- **Actions menées individuellement :**
  - Paramétrage des arguments de la ligne de commande de l'agent GLPI.
  - Déploiement pilote sur un échantillon de 10 postes du service administratif.
  - Rédaction complète de la documentation technique à destination de l'équipe support.

## 5. LIVRABLES ET PREUVES ASSOCIÉES
- Procédure technique : *Guide d'installation et de dépannage de l'agent GLPI (PDF)*
- Script de déploiement commenté : `install_glpi_agent.ps1`
- Schéma du flux réseau : diagramme d'architecture précisant les flux (HTTPS / port 62354) entre postes et serveur
- Capture d'écran GLPI : extrait anonymisé de la liste des postes inventoriés avec succès
- Cahier de recette : tableau de validation des tests d'inventaire sur l'échantillon pilote

## 6. COMPÉTENCES DU BLOC 1 MOBILISÉES
- [x] **B1.1 — Gérer le patrimoine informatique :** Mise en place d'un recensement automatisé des actifs matériels et logiciels pour fiabiliser le suivi du parc.
- [ ] **B1.2 — Répondre aux incidents et demandes d'assistance**
- [ ] **B1.3 — Développer la présence en ligne**
- [x] **B1.4 — Travailler en mode projet :** Respect du calendrier de déploiement, phasage test/pilote/généralisation et points d'avancement hebdomadaires.
- [x] **B1.5 — Mettre à disposition des utilisateurs un service informatique :** Déploiement transparent sans coupure de service pour les utilisateurs finaux.
- [ ] **B1.6 — Organiser son développement professionnel**

## 7. BILAN RÉFLEXIF ET AUTO-ÉVALUATION
- **Points forts :** Déploiement silencieux sans impact sur l'activité des utilisateurs ; couverture de 98 % du parc atteint en moins de deux semaines.
- **Axes d'amélioration :** Mettre en place un script de détection automatique des postes hors ligne depuis plus de 30 jours pour archiver automatiquement les fiches dans GLPI.
- **Compétences professionnelles consolidées :** Maîtrise des déploiements par GPO Active Directory et assimilation concrète des enjeux de la gestion des actifs informatiques (ITAM).