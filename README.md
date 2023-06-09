# Simplon - Brief 10

## Objectif

Mettre en oeuvre une solution de monitoring complète pour l’application Azure Vote App

- Récupérer les metrics des containers 
- Afficher les données collectées

Outils : Grafana et Prometheus.

## Installation
### Déployer un Cluster AKS 

`az aks create -g <resource-group> -n myCluster --enable-managed-identity --node-count 2`

### Installation de Prometheus et Grafana via Helm

1. `helm repo add prometheus-community https://prometheus-community.github.io/helm-charts`
2. `helm repo update`
4. `kubectl create ns monitoring`
5. `helm install prometheus promtheus-community/kube-promtheus-stack -n monitoring`
6. `kubectl get all -n monitoring`

### Accéder à Grafana

1. Rediriger les ports avec `kubectl port-forward -n monitoring prometheus-grafana-<pod-id> 3000`
2. Se rendre à `http://localhost:3000`
3. Se connecter avec `"admin" / "prom-operator"`

### Déploiement de l'infrastructure à monitorer

1. `kubectl create ns votingapp`
2. `kubectl create secret generic redis-credentials --from-literal=username=mydbusername --from-literal=password=myinsanepassword --namespace=votingapp`
3. `kubectl apply -f ./infra.yml --namespace=votingapp`

## Démonstration

### Dashboard Redis
![](https://github.com/DevSoleo/simplon-brief-10/blob/main/redis-dashboard.png)

### Dashboard Voting App
![](https://github.com/DevSoleo/simplon-brief-10/blob/main/votingapp-dashboard.png)

### Alertes Redis
![](https://github.com/DevSoleo/simplon-brief-10/blob/main/redis-alert-cpu.png)
![](https://github.com/DevSoleo/simplon-brief-10/blob/main/redis-alert-memory.png)
