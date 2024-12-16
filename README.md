# Déploiement d'Apache Airflow
Ce document décrit les étapes nécessaires pour déployer Apache Airflow en utilisant Helm et Kubernetes. Il couvre la création de secrets, l'installation et la mise à jour d'Airflow, ainsi que la configuration des ressources.

## 1. Création du secret pour le Webserver
Créez un secret Kubernetes pour le Webserver d'Airflow :
```bash
kubectl create secret generic my-webserver-secret --from-literal="webserver-secret-key=$(python3 -c 'import secrets; print(secrets.token_hex(16))')" -n airflow
```
## 2. Configuration et Installation d'Airflow avec Helm
Ajouter le dépôt Helm d'Airflow :
```bash
helm repo add airflow-stable https://airflow-helm.github.io/charts
helm repo update
```
Afficher les valeurs par défaut et les sauvegarder dans un fichier :
```bash
helm show values https://airflow-helm.github.io/charts > values_stable.yaml
```
utiliser le  fichier values_stable.yaml dans ... qui contient des updates spécifiques.
Installer ou mettre à jour Airflow avec Helm :
```bash
helm upgrade --install airflow-stable airflow-stable/airflow --namespace airflow --create-namespace -f values_stable.yaml --debug --timeout 1200
```

## 4. Redémarrage des Pods
Redémarrez tous les déploiements dans le namespace airflow :
bash(corrige la commande)
kubectl rollout all deploy -n airflow
##  5. Application du Horizontal Pod Autoscaler (HPA)
Appliquez les configurations HPA pour les services :
```bash
kubectl apply -f sheduler-hpa.yaml -n airflow
kubectl apply -f triggerer-hpa.yaml -n airflow
kubectl apply -f webserver-hpa.yaml -n airflow
kubectl apply -f worker-hpa.yaml -n airflow
```
Notes

Adaptez les valeurs des PVC et des HPA selon vos besoins spécifiques.
Pour toute question ou problème, consultez la documentation officielle d'Airflow ou les issues du dépôt Helm d'Airflow.

# airflow-satck
