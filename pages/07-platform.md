---
layout: center
---

## PLATFORM

<br>
<br>

<div v-click>
<span v-mark="{ at: 1, color: 'red', type: 'circle' }">

## ...for Devs and Ops!...

</span>
</div>

 
---
transition: slide-left
layout: two-cols
---
# Grafana

- Grafana boards as config maps




::right::

## Config

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: grafana-dashboard
  namespace: monitoring
  labels:
    grafana_dashboard: "1"
data:
# Dashboard exported from grafana  
  simple-dashboard.json: |
    {
      "id": null,
      "title": "Simple Dashboard",
      "tags": [],
      "timezone": "browser",
      "schemaVersion": 16,
      "version": 1,
      "refresh": "5s",
      "panels": [
        {
          "type": "graph",
          "title": "CPU Usage",
            ....
```


---
transition: slide-left
layout: two-cols
---
# Monitoring

- Prometheus service scraper


::right::

## Config

```yaml
apiVersion: monitoring.coreos.com/v1
kind: ServiceMonitor
metadata:
  name: my-app-scrape
  namespace: monitoring
  labels:
    app: my-app
spec:
  selector:
    matchLabels:
      app: my-app
  endpoints:
    - port: http
      interval: 30s
      path: /metrics
      scheme: http
```

