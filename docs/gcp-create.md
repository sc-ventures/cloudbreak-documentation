## Create a Cluster on GCP

{!docs/common/cluster-basic-conf1.md!}
| **Availability Zone** | Select the availability zone in which you would like to launch your cluster. |
{!docs/common/cluster-basic-conf2.md!}

{!docs/common/cluster-basic-net&blue.md!}

7. On the **Add File System** page, select to use one of the following filesystems:

    * *Local HDFS*: No external storage outside of HDFS will be used
    * *GCS file system*: If you select to use Google Cloud Storage option, you must provide:

        | Parameter | Description |
|---|---|  
| Project Id | The project ID registered when creating a credential should be pre-populated. |
| Service Account Email Address | The email address registered when creating a credential should be pre-populated. |
| Default Bucket Name | (Deprecated) The name of an existing Google Cloud Storage bucket. This is an optional and deprecated configuration parameter (mapped to "fs.gs.system.bucket" in core-site.xml) to set the GCS bucket as a default bucket for URIs without having to specify the "gs:" prefix.  For more information about the [GCS file system](https://cloud.google.com/storage/docs/gcs-fuse) and [bucket naming](https://cloud.google.com/storage/docs/naming#requirements), refer to  GCP documentation. |

{!docs/common/cluster-basic-review.md!}


### Advanced Options

{!docs/common/cluster-adv-1.md!}
{!docs/common/cluster-adv-2.md!}

{!docs/common/cluster-adv-3.md!} 

{!docs/common/cluster-adv-4.md!}

<div class="next">
<a href="../clusters-access/index.html">Next: Access Cluster</a>
</div>

