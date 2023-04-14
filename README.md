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
    | name | The name (project, location, cluster, node pool id) of the node pool to set size. Specified in the format projects/*/locations/*/clusters/*/nodePools/*. |


- Request body
The request body contains data with the following structure:
