## Autoscaling 

Autoscaling allows you to define policies to automatically increase or decrease the capacity of the cluster when certain pre-defined conditions are met. 

When creating an autoscaling policy, you define:

* An **alert** that triggers a scaling policy. Alerts can be **Ambari metric-based** or **time-based** (cron expression).  
* A **scaling policy** that adds or removes a set number of worker or compute nodes based on the conditions defined in the attached alert. 

For example:

| Alert Type | Alert Definition | Policy Definition |
|---|---|---|
| Metric based | *NodeManager Health* alert with *CRITICAL* state for 5 minutes | Add 3 worker nodes |
| Metric based | *Ambari Server Alerts* alert with *CRITICAL* state for 30 minutes | Set 0 compute nodes |
| Time based | Every day at 08:00 AM (GMT-8) | Remove 5 worker nodes |
 

### Enabling Auto Scaling 

For each newly created cluster, autoscaling is disabled by default but it can be enabled once the cluster is in a running state. 

> Autoscaling functionality is only available in the UI. It is not available in the CLI. 

**Steps**

1. On the cluster details page, click the **Autoscaling** tab.   
3. Click the toggle button to enable autoscaling.  
4. The toggle button turns green and you can see that "Autoscaling is enabled".


### Defining an Alert

After you have enabled autoscaling, define an alert.

#### Defining a Metric-based Alert 

If you would like to change default thresholds for an Ambari Metric, refer to [Modifying Alerts](https://docs.hortonworks.com/HDPDocuments/Ambari-2.6.1.0/bk_ambari-operations/content/modifying_alerts.html) in Ambari documentation.  

**Steps**

TBD 


#### Defining a Time-based Alert 

**Steps**

TBD

[Cron Expression Generator](http://www.cronmaker.com/)

### Creating a Scaling Policy 

**Steps**

TBD

### Configuring Autoscaling Settings 

**Steps**

TBD

