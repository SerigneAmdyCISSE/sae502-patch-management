# sae502-patch-management
Supervision et gestion automatisée des correctifs de sécurité Ubuntu avec Ansible et Docker Compose — SAÉ 5.02.

## Environnement de laboratoire
- **Hyperviseur :** VMware (et non VirtualBox)
- **Ressources :** 2 Machines Virtuelles Ubuntu Server 24.04 LTS (2 vCPU, 2 Go de RAM / 20 Go Disque par VM)
- **Réseau :** 
  - `vm1-control` : 192.168.75.10 (Contrôleur Docker/Ansible)
  - `vm2-target` : 192.168.75.11 (Cible)
