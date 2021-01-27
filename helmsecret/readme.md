https://github.com/zendesk/helm-secrets#usage-and-examples


helm create helmSecret-chart
helm secrets enc secrets.yaml


POD=$(kubectl get pods -n argocd -l app.kubernetes.io/name=argocd-server -o name | cut -d'/' -f 2)

argocd login localhost:8080 --insecure --username admin --password $POD
argocd account update-password --current-password $POD --new-password password

argocd login localhost:8080 --insecure --username admin --password password

argocd app create helmSecretChart --repo git@github.com:icdckl2020/helmSecretTest.git --path helmSecretChart --dest-namespace default --dest-server https://kubernetes.default.svc --helm-set replicaCount=1 --file secrets.yaml

