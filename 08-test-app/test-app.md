# Test the application with OCI Object Storage

## Introduction

This lab provides instructions for testing the application with OCI Object Storage.

Estimated Lab Time: 05 minutes

### Objectives

In this lab, you will:

* Configure external IP of the OCI Load Balancer
* Test the application

## Task 1: Configure external IP of the OCI Load Balancer

1. In the same terminal in VS Code, run the command to retrieve the URL of the microservice and set it as the value of the `IP` environment variable:

   ```bash
   <copy>
   export IP=$(kubectl get svc oci -n=$K8S_NAMESPACE -o json | jq -r .status.loadBalancer.ingress[0].ip)
   </copy>
   ```

2. Confirm the value set by running the following command:

   ```bash
   <copy>
   echo $IP
   </copy>
   ```

## Task 2: Test the application

1. In the same terminal in VS Code, run the commend to 

   ```bash
   <copy>
   curl -i -F "fileUpload=@profile.jpg" http://$IP:8080/pictures/foo
   </copy>
   ```

2. Create an `ocirsecret` secret for authentication to Container Registry using the command below. The secret is a object to store user credential data (encrypted data), for example, the database username and password. In this case, OKE uses it to authenticate to Container Registry to be able to pull microservices container images.

   ```bash
   <code>
   kubectl create secret docker-registry ocirsecret \
      --namespace=$K8S_NAMESPACE \
      --docker-server=$OCI_REGION.ocir.io \
      --docker-username=$OCIR_USERNAME \
      --docker-password=$AUTH_TOKEN
   </code>
   ```

3. Deploy the microservice container image on OKE. Use placeholders that will be substituted using environment variables.

   ```bash
   <code>
   envsubst < k8s.yml | kubectl apply -f
   </code>
   ```

4. Run the following command to check the status of the pods and make sure that all of them have the status “Running”:

   ```bash
   <code>
   kubectl get pods -n=$K8S_NAMESPACE
   </code>
   ```

   Output:
   ```txt
   NAME                      READY   STATUS    RESTARTS   AGE
   api-6fb4cd949f-kxxx8      1/1     Running   0          13s
   orders-595887ddd6-6lzp4   1/1     Running   0          25s
   users-df6f78cd7-lgnzx     1/1     Running   0          37s
   ```

5.  Run this command to check the status of the microservices:

   ```bash
   <code>
   kubectl get services -n=$K8S_NAMESPACE
   </code>
   ```

   Output:
   ```txt
   NAME         TYPE           CLUSTER-IP     EXTERNAL-IP      PORT(S)             AGE
   api          LoadBalancer   10.96.70.48    129.146.149.81   8080:31666/TCP      2m9s
   orders       NodePort       10.96.94.130   <none>           8080:31702/TCP      2m22s
   users        NodePort       10.96.34.174   <none>           8080:31528/TCP      2m33s
   ```

   If the EXTERNAL-IP property of the api service has a <pending> status, wait a few seconds and then run the command again. If the <pending> status persists for more than one minute, try the following:

   1. Verify that a Load Balancer was created.
      a. In the Oracle Cloud Console, open the navigation menu.
      b. Click **Networking**, and then click **Load balancers**.
      c. Click **Load balancer**.
      d. Ensure a Load Balancer has been created for your service.
   2. Check Load Balancer quota (if a Load Balancer was not created, you may have reached the quota limit).
      a. In the Oracle Cloud Console, open the navigation menu.
      b. Click **Governance & Administration**. Under **Tenancy Management**, click **Limits, Quotas and Usage**.
      c. Select the Load Balancer quota. If the quota limit has been reached, request a quota increase or delete unused Load Balancers.

Congratulations! In this section, you successfully deployed the application to OKE.

You may now **proceed to the next lab**.

## Acknowledgements

* **Author** - [](var:author)
* **Contributors** - [](var:contributors)
* **Last Updated By/Date** - [](var:last_updated)
