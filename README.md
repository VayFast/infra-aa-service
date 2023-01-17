# infra-as-service
infra as a service 
Hosting WePN Servers on Kubernetes
# Approach
- Build a Docker image using the “build action” script, tag image and push to an image registry

- Create a TLS certificate using OpenSSL and copy to Kubernetes

- Build Kubernetes application for private key generation (run Docker image with associated environment variables and volume mounts)
- Build Kubernetes application for hosting the servers (run Docker image with associated environment variables and volume mounts)

- Output associated encrypted string to use in WePN Manager

# Deploy
- apply NFS-Service.yaml file `kubectl apply -f 002-nfs-server-service.yaml`
- get the clusterIP of the service `kubectl get svc nfs-server`
- apply manager.yaml `kubectl apply -f`
- finally deploy deployment.yaml `kubectl apply -f`
- Output associated encrypted string to use in WePN manager: `export SHA=$(openssl x509 -noout -fingerprint \ -sha256 -inform pem -in  {SB_CERTIFICATE_FILE} \ | sed "s/://g" | sed 's/.*=//')`
