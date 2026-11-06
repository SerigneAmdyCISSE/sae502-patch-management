
# SAÉ 5.02 — Patch Management Automatisé

**Étudiant :** Serigne Amdy CISSE  
**Projet :** Automatisation et contrôle des correctifs de sécurité sur Ubuntu avec Ansible et Docker Compose  
**Date de soutenance prévue :** 15 décembre 2026  

---

## 1. Contexte et Objectifs

Ce projet vise à automatiser l'application stricte et tracée des correctifs de sécurité sur un parc de serveurs Ubuntu. Afin de garantir un environnement d'exécution isolé, reproductible et sécurisé, le moteur Ansible n'est pas installé sur la machine hôte, mais s'exécute exclusivement à l'intérieur d'un conteneur Docker orchestré par Docker Compose.

Les fonctionnalités prioritaires (socle de référence) démontrées dans ce dépôt incluent :
* L'exécution conteneurisée des tâches d'administration.
* La vérification des préconditions (disponibilité, espace disque, intégrité du gestionnaire de paquets).
* L'identification et la simulation exclusive des correctifs de sécurité (filtrage strict et respect des exclusions).
* La résilience aux pannes réseau avec la gestion explicite des cibles inaccessibles (`unreachable`) et la génération de rapports d'erreurs locaux.
* L'absence de redémarrage automatique post-mise à jour.

---

## 2. Environnement de Laboratoire

Le laboratoire est déployé sur l'hyperviseur **VMware** (Workstation/Player) et repose sur deux machines virtuelles fonctionnant sous Ubuntu Server 24.04 LTS.

### 2.1. Ressources Matérielles

| Rôle | Nom d'hôte | vCPU | Mémoire Vive (RAM) | Stockage (Disque) |
|---|---|:---:|:---:|:---:|
| **Contrôleur** | `vm1-control` | 2 | 2 Go | 20 Go |
| **Cible** | `vm2-target` | 2 | 2 Go | 20 Go |

### 2.2. Architecture Réseau

Chaque machine dispose d'une double interface réseau pour séparer le trafic d'administration du trafic de téléchargement des paquets :
1. **Interface NAT :** Accès à Internet et aux dépôts logiciels Ubuntu.
2. **Interface Host-Only (Réseau privé) :** Dédiée aux communications SSH sécurisées par clés.

| Machine | Adresse IP (Privée) | Utilisateur Ansible | Authentification |
|---|---|---|---|
| `vm1-control` | `192.168.75.10` | `adminlab` | Clé Ed25519 (Générateur) |
| `vm2-target` | `192.168.75.11` | `adminlab` | Clé Ed25519 (Autorisée) |

---

## 3. Structure du Dépôt

```text
sae502-patch-management/
├── Dockerfile                  # Image Ubuntu allégée avec Ansible et client SSH
├── compose.yaml                # Orchestration du conteneur et montage des volumes/clés
├── README.md                   # Documentation principale du projet
├── inventory/
│   └── hosts.yml               # Inventaire dynamique des cibles et variables de connexion
├── playbooks/
│   ├── test_connection.yml     # Playbook de validation SSH
│   └── test_unreachable_report.yml # Playbook gérant la cible inaccessible et le reporting
├── docs/
│   ├── complement-professeur.md # Planification des heures et compléments CDC
│   └── tests/                  # Traces d'exécution brutes des essais (PoC)
└── reports/                    # Rapports Markdown générés automatiquement par Ansible
