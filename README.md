kubectl get pods -A -o jsonpath='{range .items[*]}{.metadata.annotations.sidecar\.istio\.io/status}{"\n"}{end}' | grep -c "istio-proxy"
