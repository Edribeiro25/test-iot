apiVersion: traefik.io/v1alpha1
kind: Middleware
metadata:
  name: private-browser-detection
spec:
  # Détecte les navigateurs privés en fonction de l'agent utilisateur
  chain:
    - Matcher:
        TCPHost: "Host('your-domain.com')"
        RequestHeaders:
          Headers:
            UserAgent:
              - "Firefox Focus"
              - "Vivaldi"
              - "Brave"
    - Redirect:
        Scheme: "http"
        Destination: "http://app3.your-domain.com"  # URL de redirection pour les navigateurs privés
  # Rediriger vers app1 par défaut
  defaultBackend: "svc-app1-backend"

Error from server (Invalid): error when applying patch:
{"metadata":{"annotations":{"kubectl.kubernetes.io/last-applied-configuration":"{\"apiVersion\":\"networking.k8s.io/v1\",\"kind\":\"Ingress\",\"metadata\":{\"annotations\":{},\"name\":\"web-ingress\",\"namespace\":\"default\"},\"spec\":{\"rules\":[{\"http\":{\"paths\":[{\"backend\":{\"service\":{\"name\":\"svc-app1\",\"port\":{\"number\":80}}},\"middleware\":\"private-browser-detection\",\"path\":\"/\",\"pathType\":\"Prefix\"}]}},{\"host\":\"app2.com\",\"http\":{\"paths\":[{\"backend\":{\"service\":{\"name\":\"svc-app2\",\"port\":{\"number\":80}}},\"path\":\"/\",\"pathType\":\"Prefix\"}]}},{\"http\":{\"paths\":[{\"backend\":{\"service\":{\"name\":\"svc-app3\",\"port\":{\"number\":80}}},\"path\":\"/\",\"pathType\":\"Prefix\"}]}}]}}\n"}},"spec":{"rules":[{"http":{"paths":[{"backend":{"service":{"name":"svc-app1","port":{"number":80}}},"middleware":"private-browser-detection","path":"/","pathType":"Prefix"}]}},{"host":"app2.com","http":{"paths":[{"backend":{"service":{"name":"svc-app2","port":{"number":80}}},"path":"/","pathType":"Prefix"}]}},{"http":{"paths":[{"backend":{"service":{"name":"svc-app3","port":{"number":80}}},"path":"/","pathType":"Prefix"}]}}]}}
to:
Resource: "networking.k8s.io/v1, Resource=ingresses", GroupVersionKind: "networking.k8s.io/v1, Kind=Ingress"
Name: "web-ingress", Namespace: "default"
for: "/vagrant/deployment/ingress.yaml": error when patching "/vagrant/deployment/ingress.yaml":  "" is invalid: patch: Invalid value: "map[metadata:map[annotations:map[kubectl.kubernetes.io/last-applied-configuration:{\"apiVersion\":\"networking.k8s.io/v1\",\"kind\":\"Ingress\",\"metadata\":{\"annotations\":{},\"name\":\"web-ingress\",\"namespace\":\"default\"},\"spec\":{\"rules\":[{\"http\":{\"paths\":[{\"backend\":{\"service\":{\"name\":\"svc-app1\",\"port\":{\"number\":80}}},\"middleware\":\"private-browser-detection\",\"path\":\"/\",\"pathType\":\"Prefix\"}]}},{\"host\":\"app2.com\",\"http\":{\"paths\":[{\"backend\":{\"service\":{\"name\":\"svc-app2\",\"port\":{\"number\":80}}},\"path\":\"/\",\"pathType\":\"Prefix\"}]}},{\"http\":{\"paths\":[{\"backend\":{\"service\":{\"name\":\"svc-app3\",\"port\":{\"number\":80}}},\"path\":\"/\",\"pathType\":\"Prefix\"}]}}]}}\n]] spec:map[rules:[map[http:map[paths:[map[backend:map[service:map[name:svc-app1 port:map[number:80]]] middleware:private-browser-detection path:/ pathType:Prefix]]]] map[host:app2.com http:map[paths:[map[backend:map[service:map[name:svc-app2 port:map[number:80]]] path:/ pathType:Prefix]]]] map[http:map[paths:[map[backend:map[service:map[name:svc-app3 port:map[number:80]]] path:/ pathType:Prefix]]]]]]]": strict decoding error: unknown field "spec.rules[0].http.paths[0].middleware"
Error from server (BadRequest): error when creating "/vagrant/deployment/middleware.yaml": Middleware in version "v1alpha1" cannot be handled as a Middleware: strict decoding error: unknown field "spec.Backend", unknown field "spec.chain[0].Matcher", unknown field "spec.chain[1].Redirect"
