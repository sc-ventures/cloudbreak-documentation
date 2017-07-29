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
| Default Bucket Name | ????  For more information about the [GCS file system](https://cloud.google.com/storage/docs/gcs-fuse) and [bucket naming](https://cloud.google.com/storage/docs/naming#requirements), refer to  GCP documentation. |

    >>>>TO-DO: Regarding "Default Bucket Name", is this an existing bucket or will a new bucket be created? How exactly will it be used "by default?"

{!docs/common/cluster-basic-review.md!}


### Advanced Options

{!docs/common/cluster-adv-1.md!}
{!docs/common/cluster-adv-2.md!}

{!docs/common/cluster-adv-3.md!} 

{!docs/common/cluster-adv-4.md!}

<div class="next">
<a href="../gcp-cb-ui/index.html">Next: Manage & Monitor Clusters</a>
</div>