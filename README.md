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

sudo kubectl api-resources --verbs=list --namespaced -o name | xargs -n 1 kubectl get --show-kind --ignore-not-found -n kyverno

sudo kubectl patch ns kyverno -p '{"metadata":{"finalizers":null}}'
sudo kubectl delete ns kyverno

kctl get policyreport -A
kctl get policyreport -n prometheus | grep exporter
kctl get cpol,pol -A
kctl get validatingwebhookconfigurations,mutatingwebhookconfigurations
kctl -n kyverno logs <kyverno_pod_name> 
kctl -n kyverno get netpol kyverno-admission-controller -o yaml

kctl get ds,pod -n prometheus -l app.kubernetes.io/name=prometheus-node-exporter