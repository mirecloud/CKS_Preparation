Unexpected Envoy disconnection: downstream_local_disconnect(filter_chain_is_being_removed) observed on transit-gateway during API calls



Hello Tetrate team,

We are observing unexpected Envoy disconnections within our production cluster.

**Environment**
- Cluster: app21.hz.k8s.c0.ms.com
- Namespace: transit-ist-client
- Tetrate Istio / Proxy version: 1.21.6 (image: mshub.webfarm.ms.com/tetrate/proxyv2:1.21.6-distroless)
- Deployment: transit-gateway
- Istio-proxy container started: 2025-10-09T11:11:58 -0400

**Issue**
During normal operation, API requests to `/single-executions/transfers-aggregate` intermittently fail with: downstream_local_disconnect(filter_chain_is_being_removed)
and an HTTP status `0`.

Trace and logs show that the issue happens at the moment when a `transit-gateway` pod restarts or reloads. 
It looks like the Envoy sidecar terminates active connections abruptly while the filter chain is being removed.

**Impact**
- Requests are terminated before completion (HTTP status 0)
- Affected services: transit-gateway and downstream client APIs
- The issue has been reproduced during rolling deployments

**Steps to reproduce**
1. Send continuous API requests to the service `transit-gateway`
2. Trigger a pod restart or rollout (`kubectl rollout restart deploy/transit-gateway`)
3. Observe in the logs: downstream_local_disconnect(filter_chain_is_being_removed)

4. and failed API calls

**What we’ve already verified**
- The deployment and HPA are healthy (`3/3` ready pods)
- The proxy version is `1.21.6` from Tetrate image registry
- TerminationGracePeriodSeconds: 30s (default)
- No abnormal CPU/memory pressure
- No other errors in Istio control plane

**Question / Request**
Could you please confirm if this behavior (filter chain removed during connection drain) is expected in version 1.21.6?
If not, is there any recommended configuration (e.g. `drain_duration`, `connection_drain_timeout`, or graceful shutdown tuning) to prevent Envoy from abruptly disconnecting active connections during rollout?

**Attachments**
- Cluster logs from failing request
- `kubectl get all -n transit-ist-client`
- `kubectl describe pod transit-gateway-...` (showing istio-proxy container)

Thanks in advance for your guidance.

