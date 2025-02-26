This repo will help you out to crate user, generate certificate, certificate signin request and approve certificate and RBAC concept

Authenticate to cluster

  aws --region us-east-1 eks update-kubeconfig --name dev-cluster

  kubectl config get-contexts
  
  kubectl config set-context <context-name>
  
  kubectl config use-context <context-name>

  kubectl create ns dev
  
  kubectl get ns

https://kubernetes.io/docs/reference/access-authn-authz/certificate-signing-requests/#create-certificatessigningrequest <k8s document>
https://aungzanbaw.medium.com/a-step-by-step-guide-to-creating-users-in-kubernetes-6a5a2cfd8c71

Generate certificate

openssl genrsa -out devuser.key 2048
openssl req -new -key devuser.key -out devuser.csr -subj "/CN=devuser"
cat devuser.csr | base64 | tr -d "\n"
create yaml file for certificate sign request, copie and past the devuser.csr encoded base64 token in request string
kubectl apply -f .\certificate-signin-req.yaml

Approve the CertificateSigningRequest
Get the list of CSRs: kubectl get csr
Approve the CSR: kubectl certificate approve devuser
Retrieve the certificate from the CSR: kubectl get csr/devuser -o yaml
kubectl get csr -o yaml >devuser.crt
#kubectl get csr devuser -o jsonpath='{.status.certificate}' | base64 -d  > devuser.crt
kubectl apply -f role.yaml
kubectl apply -f rolebinding.yaml
kubectl config set-credentials devuser --client-key=devuser.key --client-certificate=devuser.crt --embed-certs=true
kubectl config set-context devuser --cluster=dev-cluster --user=devuser
kubectl config use-context devuser
kubectl config current-context
kubectl config view



