# kubernetes-zero-to-hero

My hands-on Kubernetes learning notes. Work in progress.

## What's here so far

| File | What it does |
|---|---|
| `nginx-deployment.yaml` | Deployment running 1 replica of `nginx:1.24` (container port 80) |
| `nginx-service.yaml` | Service that selects pods with label `app: nginx` and forwards port 80 to the pod |

> **Note:** `nginx-service.yaml` uses `targetPort: 5000`, but the nginx container listens on port 80. For this nginx setup, `targetPort` should be `80`, otherwise requests won't reach the app. If your app listens on 5000, set `containerPort: 5000` in the deployment as well.

## Try it

```bash
minikube start
kubectl apply -f nginx-deployment.yaml
kubectl apply -f nginx-service.yaml

kubectl get pods
kubectl get svc
kubectl get endpoints nginx-service

kubectl port-forward svc/nginx-service 8080:80   # then open http://localhost:8080
```

## Commands I'm using

```bash
kubectl apply -f <file>                          # create/update from YAML
kubectl get pods|deployments|svc                 # list resources
kubectl describe pod <name>                      # details + events
kubectl logs <pod>                               # app logs
kubectl exec -it <pod> -- sh                     # shell into a pod
kubectl scale deployment <name> --replicas=3     # scale
kubectl set image deployment/<name> nginx=nginx:1.25   # update image
kubectl rollout status|undo deployment/<name>    # watch / roll back
kubectl delete -f <file>                         # clean up
```

## Key idea

`Service port` -> receives traffic. `targetPort` -> the port the app listens on inside the pod. The Service finds pods using the label **selector**, not by IP.

## Roadmap

- [x] Deployment
- [x] Service
- [x] ConfigMaps
- [ ] Secrets
- [ ] Ingress
