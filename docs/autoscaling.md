## Autoscaling 

Autoscaling allows you to set up automatic cluster scaling based on Ambari metrics or based on time. When creating an autoscaling policy, you define:

* An **alert** that triggers a scaling policy.   
* A **scaling policy** that adds or removes a set number of nodes to a selected host group when the conditions defined in the attached alert are met.   

**Metric-based Autoscaling**

Cloudbreak accesses all available Ambari metrics and allows you to define alerts based on these metrics. For example:

| Alert Definition | Policy Definition |
|---|---|
| *????* alert with *CRITICAL* state for 5 minutes | Add 10 worker nodes |
| *????* alert with *CRITICAL* state for 10 minutes | Set the number of worker nodes to 30 |
| *????* alert with *CRITICAL* state for 15 minutes | Increase the number of worker nodes by 80% |


[Comment]: <> (Need real examples)

[Comment]: <> (Should we document the list of metrics or will this be something that changes?)

**Time-based Autoscaling**

These alerts can be defined in a cron expression format. For example: 

| Alert Definition | Policy Definition |
|---|---|
| Every day at 07:00 AM (GMT-8) | Add 90 worker nodes | 
| Every day at 08:00 PM (GMT-8) | Remove 90 worker nodes |
 

### Enable Auto Scaling 

For each newly created cluster, autoscaling is disabled by default but it can be enabled once the cluster is in a running state. 

[Comment]: <> (Is it disabled by default?)

> Autoscaling configuration is only available in the UI. It is currently not available in the CLI. 

**Steps**

1. On the cluster details page, navigate to the **Autoscaling** tab.   
3. Click the toggle button to enable autoscaling:

    <a href="../images/cb-autoscaling1.png" target="_blank" title="click to enlarge"><img src="../images/cb-autoscaling1.png" width="650" title="Autoscaling in Cloudbreak UI"></a>  
      
4. The toggle button turns green and you can see that "Autoscaling is enabled".
5. [Define alerts](#defining-an-alert) and [scaling policies](#create-a-scaling-policy). 


### Defining an Alert

After you have enabled autoscaling, define a metric-based or time-based alert.  

#### Define a Metric-based Alert 

To create a metric-based alert, perform the following steps.  

> If you would like to change default thresholds for an Ambari metric, refer to [Modifying Alerts](https://docs.hortonworks.com/HDPDocuments/Ambari-2.6.1.0/bk_ambari-operations/content/modifying_alerts.html) in Ambari documentation. 
> If you would like to create a custom Ambari alert, refer to [How to Create a Custom Ambari Alert and Use it for Cloudbreak Autoscaling Policies](https://community.hortonworks.com/articles/143762/how-to-create-a-custom-ambari-alert-and-use-it-for.html).

**Steps**

1. [Enable autoscaling](#enable-autoscaling).  
2. In the **Alert Configuration** section, **Metric Based** alert type should be preselected:
    <a href="../images/cb-autoscaling2.png" target="_blank" title="click to enlarge"><img src="../images/cb-autoscaling2.png" width="650" title="Autoscaling in Cloudbreak UI"></a>    
3. Enter alert name.
4. Choose metric type, alert status, and alert duration that should trigger a scaling action.   
5. Click **+** to save the alert.  

Once you have defined an alert, [create a scaling policy](#create-a-scaling-policy) that this metric should trigger.

**Related Links:**  
[How to Create a Custom Ambari Alert and Use it for Cloudbreak Autoscaling Policies](https://community.hortonworks.com/articles/143762/how-to-create-a-custom-ambari-alert-and-use-it-for.html) (HCC)   
[Modifying Alerts](https://docs.hortonworks.com/HDPDocuments/Ambari-2.6.1.0/bk_ambari-operations/content/modifying_alerts.html) (Hortonworks)   



#### Define a Time-based Alert 

To create a time-based alert, perform the following steps.

**Steps**

1. [Enable autoscaling](#enable-autoscaling).  
2. In the **Alert Configuration** section, select the **Time Based** alert type:  
    <a href="../images/cb-autoscaling3.png" target="_blank" title="click to enlarge"><img src="../images/cb-autoscaling3.png" width="650" title="Autoscaling in Cloudbreak UI"></a>  
3. Enter alert name.  
4. Select your timezone.   
4. Enter a cron expression to define the frequency of the alert. Refer to [Cron Expression Generator](http://www.cronmaker.com/).  
5. Click **+** to save the alert.   

Once you have defined an alert, [create a scaling policy](#create-a-scaling-policy) that this metric should trigger.


### Create a Scaling Policy 

To create a scaling policy, perform the following steps.

**Steps**

1. [Enable autoscaling](#enable-autoscaling) and [create at least one alert](#defining-an-alert).  
2. In the **Policy Configuration** section, provide the information required:
    <a href="../images/cb-autoscaling4.png" target="_blank" title="click to enlarge"><img src="../images/cb-autoscaling4.png" width="650" title="Autoscaling in Cloudbreak UI"></a>   
3. Enter a unique name for your policy.   
4. Select one of the following actions: Add, Remove, or Set.  
5. Enter a number defining how many or what percentage of nodes to add or remove. If the action selected is "set", this defines the number of nodes that the host group will be set to during scaling.  
6. Select "nodes" or "percent", depending on whether you want to scale to a specific number, or percent of current number of nodes.  
7. Select the host group to which to apply the scaling.  
8. Select the alert based on which the scaling should be applied.   
9. Click **+** to save the alert.   

[Comment]: <> (Are custom metrics supported? AFAIK, yes.)


### Configure Autoscaling Settings 

To configure the auto scaling settings for your cluster, perform these steps. 

**Steps**

1. [Enable autoscaling](#enable-autoscaling).
2. In the **Cluster Scaling Configuration**, provide the following information:
    <a href="../images/cb-autoscaling5.png" target="_blank" title="click to enlarge"><img src="../images/cb-autoscaling5.png" width="650" title="Autoscaling in Cloudbreak UI"></a>   
    
    | Setting | Description	 | Default Value |
|---|---|---|
| Cooldown time  | After an auto scaling event occurs, the amount of time to wait before enforcing another scaling policy. | 30 minutes |
| Minimum Cluster Size |	The minimum size allowed for the cluster. Auto scaling policies cannot scale the cluster below or above this size. | 3 nodes |
| Maximum Cluster Size |	The maximum size allowed for the cluster. Auto scaling policies cannot scale the cluster below or above this size. | 100 nodes |

3. Click **Save** to save the changes. 

