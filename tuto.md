# Mini-Projet Kubernetes — IC GROUP

##Objectif

Déployer un environnement Kubernetes local à l’aide de Minikube dans une machine virtuelle (VMware Workstation), contenant :

- Un site vitrine Flask (ic-webapp)

- Un ERP Odoo 13 avec base PostgreSQL

- Un outil d’administration pgAdmin4

## **Prérequis**

- VM sous VMware Workstation avec Ubuntu 20.04 ou 24.04

- Accès root ou sudo

- Connexion Internet active


## **Logiciels à installer**
`
sudo apt update && sudo apt upgrade -y
`
### Installer les outils nécessaires
`
sudo apt install -y curl git docker.io
sudo usermod -aG docker $USER && newgrp docker
`
### Installer kubectl
`
curl -LO https://dl.k8s.io/release/v1.29.2/bin/linux/amd64/kubectl
chmod +x kubectl
sudo mv kubectl /usr/local/bin/kubectl
`
### Vérifier la version
`
kubectl version --client
`
### Installer Minikube
`
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube
`

### Démarrer Minikube avec le driver Docker

minikube start --driver=docker

### Vérifier que le cluster fonctionne :
`
kubectl get nodes
`

### Cloner le dépôt source de l'application Flask

`
git clone https://github.com/sadofrazer/ic-webapp.git
`

`
cd ic-webapp
`
### Structure du projet

k8s/

├── namespace.yaml

├── odoo.yaml

├── postgres.yaml

├── pgadmin.yaml

├── pgadmin-configmap.yaml

├── webapp.yaml

├── webapp-config.yaml

└── secret.yaml

# Configuration fichier .yaml
  
### namespace.yaml

![image](https://github.com/user-attachments/assets/24458149-ba5b-4f3d-8caa-bfb7b661cc91)

### odoo.yaml
![image](https://github.com/user-attachments/assets/b8999f03-5b62-45d5-9451-3ba1a2ede3a5)

![image](https://github.com/user-attachments/assets/0e1d4ce0-e8b7-47ac-9075-891630139e11)

### postgresql.yaml

![image](https://github.com/user-attachments/assets/789a75cb-782c-4c9a-bf1d-af81ec03040b)

![image](https://github.com/user-attachments/assets/70cb0897-d197-4711-ae77-600c98e21a28)

![image](https://github.com/user-attachments/assets/77419847-e2e2-47e7-9042-bfabba767750)

### pgadmin.yaml
![image](https://github.com/user-attachments/assets/eb7dd054-2735-4dd7-9b04-3b6950b4def4)
![image](https://github.com/user-attachments/assets/4e11c345-d4f6-4202-86eb-28c06e26c312)

### pgadmin-configmap.yaml
![image](https://github.com/user-attachments/assets/4eba3a7a-a22b-4af3-a7d9-dd66f79dc817)

### webapp.yaml
![image](https://github.com/user-attachments/assets/f0a8eeec-97e6-4d9d-b49a-7b0f3253508c)

### webapp-config.yaml
![image](https://github.com/user-attachments/assets/7fc04b83-bfcf-4136-bf47-7f133dd54d21)

### secret.yaml
LAncer la commande ci-dessous:

kubectl create secret generic app-secrets \
  --from-literal=pgadmin-email="votre email" \
  --from-literal=pgadmin-password="votre password" \
  --from-literal=postgres-password=odoo \
  --from-literal=odoo-password=odoo \
  --namespace=icgroup \
  --dry-run=client -o yaml > secret.yaml


### Déploiement des ressources Kubernetes 
`
kubectl apply -f k8s/namespace.yaml
`

`
kubectl apply -f secret.yaml
`

`
kubectl apply -f postgresql.yaml
`

`
kubectl apply -f odoo.yaml
`

`
kubectl apply -f pgadmin-configmap.yaml
`

`
kubectl apply -f pgadmin.yaml
`

`
kubectl apply -f webapp-config.yaml
`

`
kubectl apply -f webapp.yaml
`

### Vérification
`
kubectl get all -n icgroup
`
![image](https://github.com/user-attachments/assets/33d10bff-064f-48ee-be6d-d02905d984f5)

`
kubectl describe svc
`

![image](https://github.com/user-attachments/assets/75c377b7-3a99-4b05-a58d-fd5318d822f1)

### Accès aux services

Récupérer l’IP Minikube :
`
minikube ip
`

### Services exposés via NodePort :

Webapp

http://<minikube_ip>:30080

Odoo

http://<minikube_ip>:30069

pgAdmin

http://<minikube_ip>:30050

### Interfaces

### Webapp
![image](https://github.com/user-attachments/assets/c5537f99-0748-484d-9a36-30cfffe3c947)

### PgAdmin

![image](https://github.com/user-attachments/assets/e2214449-039c-418a-b608-a29ae21cafbe)

### Odoo
![image](https://github.com/user-attachments/assets/94bdab54-f1de-4b25-b2d9-4708c2205494)



