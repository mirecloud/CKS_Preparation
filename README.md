avg by (owner_name) (
  (
    max by (job_name) (
      kube_job_status_completion_time{namespace="$namespace"}
      - kube_job_status_start_time{namespace="$namespace"}
    )
  )
  * on (job_name) group_left(owner_name)
  (
    max by (job_name, owner_name) (
      kube_job_owner{namespace="$namespace", owner_kind="CronJob"}
    )
  )
)
