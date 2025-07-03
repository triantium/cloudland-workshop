# Monitoring mit Prometheus, Grafana und Loki

## Installation
```
kubectl create ns monitoring

helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo add stable https://charts.helm.sh/stable
helm repo update

helm upgrade --install slixmonitoring -n monitoring prometheus-community/kube-prometheus-stack
```
## Konfiguration

https://github.com/prometheus-operator/kube-prometheus

### Prometheus Server für Services installieren

kubectl apply -f service-prometheus-app.yaml

### Datasource bei Grafana hinzufügen

Auslesen der Configmap

kubectl get configmap slixmonitoring-kube-promet-grafana-datasource -n monitoring -o yaml

Diese Configmap erweitern und unter datasource.yaml eine weitere Datasource hinzufügen mir default:false und einem anderen Namen. Die Url ist der Name des Services der beim erstellen des Prometheusservers angelegt wird (service-prometheus-app.yaml)

Dann Grafanadeployment neu starten

kubectl rollout restart deployment slixmonitoring-grafana -n monitoring

### Dashboard bei Grafana hinzufügen

Dashboard im Netz suchen (Grafana-Labs) und das JSON-File herunterladen
Dann ein File analog den bestehenden erstellen und die ConfigMap erstellen

kubectl apply -f prometheus-keycloak-dashboard.yaml -n monitoring

Dann Grafana neu starten

## Logmonitoring Loki

Loki und Promtail installieren
```
helm repo add grafana https://grafana.github.io/helm-charts
helm repo update
helm upgrade --install -n monitoring loki grafana/loki-stack -f loki-values.yaml
```
## Abfrage und Verwenden des Logmonitorings

CheatSheet zur Abfrage:

https://megamorf.gitlab.io/cheat-sheets/loki/


## Zugriff Dashboards

```
kubectl port-forward service/slixmonitoring-grafana -n monitoring 8081:80
```

Default-Credentials:
admin:prom-operator

