| Attached Volumes Per Instance | Enter the number of volumes attached per instance. Default is 1. |
| Volume Size | Enter the size in GBs for each volume. Default is 100. |

6. On the **Network** page, provide the following to specify the networking resources that will be used for your cluster:

    | Parameter | Description |
|---|---|
| Select Network | Select the virtual network in which you would like your cluster to be provisioned. You can select an existing network or create a new network. |
| Select Subnet | Select the subnet in which you would like your cluster to be provisioned. If you are using a new network, create a new subnet. If you are using an existing network, select an existing subnet. |
| Subnet (CIDR)| If you selected to create a new subnet, you must define a valid [CIDR](http://www.ipaddressguide.com/cidr) for the subnet. Default is 10.0.0.0/16. |

    > Cloudbreak uses public IP addresses when communicating with cluster nodes.  
