## Using internal hostnames for cluster hosts

Cloudbreak maintains an internal DNS server (using [Unbound](https://nlnetlabs.nl/documentation/unbound/)) with entries for all hosts in the cluster. In this setup, the internal DNS is configured to allow internal communication between the nodes of the same cluster. Queries that the internal DNS cannot resolve are automatically forwarded to the cloud provider’s nameserver. The DNS server is distributed in the sense that each of the VMs runs one server with correct entries, so there is no single point of failure.

By default, the internal FQDNs are inherited from the cloud provider’s DNS server. You can optionally change the hostname prefix and the domain by specifying them as parameters in your cluster CLI JSON. If you would like to specify a custom hostname and a custom domain name, add the following entries to the CLI JSON at the top level of the JSON, replacing the `[$DOMAIN_NAME]` and `[$HOST_NAME_PATTERN]` with actual values:

<pre>
"customDomain": {
    "customDomain": "[$DOMAIN_NAME]",
    "customHostname": "[$HOST_NAME_PATTERN]"
  }
</pre>

For example:

<pre>
"customDomain": {
    "customDomain": "hortonworks.local",
    "customHostname": "test"
  }
</pre>

When these parameters are provided in the CLI JSON, VM FQDNs are generated based on the values specified. For the values included in the example, these FQDNs are `test0.hortonworks.local`, `test1.hortonworks.local`, `test2.hortonworks.local`, and so on.