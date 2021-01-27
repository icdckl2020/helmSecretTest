https://medium.com/faun/handling-kubernetes-secrets-with-argocd-and-sops-650df91de173
https://github.com/zendesk/helm-secrets#usage-and-examples


helm create helmSecret-chart
helm secrets enc secrets.yaml


POD=$(kubectl get pods -n argocd -l app.kubernetes.io/name=argocd-server -o name | cut -d'/' -f 2)

argocd login localhost:8080 --insecure --username admin --password $POD
argocd account update-password --current-password $POD --new-password password

argocd login localhost:8080 --insecure --username admin --password password

argocd app create helmsecret --repo git@github.com:icdckl2020/helmSecretTest.git --path helmsecret --dest-namespace default --dest-server https://kubernetes.default.svc --helm-set replicaCount=1 --file secrets.yaml

#Install Locally
helm secrets install --name helmsecret helmsecret/ -f helmsecret/secrets.yaml

#Install through argoCD
kubectl apply -f project.yaml

#Check secret is installed
kubectl -n default get secret test-secret -o yaml
echo cGFzc3dvcmQ= | base64 --decode

kubectl -n default delete secret test-secret

#Apps of App
https://argoproj.github.io/argo-cd/operator-manual/cluster-bootstrapping/


