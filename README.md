CIS Benchmark

sh ./Assessor/Assessor-CLI.sh -i -rd /var/www/html/ -nts -rp index
echo "Authorized users only. All activity may be monitored and reported." > /etc/issue
https://www.cisecurity.org/cis-benchmarks#kubernetes
https://www.cisecurity.org/cybersecurity-tools/cis-cat-pro/cis-benchmarks-supported-by-cis-cat-pro
https://learn.cisecurity.org/cis-cat-lite


Kube-bench

curl -L https://github.com/aquasecurity/kube-bench/releases/download/v0.12.0/kube-bench_0.12.0_linux_amd64.deb -o kube-bench_0.12.0_linux_amd64.deb
sudo dpkg -i kube-bench_0.12.0_linux_amd64.deb
kube-bench version
kube-bench run 
kube-bench --check="1.3.1"
