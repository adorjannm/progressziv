# Első óra

Docker: `run`, `stop`, `build`, `exec`, `update`, `--volume`, `--network`, `rm`, `--port`

Kubernetes: `kubectl` + Hellow World, vmi UI (`k9s`, `Lens`)

## Docker

[Project](https://github.com/DevAbdurR/Simple-Calulator?tab=readme-ov-file)

![docker-commands](./docker.gif)

`--volume`: perzisztens adattárolás - ne vesszzen el adat akkor se, ha a containert töröljük

`--network`: privát hálózat - containerek egy hálótzatban tudnak egymással kommunikálni, de kívülről nem érhetőek el

`update`: futás közben tudjuk modosítani, hogy mennyi CPU-t vagy memóriát használhat a container anélkül, hogy le kellene állítani

```bash
docker build -t 01 .
docker run --name calc -p 3000:3000 -d 01
docker ps
docker exec calc ls
docker stop calc
docker ps
docker rm calc
```

## Kubernetes

![kuberentes commands](./kubernets.gif)

```bash
kubectl create deployment hello-node --image=registry.k8s.io/e2e-test-images/agnhost:2.39 -- /agnhost netexec --http-port=8080
kubectl get deployments
kubectl get pods
kubectl get events
kubectl logs hello-node-55fdcd95bf-7fwb8
kubectl expose deployment hello-node --type=LoadBalancer --port=8080
kubectl get services

kubectl delete service hello-node
kubectl delete deployment hello-node
```
