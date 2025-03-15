# Negyedik óra

- [x] Install Kubernetes dashboard to Kubernetes.
- [x] The app you created in the previous session, create a Helm chart for it.
- [x] Then release your app 5 times using your new Helm chart.
- [x] You don't have to upload the Helm chart to a repository, you just have to package it, because you can install TAR.GZ charts from local file path as well.

Kérdés: Kell-e a `---` a templatek végére?

## Kubernetes dashboard

```bash
helm repo add kubernetes-dashboard https://kubernetes.github.io/dashboard/
helm install kubernetes-dashboard kubernetes-dashboard/kubernetes-dashboard
```

## Helm chart

Override-ok:

```yaml
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

### Archive

```bash
tar -czf chart.tar.gz chart/
```

### Install

```bash
helm upgarde --install my-chart chart.tar.gz --values values.yaml
```
