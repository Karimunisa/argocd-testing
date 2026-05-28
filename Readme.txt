# How to install ArgoCD with minikube

1. Ec2 instance volume should have the capacity 20GB
2. Then check disk space 
  - df -h
  - Now / should show around: 20GB
3. lsblk   
    -You should see disk changed to 20G but partition still 7G.
4. restart minikube 
    - minikube delete 
    - minikube start --driver=docker --memory=2200 --cpus=2
5. Reinstall ArgoCD 
   - kubectl create namespace argocd
   - kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
6. Verify pods
   - kubectl get pods -n argocd
7. Wait for Pods continuously
   - kubectl get pods -n argocd -w
8. Change ArgoCD server service to NodePort
 - kubectl patch svc argocd-server -n argocd \
   -p '{"spec": {"type": "NodePort"}}'
9. Get Service port
   - kubectl get svc argocd-server -n argocd
10. Check AWS Security Group 
    - Custom TCP in inbound rules
11. kubectl get endpoints argocd-server -n argocd
12. Access ArgoCD UI
   - kubectl port-forward svc/argocd-server -n argocd 8080:443
13. https://<EC2:PUBLIC-PI>:NodePortIP
14. Get Admin password
   - kubectl -n argocd get secret argocd-initial-admin-secret \
     -o jsonpath="{.data.password}" | base64 -d
   - Username: admin
15. vi create-application.yaml
    kubectl apply -f create-application.yaml  --> Create application in ArgoCD
    kubectl delete -f create-application.yaml 
    kubectl get pods
    kubectl system prune
    kubectl delete pods guestbook-ui
    kubectl delete all --all -A
    kubectl get pods -n argocd
    kubectl get pods
    kubectl get ns
    kubectl delete ns argocd
    kubectl get pods -n argocd
    kubectl get deployment -n argocd