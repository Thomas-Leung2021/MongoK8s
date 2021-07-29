# MongoK8s
This is a simple project for deploying Mongodb and MongoExpress in Minikube.

## General request flow:
Request from browser
Go to Mongo Express External Service
Forward to Mongo Express Pod
Pod connects to Mongo DB internal service (the database URL in the ConfigMap)
Forward to the MongoDB Pod (Authenticate using Secret)

## Steps to deploy deployment:
```
kubectl apply -f mongo-secret.yaml  // you need to create this first before deployment

kubectl get secret                  // to see the secret is installed

kubectl apply -f mongo.yaml         // create deployment

kubectl get all                     // should see the pod, deployment, and replicaset

kubectl get pod --watch             // see the creating process of the pod

kubectl describe pod \[pod-name\]
```


## Steps to deploy mongoDB Internal Service
```
kubectl apply -f mongo.yaml

kubectl get service                 // check service is created

// in Endpoints, you can see the ip address of the pod and 
// the port where the application in the port is listening it
kubectl describe service mongodb-service

// you can verify the ip address of the pod (is the same in endpoint)
kubectl get pod -o wide

// get all components, filter by mongodb
// you should see pod, service, deployment and replicaset
get all | grep mongodb
```

## Mongo Express Deployment & ConfigMap
```
kubectl apply -f mongo-configmap.yaml

kubectl apply -f mongo-express.yaml

// you can see the express server start and it is connected to the database
kubectl logs [mongo-express-pod-name] 
```

## Monge Express External Service for browser access
```
kubectl apply -f mongo-express.yaml

// you will see mongo-express-service EXTERNAL-IP is <pending> in minikube
// normally you will see an external ip address
kubectl get service     

// assign external service a public ip address
minikube service mongo-express-service
```

### Reference:
https://www.youtube.com/watch?v=X48VuDVv0do