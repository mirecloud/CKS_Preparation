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
---

/* start_time en ms, dédupliqué */
max by (namespace, job_name, mks_cluster) (
  max without (endpoint, instance, job, service, prometheus, prometheus_replica, container, pod) (
    kube_job_status_start_time{mks_cluster="$cluster"} * 1000
  )
)
* on (namespace, job_name, mks_cluster) group_left()
/* filtre “failed”, dédupliqué */
max by (namespace, job_name, mks_cluster) (
  max without (endpoint, instance, job, service, prometheus, prometheus_replica, container, pod) (
    kube_job_failed{condition="true", mks_cluster="$cluster"} == 1
  )
)
---

max by (namespace, job_name, mks_cluster) (
  (
    max by (namespace, job_name, mks_cluster) (
      max without (endpoint, instance, job, service, prometheus, prometheus_replica, container, pod) (
        kube_job_status_start_time{mks_cluster="$cluster"} * 1000
      )
    )
  * on (namespace, job_name, mks_cluster) group_left()
    max by (namespace, job_name, mks_cluster) (
      max without (endpoint, instance, job, service, prometheus, prometheus_replica, container, pod) (
        kube_job_failed{condition="true", mks_cluster="$cluster"} == 1
      )
    )
  )
)
---
min by (namespace, job_name, mks_cluster) (
  (
    max without (endpoint, instance, job, service, prometheus, prometheus_replica, container, pod) (
      kube_job_status_start_time{mks_cluster="$cluster"} * 1000
    )
  * on (namespace, job_name, mks_cluster) group_left()
    max without (endpoint, instance, job, service, prometheus, prometheus_replica, container, pod) (
      kube_job_failed{condition="true", mks_cluster="$cluster"} == 1
    )
  )
)
