# Cleanup

## Introduction

From the Oracle Cloud Console, clean up the resources provisioned for this workshop.

Estimated Workshop Time: 05 minutes

### Objectives

In this lab, you will:

* Delete the Kubernetes namespace
* Delete the image from the private OCIR repo
* Delete the private OCIR repo
* Delete OKE cluster
* Delete Object Storage bucket
* Destroy Stack
* Delete the Stack
* Delete the OKE Workload Identity policy
* (Optional) Purge local docker resources

## Task 1: Cleanup

From the Oracle Cloud Console, clean up the resources provisioned for this workshop:

1. From the same terminal in VS Code, run the command to delete the Kubernetes namespace.

    ```bash
    <copy>
    kubectl delete ns $K8S_NAMESPACE
    </copy>
    ```

2. From **Developer Services** >> **Container Registry** >> search for your repo >> **images**, use **Delete Image** to delete image in OCIR repository.

3. From **Developer Services** >> **Container Registry** >> search for your repo >> use **Delete repository** to delete the OCIR repository.

4. From **Developer Services** >> **Kubernetes Clusters (OKE)** >> navigate to your cluster details section >> use **Delete** to delate it.

5. From **Storage >> Object Storage & Archive Storage >> Buckets**, delete the **Bucket**.

6. From **Resource Manager >> Stacks >> Stack Details** screen, run **Destroy** to delete the VCN and the Compute instance.

7. From **Resource Manager >> Stacks >> Stack Details** screen, **Delete** the stack.

8. From **Identity & Security >> Identity >> Policies**, delete the Workload Identity policy.

9. (Optional) From the same terminal in VS Code, run the command to purge all local docker resources (images, containers, volumes, etc.)

    ```bash
    <copy>
    docker system prune -a
    </copy>
    ```

    Check it:

    ```bash
    <copy>
    docker system df
    </copy>
    ```

Congratulations! You've successfully completed this lab.

You may now **proceed to the next lab**.

## Acknowledgements

* **Author** - [](var:author)
* **Contributors** - [](var:contributors)
* **Last Updated By/Date** - [](var:last_updated)