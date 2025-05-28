# Metrics-Server 

## Warum ? 

  * Es wird ein API bereitgestellt, die Informationen zu den Auslastung von Pods und Nodes sammelt

## Installation 

```
helm repo add metrics-server https://kubernetes-sigs.github.io/metrics-server/
helm -n kube-system upgrade --install my-metrics-server metrics-server/metrics-server --version 3.12.2
```

  * Achtung, danach geht es nicht sofort, es dauert einen Momeent bis ich es verwenden kann (geschätzt 5 Minuten) 

## Verwendung 

```
kubectl top pods
# Pods in allen Namespaces 
kubectl top pods -A
kubectl top nodes
```
