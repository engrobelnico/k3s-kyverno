# k3s-kyverno
deploy kyverno on K3s

helm repo add kyverno https://kyverno.github.io/kyverno/
helm repo update
helm dependency update