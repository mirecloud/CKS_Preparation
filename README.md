max by (namespace, job_name, mks_cluster) (
  kube_job_status_start_time{mks_cluster="$cluster"}
)
* on(namespace, job_name, mks_cluster) group_left()
max by (namespace, job_name, mks_cluster) (
  kube_job_failed{condition="true", mks_cluster="$cluster"} == 1
)


(
  max by (namespace, job_name, mks_cluster) (kube_job_status_start_time{mks_cluster="$cluster"})
  * on(namespace, job_name, mks_cluster) group_left()
  max by (namespace, job_name, mks_cluster) (kube_job_failed{condition="true", mks_cluster="$cluster"} == 1)
)
* on(namespace, job_name, mks_cluster) group_left(owner_name)
max by (namespace, job_name, owner_name, mks_cluster) (
  kube_job_owner{owner_kind="CronJob", mks_cluster="$cluster"}
)



----
(
  kube_job_status_start_time{mks_cluster="$cluster"} * 1000
)
* on(job_name, namespace, mks_cluster) group_left()
(
  kube_job_failed{condition="true", mks_cluster="$cluster"} == 1
)
