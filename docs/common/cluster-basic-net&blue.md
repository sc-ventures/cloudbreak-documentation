    > By default, Ambari Username and Ambari Password are set to `admin`. You can override it in the "[Configure Cluster](#configure-cluster)" tab.

5. On the **Set up Network and Security** page, provide the following parameters:

    | Parameter | Description |
|---|---|
| Network | Select the virtual network in which you would like your cluster to be provisioned. You can define custom network configurations or use default network configurations. |
| Enable Knox Gateway | (Optional) Select this option to enable secure access to Ambari web UI and other cluster UIs via Knox gateway. |
| Enable Kerberos Security | (Optional) Select this option to enable Kerberos for your cluster. You will have an option to create a new kerberos or use an existing one. For more information refer to Kerberos [documentation](http://sequenceiq.com/cloudbreak-docs/latest/kerberos/). |

6. On the **Choose Blueprint** page, select the blueprint that you would like to use for your cluster. You can either choose one of the pre-configured blueprints, or add your own in the **manage blueprints** tab.

    For each host group you must provide the following:
    
    | Parameter | Description |
|---|---|
| Group Size | Enter a number defining how many nodes to create per host group. Default is 1. The "Group Size" for that host group on which Ambari Server is installed must be set to "1". |
| Template | If you have previously created a template for VMs and storage, you can select it here. If you don't make a selection, default will be used. |
| Security Group | If you have previously created a template for a security group, you can select it here. If you don't make a selection, default will be used. |
| Ambari Server | You must select one node for Ambari Server. The "Group Size" for that host group must be set to "1". |
| Recipes | You can select a previously added recipe (custom script) to be executed on all nodes of the host group. Refer to [Recipes](recipes.md). |

