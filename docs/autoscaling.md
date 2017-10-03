## Auto Scaling 

>>>>TO-DO: I'm not sure if the auto scaling options in the new UI will be similar to auto scaling in HDCloud or the auto scaling in the old Cloudbreak. 

>>>> TO-DO: In the new UI, auto scaling is part of the create cluster process. My guess is that it will also be available from the cluster details page.

For each cluster you can optionally enable auto scaling, which allows you to define one or more **scaling policies** to automatically increase or decrease the capacity of the cluster or an application running on it based on an **alert** and according to the policy definition. Scaling granularity is at the host group level; Thus you have an option to scale services or components only, not the whole cluster.

The auto scaling capability is based on [Ambari Metrics](https://cwiki.apache.org/confluence/display/AMBARI/Metrics) and [Ambari Alerts](https://cwiki.apache.org/confluence/display/AMBARI/Alerts). Based on the blueprint used and the services running, Cloudbreak accesses all available metrics from the subsystem and defines alerts based on these metrics.


### Enable Auto Scaling

You can enable auto scaling for your cluster either when creating a cluster or once the cluster is running, from the cluster details page. 

Choose **enable** to enable auto-scaling and then:

1. Create either [a metric-based alert](#create-a-metric-based-alert) or [a time-based alert](#create-a-time-based-alert).  
2. [Create a scaling policy](#create-a-scaling-policy).    
3. [Configure Auto Scaling](#configure-auto-scaling).  


#### Create a Metric-based Alert

Metric-based alerts use Ambari metrics. These metrics have a default threshold value configured in Ambari, which you can modify in Ambari web UI.

To create a new Cloudbreak metric-based alert in the Cloudbreak UI:

1. Enable auto scaling by clicking **enable**.  
1. Enter **alert name**. Only alphanumeric characters (min 5, max 100 characters) are allowed.  
1. (Optional) Enter a **description** for the new alert.  
1. Select a metric, and then its **desired state**. The Ambari metrics available are based on installed services and their state is based on the Ambari threshold value:  
    * OK  
    * WARN  
    * CRITICAL  
1. Enter the **period** (in minutes) to define the metric state endurance after the alert has been triggered. Only numeric characters are allowed.

You can change default threshold for an Ambari metric in the Ambari web UI by using the following steps:

1. Log in to Ambari web UI.  
2. From the header menu, select **Alerts** to open the Alerts page.  
3. Select an alert from the list.  
4. In the Configuration panel, click **Edit**.  
5. Now you can modify the values in the **Threshold** section.

<a href="../images/ambari_threshold.png" target="_blank" title="click to enlarge"><img src="../images/ambari_threshold.png" width="650" title="Ambari web UI"></a> 


#### Create a Time-based Alert

Time-based alerts are based on cron expressions, allowing alerts to be triggered based on time.

To create a new Cloudbreak time-based alert in the Cloudbreak UI:

1. Enter **alert name**. Only alphanumeric characters (min 5, max 100 characters) are allowed.  
2. (Optional) Enter **description** for the new alert.   
3. Select a **time zone** for the new alert.    
4. Provide the **cron expression** to define the time-based job scheduler (cron expression) for this alert.


#### Create a Scaling Policy 

To create a new Cloudbreak scaling policy:

1. Enter the **policy name**. Only alphanumeric characters (min 5, max 100 characters) are allowed.
2. Select a type first (node count, percentage, or exact), and then enter a value for the **scaling adjustment**:
    * **node count** - number of nodes to be added or removed
    * **percentage** - computed percentage adjustment based on the cluster size
    * **exact** - specific target size of the cluster
3. Select the Ambari host group where the cluster is to be scaled.  
3. Select the previously created Cloudbreak alert to apply the scaling policy to it.  

A scaling policy may contain multiple alerts. When an alert is triggered, a scaling adjustment is applied.

In a scaling policy, the triggered rules are applied in order.


#### Configure Auto Scaling 

You can configure cluster scaling by 


| Setting	| Description |	Default Value |
|---|---|---|
| Cooldown Time | After an auto scaling event occurs, the amount of time to wait before enforcing another scaling policy.	| 30 minutes |
| Cluster Size Limit |	The minimum and maximum size allowed for the cluster despite scaling adjustments. Auto scaling policies cannot scale the cluster above or below this size.	 | Min 3 nodes, max 100 nodes | 

>>>>TO-DO: Is this min 3 nodes including or excluding master node? 

To make sure the scaling adjustments don't oversize or undersize your cluster, you can keep the cluster size within defined boundaries using **cluster size min.** and **cluster size max.**.

To avoid stressing the cluster, Cloudbreak uses the **cooldown time period** (in minutes) setting. When an alert is raised and there is an associated scaling policy, the system will not apply the policy until the configured cooldown timeframe has elapsed.

To keep your cluster healthy, Cloudbreak auto scaling runs several background checks during a downscale operation.

* Cloudbreak never removes application master nodes from a cluster. In order to make sure that the node running Ambari Metrics is not removed, Cloudbreak has to be able to access the YARN Resource Manager. When creating a cluster using the default secure network template, make sure that the RM's port is open on that node.  
* In order to keep a healthy HDFS during downscale, Cloudbreak always keeps the replication factor configured and makes sure that there is enough space on HDFS to rebalance data.  
* During downscale, in order to minimize the rebalancing, replication, and HDFS storms, Cloudbreak checks block locations and computes the least costly operations.  
