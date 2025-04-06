# Flux CD

``` bash
flux bootstrap github --token-auth --owner=adorjannm --repository=progressziv --branch=master --path=clusters/docker-desktop --personal
```

## Helm repo

Múlt órai chart-ot helm repoként

``` yaml
apiVersion: source.toolkit.fluxcd.io/v1beta1
kind: HelmRepository
metadata:
  name: 5-site-repo
  namespace: default
spec:
  interval: 1m
  url: https://raw.githubusercontent.com/adorjannm/progressziv/refs/heads/master/04
```

## Helm release

Az előzőleg létrehozott helm repo-ból az egyetlen chartot telepítjük ugyanolyan valuekkal, mint az előző órán a [values.yaml](../04/values.yaml)

``` yaml
apiVersion: helm.toolkit.fluxcd.io/v2beta1
kind: HelmRelease
metadata:
  name: 5-site-release
  namespace: default
spec:
  interval: 1m
  chart:
    spec:
      chart: static-site
      sourceRef:
        kind: HelmRepository
        name: 5-site-repo
      interval: 1m
  values:
    internalPort: 80
    externalPort: 80

    replicas: 2

    sites:
      - name: site-1
        id: 1
        content: |
          <h1>Site 1</h1>
      - name: site-2
        id: 2
        content: |
          <h1>Site 2</h1>
      - name: site-3
        id: 3
        content: |
          <h1>Site 3</h1>
      - name: site-4
        id: 4
        content: |
          <h1>Site 4</h1>
      - name: site-5
        id: 5
        content: |
          <h1>Site 5</h1>
```
