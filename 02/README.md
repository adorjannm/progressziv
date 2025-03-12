# Második óra

- [x] Kubernetes create, delete, edit Pod
- [x] What is a Pod?
- [ ] Demo how it looks in crictl
- [x] See the Kubernetes kube-system namespace and its Pods, read about them
- [x] When the Pod is started, exec into it and list the env
- [x] Check all mounts for a Pod. What is on the mounted locations? Why are these mounted?
- [x] See how Deployment works. Create a Deployment.
- [x] Scale up and down the Deployment.
- [x] Create Service for the Deployment.
- [x] Access the application through the Service.
- [x] Create a ConfigMap. What is it?
- [x] Create a Secret. What is it?

## Pods

Legkisebb egység a Kubernetes-ben. Egy vagy több konténerből áll és közös a a tárolójuk és közös az IP-jük, egymással localhost-on kommunikálnak.

![pod.gif](pod.gif)

```bash
kubectl apply -f basic-pod.yaml
kubectl exec -it basic-pod -- env
kubectl edit pod basic-pod
kubectl delete pod basic-pod
```

### Environment variables

```bash
kubectl exec -it basic-pod -- env
```

### Mounts

```bash
kubectl describe pod basic-pod
```

```yaml
Containers:
  basic-container:
    [...]
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-zhkd2 (ro)
[...]
Volumes:
  kube-api-access-zhkd2:
    Type:                    Projected (a volume that contains injected data from multiple sources)
    TokenExpirationSeconds:  3607
    ConfigMapName:           kube-root-ca.crt
    ConfigMapOptional:       <nil>
    DownwardAPI:             true
```

```bash
kubectl exec -it basic-pod -- ls /var/run/secrets/kubernetes.io/serviceaccount
# ca.crt  namespace  token
```

- **ca.crt**: Titkosított kommunikáció a API szerverrel
- **namespace**: Milyen namespace-ben fut a Pod
- **token**: Auth az API szerverhez

### `kube-system` namespace

```bash
kubectl get pods -n kube-system
```

- **coredns**: service-ek DNS recordjai
- **etcd**: kulcs-érték tároló, a Kuberentes összes adatát tárolja
- **kube-apiserver**: a Kubernetes frontend-je
- **kube-controller-manager**: Vezérli a cluster-t
- **kube-proxy**: Kommunikáció a *külvilággal* (kluszteren kívül)
- **kube-scheduler**: Eldönti, hogy melyik Pod melyik Node-on fusson
- **storage-provisioner**: Szolgáltat tárolót a Podoknak
- **vpnkit-controller**: Docker Desktop spcifikus, hálózat hozzáférést biztosít a clusternek

## Deployments

Definiálunk egy elvárt állapotot és a `kube-controller-manager` ebbe az állapotba próbálja meg eljuttatni a clustert.

![deployment.gif](deployment.gif)

```bash
kubectl apply -f nginx-deployment.yaml
kubectl rollout status deployment/nginx-deployment
kubectl get deployments
kubectl get rs
kubectl get pods --show-labels
kubectl describe deployments
kubectl scale deployment nginx-deployment --replicas=10
kubectl rollout status deployment/nginx-deployment
kubectl scale deployment nginx-deployment --replicas=2
```

## Services

A futó appok elérhetővé tevése

![service.gif](service.gif)

```bash
kubectl apply -f nginx-service.yaml
kubectl get services
kubectl describe service nginx-service
curl http://localhost
```

## ConfigMap

Nem titkos adatok tárolására használjuk. Lehet evn variable, volume, config file vagy command line paraméter.

![configmap.gif](configmap.gif)

```bash
kubectl apply -f configmap.yaml
kubectl exec -it configmap-pod -- /bin/sh
cd /config
ls
cat game.properties
```

## Secret

Titkos adatok tárolására használjuk (pl jelszó, access token stb). Ugyanúgy használható, mint a ConfigMap.

![secret.gif](secret.gif)

```bash
kubectl apply -f secret.yaml
kubectl get secrets
kubectl exec -it secret-pod -- /bin/sh
cd /etc/secret-volume
ls -la
cat .secret-file
```
