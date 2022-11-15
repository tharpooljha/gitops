# DevOps with GitOps
GitOps rep used to deploy in-cluster services and products to Kubernetes clusters. This is an opinionated file structure on how argocd should be setup.

- Update `apps/argocd/{dev,prod}/argocd-secret.yaml` with base64 with `admin:password`
- Update `apps/argocd/{dev,prod}/repository.yaml` with gitlab/github api key

## Install ArgoCD in dev cluster

```
kubectl apply -k argocd/dev
```
## Install ArgoCD in prod cluster

```
kubectl apply -k argocd/prod
```


## Connect to Private Container Registry

This will create a kubernetes secret that will allow us to pull docker and helm charts from artifactory into our cluster.
```
kubectl create secret docker-registry regcred \
--docker-server=https://artifactory \
--docker-username=read-only \
--docker-password=artifactory_apikey \
--docker-email=tharpool@jhacorp.com \
-n dev
```
We can then get the contents of this file in declarative form.
`kubectl get secret regcred -dev -o yaml`
We can output the contents of this file into our devops folder.

