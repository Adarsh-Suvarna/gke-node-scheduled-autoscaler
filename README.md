# Google Kubernetes Engine Node Autoscale

This repository explains the time based  Kubernetes Engine (GKE) node autoscaling. GKE's cluster autoscaler automatically resizes the number of nodes in a given node pool, based on the specified times. When demand is low during off-peak hours, the cluster autoscaler scales back down to a minimum size that we designate.
Google Kubernetes Engine cluster nodes scale up automation setup done using GCP Cloud Scheduler and Google Cloud Platform - Google Kubernetes Engine API calls. The required API calls URLs and Body to initiate the job given in the below section.

## Google Kubernetes Engine
Google Kubernetes Engine (also known as GKE) is a managed, production-ready environment for running Docker containers in the Google cloud. It permits you to form multiple-node clusters whereas conjointly providing access to any or all Kubernetes options.
GKE works with docker applications. These applications are dockerized into platform-independent, isolated user-space instances. Before you deploy any workloads on a GKE cluster, You need to always dockerize the deployment first. GKE offers two types of clusters: regional and zonal. Regional clusters consist of a three Kubernetes control planes quorum, offering higher availability than a zonal cluster can provide for your cluster’s control plane API.
Create the GKE cluster either using gcloud command or using GCP Console. Below ``gcloud`` command can be used to create a GKE Cluster.
```diff
gcloud container clusters create gke \
    --enable-ip-alias \
    --release-channel=stable \
    --machine-type=e2-standard-2 \
    --enable-autoscaling --min-nodes=1 --max-nodes=10 \
    --num-nodes=1 \
    --autoscaling-profile=optimize-utilization
```

  <p>
  <img src="https://github.com/Adarsh-Suvarna/gke-pod-scheduled-autoscaler/blob/main/img/gke-1.png">
  </p>

## Google Cloud Scheduler
Cloud Scheduler is a fully managed enterprise-grade cron job scheduler. It allows you to schedule virtually any job, including batch, big data jobs, cloud infrastructure operations, and more. You can automate everything, including retries in case of failure to reduce manual toil and intervention. Cloud Scheduler even acts as a single pane of glass, allowing you to manage all your automation tasks from one place.
With Cloud Scheduler you set up scheduled units of work to be executed at defined times or regular intervals. These work units are commonly known as cron jobs. Typical use cases might include sending out a report email on a daily basis, updating cached data every 10 minutes, or updating summary information once an hour.
Each cron job created using Cloud Scheduler is sent to a target according to a specified schedule, where the work for the task is accomplished. The target must be one of the following types:
- Publicly available HTTP/S endpoints
- Pub/Sub topics
- App Engine HTTP/S applications

The Google Cloud Scheduler can be found in the Tools section, on the GCP menu.
  <p>
  <img src="https://github.com/Adarsh-Suvarna/gke-node-scheduled-autoscaler/blob/main/img/img-5.png">
  </p>

Click on create a job to set the parameter to run. There, you have to give it a name, description, frequency, timezone and target. The frequency is specified using unix-cron format. For the target, you may choose between HTTP, Pub/Sub or App Engine HTTP. Since we cant to call the GCP GKE Cloud APIs choose the http. Fill all the details as shown in the below screenshot. We will be discussing URL, Body, Scope in the below section. As per the requirement you can copy those values into the cloud Scheduler.
  <p>
  <img src="https://github.com/Adarsh-Suvarna/gke-node-scheduled-autoscaler/blob/main/img/img-6.png">
  </p>

## Time based GKE Regional Cluster Node Auto scaling

- Method 

    projects.locations.clusters.nodePools.setSize

- HTTP request

    ```diff 
    POST https://container.googleapis.com/v1/projects/<PROJECT_ID>/locations/<LOCATION>/clusters/<CLUSTER_NAME>/nodePools/<NODEPOOL_NAME>:setSize
    ```

- Path parameters

    | Parameters | Description |
    |------------|-------------|
    | name | The name (project, location, cluster, node pool id) of the node pool to set size. Specified in the format projects/ */locations/ */clusters/ */nodePools/ *. |


- Request body

    The request body contains data with the following structure:

    **JSON representation**

    ```diff
    {
    "clusterId": "gke-test-01",
    "nodePoolId": "gke-node-pool-test-09",
    "projectId": "<PROJECT_ID>",
    "nodeCount": 1
    }
    ```

    | Fields     | Description |
    |------------|-------------|
    | projectId | The Google Developers Console project ID or project number. |
    | zone | The name of the Google Compute Engine zone in which the cluster resides. |
    | clusterId | The name of the cluster to update. |
    | nodePoolId | The name of the node pool to update. |
    | nodeCount | The desired node count for the pool. |

- Authorization Scopes

    Requires the following OAuth scope

    https://www.googleapis.com/auth/cloud-platform

## Time based GKE Zonal Cluster Node Auto scaling

- Method 

    projects.zones.clusters.nodePools.setSize

- HTTP request

    ```diff 
    POST https://container.googleapis.com/v1/projects/<PROJECT_ID>/zones/<ZONE>/clusters/<CLUSTER_ID>/nodePools/<NODE_POOL_ID>/setSize
    ```

- Path parameters

    | Parameters | Description |
    |------------|-------------|
    | projectId | The Google Developers Console project ID or project number. |
    | zone | The name of the Google Compute Engine zone in which the cluster resides. |
    | clusterId | The name of the cluster to update. |
    | nodePoolId | The name of the node pool to update.  |


- Request body

    The request body contains data with the following structure

    **JSON representation**
    ```diff
    {
    "clusterId": "gke-zonal-01",
    "projectId": "<PROJECT_ID>",
    "nodePoolId": "gke",
    "zone": "asia-south1-a",
    "nodeCount": 1
    }
    ```


    | Fields     | Description |
    |------------|-------------|
    | nodeCount | The desired node count for the pool. |
    | name | The name (project, location, cluster, node pool id) of the node pool to set size. Specified in the format projects/ */locations/ */clusters/ */nodePools/ *. |


- Authorization Scopes

    Requires the following OAuth scope

    https://www.googleapis.com/auth/cloud-platform

## GKE Node Autoscaling Observations

Following section describes the result of the Kubernetes Engine (GKE) node autoscaling. Here we have attached the screenshot for the Node autoscaling for both Zonal and Regional GKE clusters using Cloud Scheduler jobs.

1. GKE Node pool scale down using Cloud Scheduler job for Zonal and Regional GKE Clusters.
    <p>
    <img src="https://github.com/Adarsh-Suvarna/gke-node-scheduled-autoscaler/blob/main/img/img-1.png">
    </p>
2. When scale down job triggered Node count reduced to 1 for both zonal and regional clusters.
    <p>
    <img src="https://github.com/Adarsh-Suvarna/gke-node-scheduled-autoscaler/blob/main/img/img-2.png">
    </p>
3. GKE Node pool scale up using Cloud Scheduler job for Zonal and Regional GKE Clusters.
    <p>
    <img src="https://github.com/Adarsh-Suvarna/gke-node-scheduled-autoscaler/blob/main/img/img-3.png">
    </p>
4. When the scale up job triggered Node count scaled to 3 for both zonal and regional clusters.
    <p>
    <img src="https://github.com/Adarsh-Suvarna/gke-node-scheduled-autoscaler/blob/main/img/img-4.png">
    </p>
Note: Cloud scheduler jobs are based on cron jobs and we can set a desired time to implement time based GKE Cluster Nodes scale up Automation.

### Contact Me
 If you have any doubts on this please reach out to me. Happy Learning !
 - Adarsh Suvarna : adarshasuvarna@outlook.com