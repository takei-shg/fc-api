# fc-api
Factchain API Server

# commands

```
# for OSX
brew install go
brew install dep
(docker for mac with kubernetes)
(Google Cloud SDK)

gcloud components update
gcloud components update kubectl

# install dependencies
dep ensure

# create docker image
GCP_PROJECT=$(gcloud config get-value project)
docker build -f Dockerfile.pingpong -t asia.gcr.io/$GCP_PROJECT/pingpong:1.0 .
gcloud auth configure-docker
docker push asia.gcr.io/try-go-211817/pingpong:1.0

# setup GKE
kubectl create -f deploy.yaml
kubectl create -f service.yaml

## by hand
gcloud container clusters create --num-nodes=2 try-go-cluster \
--zone asia-northeast1-a \
--machine-type g1-small \
--enable-autoscaling --min-nodes=2 --max-nodes=5

kubectl run try-go-deploy \
--image=asia.gcr.io/$GCP_PROJECT/pingpong:1.0 \
--replicas=1 \
--port=8080 \
--limits=cpu=200m

kubectl expose deployment try-go-deploy --port=80 --target-port=3000 --type=LoadBalancer

## check external-ip
watch kubectl get service

# test
curl -s ${external-ip}:80/ping

# cleanup
kubectl delete deployment,service --all
```


