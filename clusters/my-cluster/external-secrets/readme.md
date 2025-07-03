# External-Secrets-Operator

Bereitstellung von External-Secrets-Operator zum lesen und Erstellen der Secrets aus Bitwarden/Vaultwarden

## Voraussetzung

Es muss manuell ein Secret angelegt werden zur Verbindung zu Bitwarden

``` 
apiVersion: v1
kind: Secret
metadata:
  name: bitwarden-cli
  namespace: external-secrets
type: Opaque
data:
  BW_HOST: https://vaultwarden.slix-cloud.de
  BW_USERNAME: ...
  BW_PASSWORD: ....

```
Und ein Secret zur Verbindung zur Dockerregistry (Optional)