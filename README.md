# Cluster Docker Swarm

## Description
Mise en place d'un cluster Docker Swarm multi-noeuds avec deploiement d'une application distribuee.
Projet realise dans le cadre d'un challenge technique pour l'admission en B3 Developpement.

## Architecture
- 1 noeud Manager : orchestre le cluster, heberge Redis et Portainer
- 2 noeuds Workers : executent les replicas du serveur web Nginx
- Reseau overlay : permet la communication entre les services sur les differents noeuds

## Stack deployee
| Service | Image | Replicas | Role |
|---------|-------|----------|------|
| web | nginx:alpine | 3 | Serveur web distribue sur les workers |
| redis | redis:alpine | 1 | Base de donnees en memoire sur le manager |
| portainer | portainer-ce | 1 | Interface de monitoring du cluster |

## Technologies utilisees
- Ubuntu Server 24.04.4 LTS (arm64)
- Docker Engine 29.4.0
- Docker Swarm (orchestration)
- UTM / QEMU (virtualisation sur Mac M1)
- SSH (administration a distance)

## Infrastructure
| Machine | Role | IP |
|---------|------|----|
| manager | Manager (Leader) | 192.168.64.4 |
| worker1 | Worker | 192.168.64.5 |
| worker2 | Worker | 192.168.64.6 |

## Fonctionnalites demontrees
1. Deploiement distribue : 3 replicas de Nginx repartis automatiquement sur les workers
2. Scaling : modification du nombre de replicas en une commande
3. Tolerance aux pannes : arret d'un worker, redistribution automatique des containers
4. Rolling update : mise a jour de Nginx sans interruption de service
5. Monitoring : visualisation du cluster en temps reel via Portainer

## Commandes principales
- Initialiser le Swarm : docker swarm init --advertise-addr 192.168.64.4
- Rejoindre le Swarm : docker swarm join --token TOKEN 192.168.64.4:2377
- Deployer la stack : docker stack deploy -c docker-compose.yml myapp
- Verifier le cluster : docker node ls / docker service ls
- Scaling : docker service scale myapp_web=6
- Rolling update : docker service update --image nginx:latest myapp_web

## Acces
| Service | URL | Identifiants |
|---------|-----|--------------|
| Nginx | http://192.168.64.4 | - |
| Nginx | http://192.168.64.5 | - |
| Nginx | http://192.168.64.6 | - |
| Portainer | http://192.168.64.4:9000 | admin / adminadmin12 |

## Auteur
Aristide Portal-Hadancourt - Avril 2026
