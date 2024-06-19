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

Error from server (BadRequest): error when creating "/vagrant/deployment/middleware.yaml": Middleware in version "v1alpha1" cannot be handled as a Middleware: strict decoding error: unknown field "spec.chain[0].Matcher", unknown field "spec.chain[1].Redirect"
