## Build Backend Services
* mvn clean install
* docker build -t \<Image Name\> .
## Run Minikube server (Install Minikube based on the Machine specs)
* minikube start
* eval $(minikube -p minikube docker-env)
* minikube image load \<Image Name\>

## Steps To enable Monitoring tools:
We have used seperate namespace called `monitoring` to run all the monitoring applications
```kubectl create namespace monitoring```

## To Start Promotheues:
```kubectl apply -f monitoring/prometheus/prometheus-pv.yaml
kubectl apply -f monitoring/prometheus/prometheus-pvc.yaml
kubectl apply -f monitoring/prometheus/prometheus-clusterrole.yaml
kubectl apply -f monitoring/prometheus/prometheus-configmap.yaml
kubectl apply -f monitoring/prometheus/prometheus-service.yaml
kubectl apply -f monitoring/prometheus/prometheus-deployment.yaml
kubectl port-forward service/prometheus-service 9090:8080 -n monitoring
```

## To Start collecting Kube State Metrics:
```
kubectl apply -f monitoring/kube-state-metrics-config
kubectl get deployments kube-state-metrics -n kube-system
```

## To start Alert manager:
```
kubectl apply -f monitoring/alertmanager/alertmanager-configmap.yaml
kubectl apply -f monitoring/alertmanager/alertmanager-template.yaml
kubectl apply -f monitoring/alertmanager/alertmanager-deployment.yaml
kubectl apply -f monitoring/alertmanager/alertmanager-service.yaml
kubectl port-forward service/alertmanager-service 9093:9093 -n monitoring
 ```

## Grafana:
```
kubectl apply -f monitoring/grafana/grafana-datasource-config.yaml
kubectl apply -f monitoring/grafana/grafana-deployment.yaml
kubectl apply -f monitoring/grafana/grafana-service.yaml
kubectl port-forward service/alertmanager-service 3000:3000 -n monitoring  
```  
 

## TO delete all the minikube resources
minikube delete --all
