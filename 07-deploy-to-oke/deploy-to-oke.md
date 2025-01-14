# Deploy the application to OKE

## Introduction

This lab provides instructions for deploy the application to OCI Container Engine for Kubernetes (OKE).

Estimated Lab Time: 05 minutes

### Objectives

In this lab, you will:

* Provide the local access to your OKE cluster
* Deploy the application to OKE

## Task 1: Provide the local access to your OKE cluster

1. In the same terminal in VS Code, run the command to create a directory for a `kubectl` configuration:

   ```bash
   <copy>
   mkdir -p $HOME/.kube
   </copy>
   ```

2. Generate a `kubectl` configuration for authentication to OKE:

   ```bash
   <copy>
   oci ce cluster create-kubeconfig \
      --cluster-id $OCI_CLUSTER_ID \
      --file $HOME/.kube/config \
      --region $OCI_REGION \
      --token-version 2.0.0 \
      --kube-endpoint PUBLIC_ENDPOINT
   </copy>
   ```

3. Set `KUBECONFIG` to the created config file, as shown below. (This variable is consumed by `kubectl`.):

   ```bash
   <copy>
   export KUBECONFIG=$HOME/.kube/config
   </copy>
   ```

4. Run the following command to check access.

   ```bash
   <copy>
   kubectl cluster-info
   </copy>
   ```

   The output should look like the following:

   ```txt
   Kubernetes control plane is running at https://<ip-address>:6443
   CoreDNS is running at https://<ip-address>:6443/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy

   To further debug and diagnose cluster problems, use 'kubectl cluster-info dump'.
   ```

## Task 2:  Deploy the application to OKE

1. In the same terminal in VS Code, run the commend to create the Kubernetes namespace. Use placeholders that will be substituted using environment variables.

   ```bash
   <copy>
   envsubst < auth.yml | kubectl apply -f -
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
