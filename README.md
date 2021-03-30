## Install cert manager
```bash
helm repo add jetstack https://charts.jetstack.io
helm repo update
helm install \
  cert-manager jetstack/cert-manager \
  --namespace cert-manager \
  --version v1.2.0 \
  --create-namespace \
 --set installCRDs=true
```

## Generate keys and certificate
```bash
sudo apt-get -y install openssl
openssl req -x509 -sha256 -nodes -days 365 -newkey rsa:2048 -subj '/O=example Inc./CN=tls' -keyout tls/tls.key -out tls/tls.crt
cat tls/tls.key| base64 -w 0
cat tls/tls.crt| base64 -w 0
```



## Deploy nginx ingress
```bash
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
helm repo update
helm install nginx-ingress ingress-nginx/ingress-nginx -n nginx-ingress --create-namespace
```
## Deploy to kube all the components
```bash
kubectl apply -f components
```

## Test deployment
```bash
echo "127.0.0.1   example.com" | sudo tee -a /etc/hosts
kubectl port-forward svc/nginx-ingress-ingress-nginx-controller -n nginx-ingress 8090:443
curl -HHost:example.com https://localhost:8090/foo
```


## Links
https://kubernetes.github.io/ingress-nginx/user-guide/tls/
