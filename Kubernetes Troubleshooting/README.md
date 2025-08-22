
A). Problem:- Error in Image Pull back Off 
 ANS:-    possible reasons, 1). Image Name Mismatch with Entered Name and Available Image. Permission to pull the image RBAC If It is Present Inside the Private Repository. Issue with Registry authorization issue with secrets, to get the clarity about issue, use the commands like describe Pending Container or events command and fix that.

B). You Container went into CrashLoopBackOff Staus:- 

In General Behavior of the k8s/kubelet Is If any pod is Shutdown/Crashed Then kubelet Will Tries to Restart That Container. If Restated Successfully and Worked Fine then no Issues. If It Is Keep On Crashing After Restarting Pod Then the k8s pod State Will Goes Into The Crashloop backoff Status.
This Situation Indicates That Issue With Application Code or Misconfigrations of Our Container.

1). Mis Configurations:- Misconfigurations in application Code, Incorrect Environment Variables, Defination Of The Incorrect Secrets, Improper Setup of Ports (or) Volumes. Networking Issues, Dependencies.
2). liveness probe is Incorrectly Configured.
3). The Memory limits are too low
4). If the application didn't get the enough resources like CPU, RAM then the Container Keeps on Restarting and wents into crash loop backoff
5). Wrong Command Line Arguments:- It is Like Our Application Code Requires Some Specific Arguments But Different Environments are Passed. Then It Will Wents into Crash loop backoff
Bugs Exceptions:- Due to The Bugs in The Applications Codes like Unhandled Exceptions (or) Segmentations faults.
 
C0> Pod Went into Pending  Status what are Possible Reasons

1). Resource Constraints Requested CPU/Memory not available on any node.
2). No Available Nodes All nodes are cordoned/drained.
3). Unsatisfiable Node Selectors / Affinity Pod spec has nodeSelector nodeAffinity taints and tolerations
4). Storage Issues (PVC Pending) Pod uses PersistentVolumeClaim (PVC) but no matching PersistentVolume (PV) exists.
5). Error in Image Pull back Off 
6). Network Policy / CNI Issues Pod cannot be scheduled because CNI (Calico/Flannel/etc.) is not configured properly. Usually happens in new clusters.
7). Some Times Scheduler Pod is Not Running Kubernetes scheduler is not running or misconfigured.


D> Service Not Accessible

1). Check 
2). Check Load Balancer  Endpoints
3). Check Labels and Selector related Service.


E>. Node is NotReady

Disk pressure → Free space or expand disk.

❌ Memory pressure → Scale nodes or optimize workloads.

❌ Network plugin issue → Restart CNI DaemonSet.

❌ Kubelet stopped → Restart kubelet service.

F). Cluster DNS Issues

Check logs of core DNS pod and Restart that pod
kubectl rollout restart deployment coredns -n kube-system 

g). ETCD / Control Plane Issues

Take ETCD backup regularly.

Restart unhealthy control-plane pods.

Verify certificates not expired (kubeadm certs check-expiration).


If You know Below command perpectly then you can troobleshoot moset of the Kubernets Issues.

Quick Troubleshooting Flow (Interview/On-call Ready)

Check pod status → kubectl get pods -o wide.

Describe pod → kubectl describe pod <pod>.

Check logs → kubectl logs <pod>.

Check events → kubectl get events --sort-by=.metadata.creationTimestamp.

Check node status → kubectl get nodes.

Check service & endpoints → kubectl get svc,ep.

Check network policies / CNI logs.

Check resource usage → kubectl top.

Escalate to etcd / control plane if cluster-wide.


M). Pod stuck in ContainerCreating

Due to Volume mount, image pull, or CNI plugin issues.

Check if PVC is bound Status.

Validate CNI plugin pods are running.

Check kubelet log errors on node for any.

F). Pod eviction
Cause: Node resource pressure., pod affinity/antiaffinity
fixes:
Increase node resources or scale out cluster.

Adjust pod requests/limits to avoid overcommit.

Taint nodes to move workloads.


n). Init container fails

cause:- Init container script or dependency error.
Fixes:

Ensure config/secret is mounted before init runs.

Fix entrypoint script errors.

P). Liveness/Readiness probe failures

👉 Cause: Health checks failing.

Fixes:

Verify app responds on probe port/path.

Adjust initialDelaySeconds to allow app startup.

Increase timeoutSeconds if app is slow.


Q). Secret or ConfigMap not mounted

👉 Cause: Mount path issue or missing key.

Fixes:

Ensure volumeMounts path matches.

Verify key names inside ConfigMap/Secret.


R). DaemonSet not scheduling on nodes

👉 Cause: Taints or resource mismatch.

Checks:

kubectl get ds -n <namespace>
kubectl describe ds <ds-name>


Fixes:

Add tolerations for tainted nodes.

Check node selectors match.

S). HPA (Horizontal Pod Autoscaler) not scaling

👉 Cause: Metrics not available.

Checks:

kubectl get hpa
kubectl top pods
kubectl get apiservices | grep metrics


Fixes:

Ensure metrics-server is deployed and healthy.

Verify target resource usage exceeds threshold.
