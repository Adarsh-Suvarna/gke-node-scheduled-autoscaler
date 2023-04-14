# Google Kubernetes Engine Node Autoscale

This repository explains the time based  Kubernetes Engine (GKE) node autoscaling. GKE's cluster autoscaler automatically resizes the number of nodes in a given node pool, based on the specified times. When demand is low during off-peak hours, the cluster autoscaler scales back down to a minimum size that we designate.
Google Kubernetes Engine cluster nodes scale up automation setup done using GCP Cloud Scheduler and Google Cloud Platform - Google Kubernetes Engine API calls. The required API calls URLs and Body to initiate the job given in the below section.

## GKE Regional Cluster

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
  "projectId": "testing-vm-302307",
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

## GKE Zonal Cluster

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
  "projectId": "testing-vm-302307",
  "nodePoolId": "gke",
  "zone": "asia-south1-a",
  "nodeCount": 1
}
```

    | Fields     | Description |
    |------------|-------------|
    | nodeCount | The desired node count for the pool. |
    | name | The name (project, location, cluster, node pool id) of the node pool to set size. Specified in the format projects/*/locations/*/clusters/*/nodePools/*. |

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