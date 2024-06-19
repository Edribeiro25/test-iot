#ingress.yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: web-ingress
spec:
  rules:
  - host: app1.com
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: svc-app1
            port:
              number: 80
  - host: app2.com
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: svc-app2
            port:
              number: 80
  - http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: svc-app3
            port:
              number: 80
#svc.yaml
apiVersion: v1
kind: Service
metadata:
  name: svc-app1
spec:
  selector:
    app: app1
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
  type: ClusterIP
---
apiVersion: v1
kind: Service
metadata:
  name: svc-app2
spec:
  selector:
    app: app2
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
  type: ClusterIP
---
apiVersion: v1
kind: Service
metadata:
  name: svc-app3
spec:
  selector:
    app: app3
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
  type: ClusterIP
#deployment.yaml for each app
apiVersion: apps/v1
kind: Deployment
metadata:
  name: app1-deployment
  labels:
    app: app1
spec:
  replicas: 1
  selector:
    matchLabels:
      app: app1
  template:
    metadata:
      labels:
        app: app1
    spec:
      containers:
      - name: app1
        image: paulbouwer/hello-kubernetes
        ports:
        - containerPort: 8080
apiVersion: apps/v1
kind: Deployment
metadata:
  name: app2-deployment
  labels:
    app: app1
spec:
  replicas: 3
  selector:
    matchLabels:
      app: app2
  template:
    metadata:
      labels:
        app: app2
    spec:
      containers:
      - name: app1
        image: paulbouwer/hello-kubernetes
        ports:
        - containerPort: 8080
apiVersion: apps/v1
kind: Deployment
metadata:
  name: app3-deployment
  labels:
    app: app3
spec:
  replicas: 1
  selector:
    matchLabels:
      app: app3
  template:
    metadata:
      labels:
        app: app3
    spec:
      containers:
      - name: app3
        image: paulbouwer/hello-kubernetes
        ports:
        - containerPort: 8080
apiVersion: traefik.containo.us/v1alpha1
kind: Middleware
metadata:
  name: detect-private-browsing
  namespace: default
spec:
  headers:
    customRequestHeaders:
      X-Private-Browsing: "true"
   middleware:
          name: detect-private-browsing
