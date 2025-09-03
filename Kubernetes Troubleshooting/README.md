

**The Below Commands are Majorly Used in Troubleshooting of Kubernetes Related Issues.:-**

**kubectl get pods -o wide**

For Checking the Pods Status

**kubectl describe pod**

Used to Describe the pods.

**kubectl logs (podName)**

To check the logs of the pods

**kubectl get events --sort-by=.metadata.creationTimestamp**

Used to check the events of Cluster in current namespace

**Check node status**

Used to Check Node Status

**kubectl get svc,ep**

This Command is Used to Check service Details & endpoints.

**kubectl top**

Check resource usage



**A). Error In Pulling The Image Error Name- Image Pull back Off**

--> Possible Reasons,

1). Image Name Mismatch with Entered Name and Available Image In Registry.

2). Authorization to Pull the Image If It is Present Inside the Private Repository. 

3). Issue With Registry Authorization Issue With Kubernetes Secrets Manager,
    To Get the Clarity About the Issue, Use the Commands Like Describe Pending Container or Events Command and Fix That Issue.

**B). Container Went Into CrashLoopBackOff Status:-** 

 --> In General Behavior of the k8s/kubelet, If Any Pod is Shutdown/Crashed Then kubelet Will Tries to Restart That Container. If Restated Successfully and Worked Fine then Their is No Issues. If It Is Keep On Crashing After Restarting Pod Then the k8s Pod State Will Goes Into The Crashloop Backoff Status.
 
--> This Situation Indicates That Issue With Application Code or Misconfigrations of Our Container.

**Possible Reasons:-**

1). Misconfigurations in Application Code, Incorrect Environment Variables, Defined The Incorrect Secrets, Improper Setup of Ports (or) Volumes. Networking Issues, Dependencies.

2). liveness Probe is Incorrectly Configured.

3). The Memory Limits Are Too Low.

4). If The Application Didn't Get the Enough Resources like CPU, RAM Then the Container Keeps on Restarting and wents into crash loop backoff.

5). Wrong Command Line Arguments Passed, It is Like Our Application Code Requires Some Specific Arguments But Different Environment Variables are Passed. Then It Will Goes into Crash loop backoff Status.

6). Bugs Exceptions:- Due to The Bugs in The Applications Codes like Unhandled Exceptions (or) Segmentations faults.
 
**C). Pod Went into Pending  Status what are Possible Reasons**

--> Possible Reasons:-

1). Resource Constraints Requested CPU/Memory Not Available at Node.
2). No Available Nodes, (OR) All nodes are Cordoned/Drained/Tainted.
3). Unsatisfiable Node Selectors / Affinity Pod spec has nodeSelector nodeAffinity Taints and Tolerations.
4). Storage Issues (PVC Pending) Pod Uses PersistentVolumeClaim (PVC) but No matching PersistentVolume (PV) exists/Permissions.
5). Error in Image Pull back Off.
6). Network Policy / CNI Issues Pod Cannot be Scheduled Because of CNI (Calico/Flannel/etc.) is Not Configured Properly. Usually Happens in New Clusters.
7). Some Times Scheduler Pod is Not Running State.


**D> Service Not Accessible**

1). Check Service Endpoints Related to pods.
2). Check Labels and Selector related Service.

**E>. Node is NotReady**

Check Disk pressure, CPU,Memory pressure.

Network plugin issue Solved By the Restarting the CNI DaemonSet.

Check Kubelet Service Is Working.

**F). Cluster DNS Issues**

Check logs of core DNS pod and Restart that pod

**G). ETCD / Control Plane Issues**

Take ETCD Backup Regularly.

Restart Unhealthy Control Plane Pods.

Verify Certificates Not Expired



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
