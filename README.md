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
