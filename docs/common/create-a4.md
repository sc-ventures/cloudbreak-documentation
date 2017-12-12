

[Comment]: <> (On AWS/GCP Existing security groups should be selectable only for existing VPC.)

    | Option | Description |
|---|---|
| New Security Group | <p>(Default) Creates a new security group with the rules that you defined:</p><p><ul><li>A set of [default rules](security.md#default-cluster-security-groups) is provided. You should review and adjust these default rules. If you do not make any modifications, default rules will be applied. </li><li>You may open ports by defining the CIDR, entering port range, selecting protocol and clicking **+**.</li><li>You may delete default or previously added rules using the delete icon.</li><li>If you don't want to use security group, remove the default rules.</li><ul></p> |  
| Existing Security Groups | Allows you to select an existing security group that is already available in the selected provider region. This selection is disabled if no existing security groups are available in your chosen region. |  

    <div class="danger">
<p class="first admonition-title">Important</p>
<p class="last">
By default, ports 22, 443, and 9443 are set to 0.0.0.0/0 CIDR for inbound access on the Ambari node security group. We strongly recommend that you limit this CIDR, considering the following restrictions:
<ul><li>Ports 22 and 9443 must be open to Cloudbreak's CIDR. You can set CB_DEFAULT_GATEWAY_CIDR in your Cloudbreak's Profile file in order to automatically open ports 22 and 9443 to your Cloudbrak IP. Refer to <a href="../security-cb/index.html#restricting-inbound-access-to-clusters">Restricting Inbound Access to Clusters</a>.</li>
<li>Port 22 must be open to your CIDR if you would like to access the master node via SSH.</li>
<li>Port 443 must be open to your CIDR if you would like to access Cloudbreak web UI in a browser.</li></ul>  
</p>
</div>

    <div class="danger">
<p class="first admonition-title">Important</p>
<p class="last">
By default, port 22 is set to 0.0.0.0/0 CIDR for inbound access on non-Ambari node security groups. We strongly recommend that you remove it.</p>
</div>


5. On the **Security** page, provide the following parameters:

    | Parameter | Description |
|---|---|
| Cluster User | You can log in to the Ambari UI using this username. By default, this is set to `admin`. |
| Password | You can log in to the Ambari UI using this password. |
| Confirm Password | Confirm the password. |