# k3s-kyverno
deploy kyverno on K3s

helm repo add kyverno https://kyverno.github.io/kyverno/
helm repo update
helm dependency update

helm repo add policy-reporter https://kyverno.github.io/policy-reporter

sudo kubectl get polr -n kube-system -o wide
sudo kubectl get clusterpolicyreport -o wide

argocd app delete kyverno --cascade
argocd app terminate-op kyverno
sudo kubectl patch application/kyverno --type json --patch='[ { "op": "remove", "path": "/metadata/finalizers" } ]' -n argocd