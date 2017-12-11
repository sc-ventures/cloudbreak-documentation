| Instance Count | Enter the number of instances of a given type. Default is 1. |
| Ambari Server | You must select one node for Ambari Server. The "Group Size" for that host group must be set to "1". |   


6. On the **Network** page, provide the following to specify the networking resources that will be used for your cluster:

    | Parameter | Description |
|---|---|
| Select Network | Select the virtual network in which you would like your cluster to be provisioned. You can select an existing network or create a new network. |
| Select Subnet | Select the subnet in which you would like your cluster to be provisioned. If you are using a new network, create a new subnet. If you are using an existing network, select an existing subnet. |
| Subnet (CIDR)| If you selected to create a new subnet, you must define a valid [CIDR](http://www.ipaddressguide.com/cidr) for the subnet. Default is 10.0.0.0/16. |

7. Next, define security groups for each host group. For each host group, you can either create a new security group and define its rules or use an existing security group:
