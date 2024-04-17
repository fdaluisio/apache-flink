# How to create flink gitops from flink kubernetes operator

## Clone Operator and find latest release tag
```bash
git clone https://github.com/apache/flink-kubernetes-operator.git
git tag   
```
Actual latest tag release is release-1.8.0

```bash
git checkout release-1.8.0
```

## Copy helm part of project in another directory 

```bash
cp ~/progetti/flink-kubernetes-operator/helm/flink-kubernetes-operator ~/progetti/apache-flink/
```
OR 

```bash
helm repo add ....
helm search repo flink
helm pull flink-kubernetes-operator/flink-operator --version v1.18.0
```

## Renderize helm chart and split

```bash
cd ~/progetti/apache-flink/flink-kubernetes-operator/
```
Now its time for you to customize in values.yaml your installation of operator

```bash
mkdir -p ~/progetti/apache-flink/flink-operator/resources
helm template . -f values.yaml > ~/progetti/apache-flink/flink-operator/out.yaml
cd ~/progetti/apache-flink/flink-operator/resources/
csplit -z ../out.yaml --prefix=flink-operator-  -b %2d.yaml /---/ '{*}'
```
## Apply Apache flink operator

```bash
cd ~/progetti/apache-flink/flink-operator/
kubectl create namespace flink
kubectl -n flink apply -f resources/
```
## Apply Istance of flink

```bash
cd ~/progetti/apache-flink/flink-istance/ 
kubectl -n flink apply -k overlay/dev
```

