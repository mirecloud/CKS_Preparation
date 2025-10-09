# Vérifie si le draining est configuré
kubectl -n istio-system get configmap istio -o yaml | grep -A5 "drain"

kubectl -n transit-ist-client get deploy transit-gateway -o yaml | grep terminationGracePeriodSeconds

n

kubectl -n transit-ist-client get events --sort-by=.lastTimestamp | grep transit-gateway

kubectl -n transit-ist-client describe pod <transit-gateway-pod-name> | grep -A10 "State"

kubectl -n transit-ist-client logs <transit-gateway-pod> -c istio-proxy | grep -E "drain|shutdown|disconnect|removed|termination"

kubectl -n istio-system logs -l app=istiod | grep -E "disconnect|drain|termination|xds"

kubectl -n istio-system logs -l app=istiod | grep -E "disconnect|drain|termination|xds"

