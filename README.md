
![alt text](https://cdn-images-1.medium.com/max/1200/1*1BF_eIkV6wkuqsziZ_hf7Q.png)

# Stateful Kubernetes CRUD Application Frontend

This Angular application is a part of a Medium Post named  **### How to Deploy a Stateful CRUD Web Application to Minikube(Local Cluster) and Google Cloud Kubernetes Engine(GKE)**. It is about creating a Stateful application and deploying it both Minikube(Local Cluster) and Google Cloud Kubernetes Engine(GKE).

### How to run it?
This is a classic Angular application.

Run `ng serve` for a dev server. Navigate to `http://localhost:4200/`. The app will automatically reload if you change any of the source files.

Run `ng build` to build the project. The build artifacts will be stored in the `dist/` directory. Use the `--prod` flag for a production build.

---

#### Kubernetes Deployment
There is another Github repository that contains all deployment files. But, I also adding the once that related to the Frontend.
```YML
apiVersion: v1  
kind: Service  
metadata:  
  name: application-web-service  
spec:  
  selector:  
    app: application-web-pod  
  ports:  
  - name: application-web-service-port-name  
    protocol: TCP  
    port: 4200  
    targetPort: 4200  
  type: ClusterIP  
  
---  
  
apiVersion: apps/v1  
kind: Deployment  
metadata:  
  name: application-web-deployment  
spec:  
  selector:  
    matchLabels:  
      app: application-web-pod  
  replicas: 1  
  template:  
    metadata:  
      labels:  
        app: application-web-pod  
    spec:  
      containers:  
        - name: application-web-container  
          image: gcr.io/bionic-trilogy-298608/quote-application-web  
          lifecycle:  
              preStop:  
                  exec:  
                      command: ["usr/sbin/ngnix", "-s", "quit"]  
  ports:  
          - containerPort: 4200
 
```
