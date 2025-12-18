# CKS Preparation – Security Benchmarking Guide

Ce guide documente l’approche suivie pour la préparation à l’examen CKS (Certified Kubernetes Security Specialist).

---

## 1. Évaluation du Benchmark CIS

Suivez ces étapes pour exécuter l’évaluation du benchmark CIS :

### 1.1 Exécution de l’outil CIS Assessor

```bash
sh ./Assessor/Assessor-CLI.sh -i -rd /var/www/html/ -nts -rp index
```

### 1.2 Configuration de la bannière de sécurité

```bash
echo "Authorized users only. All activity may be monitored and reported." > /etc/issue
```

### 1.3 Documentation de référence

- [CIS Kubernetes Benchmarks](https://www.cisecurity.org/cis-benchmarks#kubernetes)
- [CIS-CAT Pro Supported Benchmarks](https://www.cisecurity.org/cybersecurity-tools/cis-cat-pro/cis-benchmarks-supported-by-cis-cat-pro)
- [CIS-CAT Lite](https://learn.cisecurity.org/cis-cat-lite)

---

## 2. Installation et utilisation de Kube-Bench

Kube-bench permet de vérifier les clusters Kubernetes selon le benchmark CIS.

### 2.1 Téléchargement du package kube-bench

```bash
curl -L https://github.com/aquasecurity/kube-bench/releases/download/v0.12.0/kube-bench_0.12.0_linux_amd64.deb -o kube-bench_0.12.0_linux_amd64.deb
```

### 2.2 Installation :

```bash
sudo dpkg -i kube-bench_0.12.0_linux_amd64.deb
```

### 2.3 Vérification de l’installation :

```bash
kube-bench version
```

### 2.4 Exécution des analyses kube-bench :

- **Toutes les vérifications :**
  ```bash
  kube-bench run
  ```
- **Vérification spécifique (ex : `1.3.1`) :**
  ```bash
  kube-bench --check="1.3.1"
  ```

---

Pour plus de détails, reportez-vous toujours à la documentation officielle et vérifiez les dernières versions des outils de sécurité Kubernetes pour appliquer les meilleures pratiques.

---

## 3. Démonstration : Utilisation d’un Service Account

Pour la démonstration de la création et l’utilisation d’un Service Account :

```bash
kubectl create sa dashboard-sa
```
```
serviceaccount/dashboard-sa created
```

Exemple de manifest YAML pour le déploiement :

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: web-dashboard-5f88cdc488-5s975
  namespace: default
  labels:
    name: web-dashboard
    pod-template-hash: 5f88cdc488
spec:
  serviceAccountName: default
  containers:
  - name: web-dashboard
    image: gcr.io/kodekloud/customimage/my-kubernetes-dashboard@sha256:7d70abe342b13ff1c4242dc83271ad73e4eedb04e2be0dd30ae7ac8852193069
    ports:
    - containerPort: 8080
    env:
    - name: PYTHONUNBUFFERED
      value: "1"
    volumeMounts:
    - mountPath: /var/run/secrets/kubernetes.io/serviceaccount
      name: kube-api-access-hqgwm
      readOnly: true
  volumes:
  - name: kube-api-access-hqgwm
    projected:
      sources:
        - serviceAccountToken:
            expirationSeconds: 3607
        - configMap:
            name: kube-root-ca.crt
            optional: false
        - downwardAPI: {}
```
cat /var/rbac/pod-reader-role.yaml
---
kind: Role
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  namespace: default
  name: pod-reader
rules:
- apiGroups:
  - ''
  resources:
  - pods
  verbs:
  - get
  - watch
  - list

cat /var/rbac/dashboard-sa-role-binding.yaml
---
kind: RoleBinding
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  name: read-pods
  namespace: default
subjects:
- kind: ServiceAccount
  name: dashboard-sa # Name is case sensitive
  namespace: default
roleRef:
  kind: Role #this must be Role or ClusterRole
  name: pod-reader # this must match the name of the Role or ClusterRole you wish to bind to
  apiGroup: rbac.authorization.k8s.io

---

**Note :**
Ce guide est évolutif et doit être enrichi à mesure de la progression dans la préparation CKS. Veillez à toujours appliquer les normes de sécurité et à valider vos pratiques selon les benchmarks actuels.

kubectl patch namespace wm-285348-ps3r1 --type='json' -p='[{"op": "remove", "path": "/spec/finalizers"}]'
kubectl get namespace wm-285348-ps3r1 -o json | \
  jq '.spec.finalizers=[]' | \
  kubectl replace --raw "/api/v1/namespaces/wm-285348-ps3r1/finalize" -f -
