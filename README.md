

1.- Empaquetar site para producción
ng build --prod


2.- Crear imágenes docker

docker build -t ibk:1.0 .
docker build -t ibk:2.0 .

- Ejemplo para correr containers:

    docker build -t mynginximage1 .
    docker run --name web1 -d -p 8080:80 mynginximage1

3.- Crear tags para publicar en registry

docker tag ibk:1.0 us.gcr.io/democloud-158516/ibk:1.0
docker tag ibk:2.0 us.gcr.io/democloud-158516/ibk:2.0

docker tag mynginximage1 us.gcr.io/democloud-158516/mynginximage1
gcloud docker -- push us.gcr.io/democloud-158516/mynginximage1

4.- Push imágenes en registry

gcloud docker -- push us.gcr.io/democloud-158516/ibk:1.0
gcloud docker -- push us.gcr.io/democloud-158516/ibk:2.0

    - Ejemplo kubernetes
    kubectl create deployment nginx --image us.gcr.io/democloud-158516/mynginximage1
    kubectl run my-nginx --image=us.gcr.io/democloud-158516/mynginximage1 --replicas=20 --port=80

4.- Publicar deployment en k8s
kubectl run ibkweb --image us.gcr.io/democloud-158516/ibk:1.0

5.- Exporner servicios
kubectl expose deployments ibkweb --port=80 --type=LoadBalancer

6.- Scalar servicio
kubectl scale deployments/ibkweb  --replicas=4


7.- Actualizar site

kubectl set image deployments/ibkweb ibkweb=us.gcr.io/democloud-158516/ibk:2.0

--Commit
kubectl rollout status deployments/ibkweb

--Rollback
kubectl rollout undo deployments/ibkweb


8.- Delete servicios

kubectl delete service ibkweb
kubectl delete deployments/ibkweb

