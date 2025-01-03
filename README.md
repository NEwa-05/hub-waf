# Traefik Hub WAF examples

# Deploy Hub

### Create namespace

```bash
kubectl create ns traefik
```

### Download geoip db

```bash
kubectl apply -f hub/geodb-job.yaml
```

### Create Hub token secret

```bash
kubectl create secret generic hub-license --from-literal=token="${HUB_TOKEN}" -n traefik
```

### deploy Traefik

```bash
helm upgrade --install traefik traefik/traefik --create-namespace --namespace traefik --values hub/hub-values.yaml
```

### Add Hub dashboard ingress if needed

```bash
envsubst < hub/ingress.yaml | kubectl apply -f -
```

## Deploy whoami to test

### Create apps namespace

```bash
kubectl create ns whoami
```

### Deploy whoami, middleware and ingress

```bash
envsubst < whoami/whoami.yaml | kubectl apply -f -
envsubst < whoami/middlewares.yaml | kubectl apply -f -
envsubst < whoami/ingress.yaml | kubectl apply -f -
```