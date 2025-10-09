# Vérifie si le draining est configuré
kubectl -n istio-system get configmap istio -o yaml | grep -A5 "drain"
kubectl -n transit-ist-client get deploy transit-gateway -o yaml | grep terminationGracePeriodSeconds
