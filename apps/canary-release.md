---
runme:
  id: 01JCKXW5F427V95G5TBC5F4Z4X
  version: v3
---

# Demo Flagger

## Canary Releases

Canary Releases bei neuer Version Image z.B.

1. Anpassen der Imageversion

```

yq e '.images[0].newTag="6.1.1"' -i ./backend/kustomization.yaml

```

2. Änderungen einchecken

```
git add -A && \
git commit -m "backend 6.1.1" && \
git push origin main
```

3. Reconcile durchführen

```
flux reconcile source git flux-system
```

4. Flux Reconcile checken

```
watch flux get kustomizations
```

5. Canary-Events anzeigen

```
kubectl -n prod describe canary backend
```

## A/B-Testing

Canary Releases bei neuer Version Image z.B.

1. Anpassen der Imageversion

```

yq e '.images[0].newTag="6.1.1"' -i ./frontend/kustomization.yaml

```

2. Änderungen einchecken

```
git add -A && \
git commit -m "frontend 6.1.1" && \
git push origin main
```

3. Reconcile durchführen

```
flux reconcile source git flux-system
```

4. Flux Reconcle checken

```
watch flux get kustomizations
```

5. Canary-Events anzeigen

```
kubectl -n prod describe canary frontend
```

## Automated Rollback

Warten bei Änderung der Version

1. Anpassen des Gatings auf halt

```
yq e '.images[0].newTag="6.1.2"' -i ./frontend/kustomization.yaml

```

2. Änderungen einchecken

```
git add -A && \
git commit -m "Bump frontend to 6.1.2" && \
git push origin main
```

3. Reconcile durchführen

```
flux reconcile source git flux-system
```

4. Flux Reconcile checken

```
watch flux get kustomizations
```

5. Canary-Events anzeigen

```
kubectl -n prod describe canary backend
```

6. Fehler produzieren
```
watch curl -b 'type=insider' http://frontend.cloudland.com/status/500
```
oder Latenz generieren
```
watch curl -b 'type=insider' http://frontend.cloudland.com/delay/1
```

## Manual Gating

Warten bei Änderung der Version

1. Anpassen des Gatings auf halt

```
yq e '.spec.analysis.webhooks[0].url="http://flagger-loadtester.prod.svc.cluster.local/gate/halt"' -i ./backend/canary.yaml

```

2. Änderungen einchecken

```
git add -A && \
git commit -m "chg canary to halt" && \
git push origin main
```

3. Reconcile durchführen

```
flux reconcile source git flux-system
```

4. Flux Reconcile checken

```
watch flux get kustomizations
```

5. Canary-Events anzeigen

```
kubectl -n prod describe canary backend
```
