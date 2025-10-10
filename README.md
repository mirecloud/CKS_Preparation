
kubectl -n istio-system get configmap istio -o yaml > istio-configmap.yaml
kubectl -n istio-system get telemetry

kubectl -n istio-system get configmap

kubectl -n istio-system get telemetry telemetry -o yaml > telemetry.yaml
istioctl proxy-config all <pod_name> -n <namespace> > envoy-config-dump.txt
Hi Tanuj,

I’ve gathered the requested configurations from the istio-system namespace:

istio ConfigMap → istio-configmap.yaml

Telemetry CR → telemetry.yaml

Both files are attached to this ticket for your reference.

If needed, I can also include the Envoy proxy configuration dump from one of the transit-gateway pods to help validate the filterChain behaviour.

Let me know if you’d like me to add that as well.

— Emmanuel
