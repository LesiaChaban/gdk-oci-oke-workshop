# Provision an OCI Autonomous Database

## Introduction

This lab describes the steps to create a new OCI Kubernetes Cluster(OKE), Container Registry and an Object Storage Bucket.

Estimated Lab Time: 10 minutes

### Objectives

In this lab, you will:

* Create a Kubernetes Cluster(OKE)
* Create a Container Registry
* Create an Object Storage Bucket

## Task 1: Create a Kubernetes Cluster(OKE)

1. From the Oracle Cloud Console, open the navigation menu, click **Developer Services >> Containers & Artifacts** and then click **Kubernetes Clusters (OKE)**.

   ![Oracle Kubernetes Clusters (OKE) menu](https://oracle-livelabs.github.io/common/images/console/ds-kebernetes-clusters.png)

2. Select your workshop compartment from the **Compartment** drop down list on the left. To find the compartment name, return to the **Login Details** screen, then copy the value of the **Compartment Name**, paste it in the **Compartment** drop down list in the Oracle Cloud Console and select the filtered compartment.

    ![Containers & Artifacts Landing Page](images/compartment-name-oke.png#input)

3. Click **Create cluster**.

    ![Create cluster Button](images/create-cluster.png#input)

4. You will see the **Create cluster** screen with default values, select **Quick Create**, click **Submit**

    ![Create cluster Info](images/create-cluster-info.png#input)

## Task 2: Create a Container Registry

1. From the Oracle Cloud Console, open the navigation menu, click **Developer Services >> Containers & Artifacts** and then click **Container Registry**.

   ![Oracle Container Registry menu](https://oracle-livelabs.github.io/common/images/console/ds-kebernetes-clusters.png)

2. Select your workshop compartment from the **Compartment** drop down list on the left. To find the compartment name, return to the **Login Details** screen, then copy the value of the **Compartment Name**, paste it in the **Compartment** drop down list in the Oracle Cloud Console and select the filtered compartment.

    ![Containers & Artifacts Landing Page](images/compartment-name-container.png#input)

3. Click **Create repository**.

    ![Create repository Button](images/create-repository.png#input)

4. You will see the **Create repository** screen with default values. Check the **Compartment Name**, select "Private" in  the **Access**, paste "gdk-oke" to the **Repository Name** field, click **Create**.

    ![Create repository Info](images/create-repository-info.png#input)

## Task 3: Create an Object Storage Bucket

1. From the Oracle Cloud Console, open the navigation menu, click **Storage**. Under **Object Storage & Archive Storage**, click **Buckets**.

   ![Object Storage buckets menu](https://oracle-livelabs.github.io/common/images/console/storage-buckets.png)

2. Select your workshop compartment from the **Compartment** drop down list on the left. To find the compartment name, return to the **Login Details** screen, then copy the value of the **Compartment Name**, paste it in the **Compartment** drop down list in the Oracle Cloud Console and select the filtered compartment.

   ![Buckets Landing Page](images/buckets-landing-page.png)

3. Click **Create Bucket**.

4. On the **Create Bucket** screen, leave the default values unchanged and click **Create**.

   **Note:** OCI Object Storage bucket names are case-sensitive and must be unique in the tenancy.

   ![Create Bucket](images/create-bucket.jpg)

   The bucket gets created in a few seconds. The next task describes how to view the bucket details.

5. On the bucket details screen, scroll down to the **Objects** list. There are no objects listed because the bucket is empty.

   ![Bucket details and Objects list](images/objects-list.jpg)

6. Note down the bucket name. You will need it in the next lab.

Congratulations! In this lab, you created a new OCI Kubernetes Cluster(OKE), Container Registry and an Object Storage Bucket in your workshop compartment.

You may now **proceed to the next lab**.

## Acknowledgements

* **Author** - [](var:author)
* **Contributors** - [](var:contributors)
* **Last Updated By/Date** - [](var:last_updated)
