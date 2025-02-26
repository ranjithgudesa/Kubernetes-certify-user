Authenticate to cluster
aws --region us-east-1 eks update-kubeconfig --name dev-cluster
kubectl config get-contexts
kubectl create ns dev
kubectl get ns
kubectl config set-context <context-name>
kubectl config use-context <context-name>

https://kubernetes.io/docs/reference/access-authn-authz/certificate-signing-requests/#create-certificatessigningrequest <k8s document>
https://aungzanbaw.medium.com/a-step-by-step-guide-to-creating-users-in-kubernetes-6a5a2cfd8c71
