# Chart Mockoon 

  * Software zum Mocken von apis

## Struktur

```
mockoon/
├── Chart.yaml
├── values.yaml
└── templates/
    ├── configmap.yaml
    ├── deployment.yaml
    ├── service.yaml
    └── route.yaml
```

## `Chart.yaml`

```
apiVersion: v2
name: mockoon
description: Mockoon mock API server for OpenShift
type: application
version: 0.2.0
appVersion: "9.6.0"
```

## `values.yaml`
```yaml
image:
  repository: mockoon/cli
  tag: "9.6.0"
  pullPolicy: IfNotPresent

port: 3000

# Hot-Reload bei ConfigMap-Änderungen (kein Pod-Restart nötig)
watch: true

# Öffentliche Basis-URL für baseUrl-Templating und Callbacks
# Leer lassen = nicht setzen. Wird typisch auf die Route-URL gesetzt.
publicBaseUrl: ""

route:
  host: ""              # leer = OCP generiert <name>-<ns>.<wildcard-domain>
  termination: edge
  insecureRedirect: Redirect
  timeout: 30s

mockData: |
  {
    "uuid": "demo",
    "name": "Demo API",
    "endpointPrefix": "",
    "routes": [
      {
        "uuid": "r1",
        "method": "get",
        "endpoint": "users",
        "responses": [
          {
            "uuid": "resp1",
            "statusCode": 200,
            "headers": [{ "key": "Content-Type", "value": "application/json" }],
            "body": "{\"users\":[{\"id\":1,\"name\":\"Jochen\"}]}"
          }
        ]
      }
    ]
  }
```

## `templates/configmap.yaml`
```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: {{ .Release.Name }}-data
  labels:
    app: {{ .Release.Name }}
data:
  mock.json: |
{{ .Values.mockData | indent 4 }}
```

## `templates/deployment.yaml`
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: {{ .Release.Name }}
  labels:
    app: {{ .Release.Name }}
spec:
  replicas: 1
  selector:
    matchLabels:
      app: {{ .Release.Name }}
  template:
    metadata:
      labels:
        app: {{ .Release.Name }}
      annotations:
        # ConfigMap-Hash -> Auto-Rollout bei Mock-Änderung
        checksum/config: {{ .Values.mockData | sha256sum }}
    spec:
      # Kein runAsUser/fsGroup -> SCC restricted-v2 vergibt Random-UID
      securityContext:
        runAsNonRoot: true
        seccompProfile:
          type: RuntimeDefault
      containers:
      - name: mockoon
        image: "{{ .Values.image.repository }}:{{ .Values.image.tag }}"
        imagePullPolicy: {{ .Values.image.pullPolicy }}
        args:
          - "--data"
          - "/data/mock.json"
          - "--port"
          - "{{ .Values.port }}"
          - "--disable-log-to-file"
        env:
        - name: HOME
          value: /tmp
        ports:
        - name: http
          containerPort: {{ .Values.port }}
          protocol: TCP
        securityContext:
          allowPrivilegeEscalation: false
          readOnlyRootFilesystem: true
          capabilities:
            drop: ["ALL"]
        volumeMounts:
        - name: data
          mountPath: /data
          readOnly: true
        - name: tmp
          mountPath: /tmp
        livenessProbe:
          tcpSocket:
            port: http
          initialDelaySeconds: 5
          periodSeconds: 10
        readinessProbe:
          tcpSocket:
            port: http
          initialDelaySeconds: 2
          periodSeconds: 5
        resources:
          requests:
            cpu: 50m
            memory: 64Mi
          limits:
            cpu: 200m
            memory: 128Mi
      volumes:
      - name: data
        configMap:
          name: {{ .Release.Name }}-data
      - name: tmp
        emptyDir:
          medium: Memory
          sizeLimit: 16Mi
```

## `templates/service.yaml`
```yaml
apiVersion: v1
kind: Service
metadata:
  name: {{ .Release.Name }}
  labels:
    app: {{ .Release.Name }}
spec:
  selector:
    app: {{ .Release.Name }}
  ports:
  - name: http
    port: 80
    targetPort: http
    protocol: TCP
```

## `templates/route.yaml`
```yaml
apiVersion: route.openshift.io/v1
kind: Route
metadata:
  name: {{ .Release.Name }}
  labels:
    app: {{ .Release.Name }}
  annotations:
    haproxy.router.openshift.io/timeout: {{ .Values.route.timeout }}
spec:
  {{- with .Values.route.host }}
  host: {{ . }}
  {{- end }}
  to:
    kind: Service
    name: {{ .Release.Name }}
    weight: 100
  port:
    targetPort: http
  {{- if ne .Values.route.termination "" }}
  tls:
    termination: {{ .Values.route.termination }}
    {{- if eq .Values.route.termination "edge" }}
    insecureEdgeTerminationPolicy: {{ .Values.route.insecureRedirect }}
    {{- end }}
  {{- end }}
  wildcardPolicy: None
```

## Deploy & Test

```bash
oc new-project mockoon-demo
helm install demo-mock ./mockoon

# Route-URL holen
URL=$(oc get route demo-mock -o jsonpath='{.spec.host}')
curl https://$URL/users
```

## Override-Beispiele

```bash
# Eigener Hostname
helm install demo-mock ./mockoon \
  --set route.host=mock.apps.mycluster.example.com

# Passthrough (TLS endet im Pod, dann brauchst du im Mockoon TLS aktiv)
helm install demo-mock ./mockoon \
  --set route.termination=passthrough

# Eigene Mock-Daten aus Datei
helm install demo-mock ./mockoon \
  --set-file mockData=./my-environment.json
```

Letzteres ist für Trainings besonders praktisch – Teilnehmer exportieren ihre Mockoon-Desktop-JSON und deployen direkt damit.
