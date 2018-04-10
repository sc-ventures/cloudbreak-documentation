## Autoscaling 

Autoscaling allows you to adjust cluster capacity based on Ambari metrics and alerts, as well as schedule time-based capacity adjustment. When creating an autoscaling policy, you define:

* An **alert** that triggers a scaling policy. An alert can be based on an Ambari metric or can be time-based.     
* A **scaling policy** that adds or removes a set number of nodes to a selected host group when the conditions defined in the attached alert are met.    

**Metric-based autoscaling**

Cloudbreak accesses all available Ambari metrics and allows you to define alerts based on these metrics. For example:

| Alert Definition | Policy Definition |
|---|---|
| *ResourceManager CPU* alert with *CRITICAL* status for 5 minutes | Add 10 worker nodes |
| *HDFS Capacity Utilization* alert with *WARN* status for 20 minutes | Set the number of worker nodes to 50 |
| *Ambari Server Alerts* alert with *CRITICAL* status for 15 minutes | Decrease the number of worker nodes by 80% |


**Time-based autoscaling**

Time-based alerts can be defined by providing a cron expression. For example: 

| Alert Definition | Policy Definition |
|---|---|
| Every day at 07:00 AM (GMT-8) | Add 90 worker nodes | 
| Every day at 08:00 PM (GMT-8) | Remove 90 worker nodes |
 

### Enable auto scaling 

For each newly created cluster, autoscaling is disabled by default but it can be enabled once the cluster is in a running state. 

[Comment]: <> (Is it disabled by default?)

> Autoscaling configuration is only available in the UI. It is currently not available in the CLI. 

**Steps**

1. On the cluster details page, navigate to the **Autoscaling** tab.   
3. Click the toggle button to enable autoscaling:

    <a href="../images/cb_cb-autoscaling1.png" target="_blank" title="click to enlarge"><img src="../images/cb_cb-autoscaling1.png" width="650" title="Autoscaling in Cloudbreak UI"></a>  
      
4. The toggle button turns green and you can see that "Autoscaling is enabled".   
5. [Define alerts](#defining-an-alert) and then [define scaling policies](#create-a-scaling-policy). You can also [adjust the autoscaling settings](#configure-autoscaling-settings). 

If you decide to disable autoscaling, your previously defined alerts and policies will be preserved. 


### Defining an alert

After you have enabled autoscaling, define a metric-based or time-based alert.  


#### Define a metric-based alert 

After [enabling autoscaling](#enable-autoscaling), perform the following steps to create a metric-based alert.  

> If you would like to change default thresholds for an Ambari metric, refer to [Modifying alerts](https://docs.hortonworks.com/HDPDocuments/Ambari-2.6.1.0/bk_ambari-operations/content/modifying_alerts.html) in Ambari documentation. 

> If you would like to create a custom Ambari alert, refer to [How to create a custom Ambari alert and use it for Cloudbreak autoscaling policies](https://community.hortonworks.com/articles/143762/how-to-create-a-custom-ambari-alert-and-use-it-for.html).

**Steps**

1. In the **Alert Configuration** section, select **Metric Based** alert type.      
2. Provide the following information:

    | Parameter | Description |
|---|---|
| Enter alert name | Enter a unique name for the alert. | 
| Choose metric type | Select the Ambari metric that should trigger the alert. |
| Alert status | Select the alert status that should trigger an alert for the selected metric. One of: OK, CRITICAL, WARNING. | 
| Alert duration | Select the alert duration that should trigger an alert. |   

3. Click **+** to save the alert.  

Once you have defined an alert, [create a scaling policy](#create-a-scaling-policy) that this metric should trigger.

**Related links:**  
[How to create a custom Ambari alert and use it for Cloudbreak autoscaling policies](https://community.hortonworks.com/articles/143762/how-to-create-a-custom-ambari-alert-and-use-it-for.html) (HCC)   
[Modifying alerts](https://docs.hortonworks.com/HDPDocuments/Ambari-2.6.1.0/bk_ambari-operations/content/modifying_alerts.html) (Hortonworks)   



#### Define a time-based alert 

After [enabling autoscaling](#enable-autoscaling), perform the following steps to create a time-based alert.

**Steps**
 
1. In the **Alert Configuration** section, select the **Time Based** alert type. 
2. Provide the following information: 

    | Parameter | Description |
|---|---|
| Enter alert name. |  Enter a unique name for the alert. | 
| Select timezone. | Select your timezone. |   
| Enter cron expression | Enter a cron expression that defines the frequency of the alert. Refer to [Cron expression generator](http://www.cronmaker.com/). | 

3. Click **+** to save the alert.   

Once you have defined an alert, [create a scaling policy](#create-a-scaling-policy) that this metric should trigger.


### Create a scaling policy 

After [enabling autoscaling](#enable-autoscaling) and [creating at least one alert](#defining-an-alert), perform the following steps to create a scaling policy.

**Steps**

1. In the **Policy Configuration** section, provide the following information:

    | Parameter | Description |
|---|---| 
| Enter policy name | Enter a unique name for the policy. | 
| Select action | Select one of the following actions: Add (to add nodes to a host group) Remove (to delete nodes from a host group), or Set (to set the number of nodes in a host group to the chosen number). | 
| Enter number or percentage | Enter a number defining how many or what percentage of nodes to add or remove. If the action selected is "set", this defines the number of nodes that a host group will be set to after scaling. |  
| Select nodes of percent | Select "nodes" or "percent", depending on whether you want to scale to a specific number, or percent of current number of nodes.  |
| Select host group | Select the host group to which to apply the scaling. | 
| Choose an alert | Select the alert based on which the scaling should be applied. |   

9. Click **+** to save the alert.   



### Configure autoscaling settings 

After [enabling autoscaling](#enable-autoscaling), perform these steps to configure the auto scaling settings for your cluster.   

**Steps**

1. In the **Cluster Scaling Configuration**, provide the following information: 
    
    | Setting | Description	 | Default Value |
|---|---|---|
| Cooldown time  | After an auto scaling event occurs, the amount of time to wait before enforcing another scaling policy. | 30 minutes |
| Minimum Cluster Size |	The minimum size allowed for the cluster. Auto scaling policies cannot scale the cluster below or above this size. | 2 nodes |
| Maximum Cluster Size |	The maximum size allowed for the cluster. Auto scaling policies cannot scale the cluster below or above this size. | 100 nodes |

2. Click **Save** to save the changes. 


