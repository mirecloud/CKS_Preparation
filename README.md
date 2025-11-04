# CKS Preparation – Security Benchmarking Guide

This document Folllow the path for my cks preparation.

---

## CIS Benchmark Assessment

Follow these steps to run CIS Benchmark assessments:

1. **Run the CIS Assessor Tool:**

   ```bash
   sh ./Assessor/Assessor-CLI.sh -i -rd /var/www/html/ -nts -rp index
   ```

2. **Configure security banner:**

   ```bash
   echo "Authorized users only. All activity may be monitored and reported." > /etc/issue
   ```

3. **Reference Documentation:**
   - [CIS Kubernetes Benchmarks](https://www.cisecurity.org/cis-benchmarks#kubernetes)
   - [CIS-CAT Pro Supported Benchmarks](https://www.cisecurity.org/cybersecurity-tools/cis-cat-pro/cis-benchmarks-supported-by-cis-cat-pro)
   - [CIS-CAT Lite](https://learn.cisecurity.org/cis-cat-lite)

---

## Kube-Bench Installation & Usage

Kube-bench is a tool for checking Kubernetes clusters against the CIS Benchmark.

1. **Download the kube-bench package:**

   ```bash
   curl -L https://github.com/aquasecurity/kube-bench/releases/download/v0.12.0/kube-bench_0.12.0_linux_amd64.deb -o kube-bench_0.12.0_linux_amd64.deb
   ```

2. **Install kube-bench:**

   ```bash
   sudo dpkg -i kube-bench_0.12.0_linux_amd64.deb
   ```

3. **Verify the installation:**

   ```bash
   kube-bench version
   ```

4. **Run kube-bench checks:**

   - Run all checks:

     ```bash
     kube-bench run
     ```

   - Run specific check (example: `1.3.1`):

     ```bash
     kube-bench --check="1.3.1"
     ```

---

For more details, always refer to the official documentation and review the latest versions of the security tools for Kubernetes security best practices.


For demonstraation of service accout we use this 
 k create sa dashboard-sa
serviceaccount/dashboard-sa created

controlplane ~ ➜  k describe pod/web-dashboard-5f88cdc488-5s975
Name:             web-dashboard-5f88cdc488-5s975
Namespace:        default
Priority:         0
Service Account:  default
Node:             controlplane/192.168.48.7
Start Time:       Tue, 04 Nov 2025 01:41:02 +0000
Labels:           name=web-dashboard
                  pod-template-hash=5f88cdc488
Annotations:      <none>
Status:           Running
IP:               10.22.0.9
IPs:
  IP:           10.22.0.9
Controlled By:  ReplicaSet/web-dashboard-5f88cdc488
Containers:
  web-dashboard:
    Container ID:   containerd://1fd8165d8e5d6686ef3fef82f11e83407f583c16afef641d4ef3348c5ad3a895
    Image:          gcr.io/kodekloud/customimage/my-kubernetes-dashboard
    Image ID:       gcr.io/kodekloud/customimage/my-kubernetes-dashboard@sha256:7d70abe342b13ff1c4242dc83271ad73e4eedb04e2be0dd30ae7ac8852193069
    Port:           8080/TCP
    Host Port:      0/TCP
    State:          Running
      Started:      Tue, 04 Nov 2025 01:41:04 +0000
    Ready:          True
    Restart Count:  0
    Environment:
      PYTHONUNBUFFERED:  1
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-hqgwm (ro)
Conditions:
  Type                        Status
  PodReadyToStartContainers   True 
  Initialized                 True 
  Ready                       True 
  ContainersReady             True 
  PodScheduled                True 
Volumes:
  kube-api-access-hqgwm:
    Type:                    Projected (a volume that contains injected data from multiple sources)
    TokenExpirationSeconds:  3607
    ConfigMapName:           kube-root-ca.crt
    Optional:                false
    DownwardAPI:             true
QoS Class:                   BestEffort
Node-Selectors:              <none>
Tolerations:                 node.kubernetes.io/not-ready:NoExecute op=Exists for 300s
                             node.kubernetes.io/unreachable:NoExecute op=Exists for 300s
Events:
  Type    Reason     Age    From               Message
  ----    ------     ----   ----               -------
  Normal  Scheduled  3m58s  default-scheduler  Successfully assigned default/web-dashboard-5f88cdc488-5s975 to controlplane
  Normal  Pulling    3m59s  kubelet            Pulling image "gcr.io/kodekloud/customimage/my-kubernetes-dashboard"
  Normal  Pulled     3m57s  kubelet            Successfully pulled image "gcr.io/kodekloud/customimage/my-kubernetes-dashboard" in 2.088s (2.088s including waiting). Image size: 31659887 bytes.
  Normal  Created    3m57s  kubelet            Created container: web-dashboard
  Normal  Started    3m57s  kubelet            Started container web-dashboard
