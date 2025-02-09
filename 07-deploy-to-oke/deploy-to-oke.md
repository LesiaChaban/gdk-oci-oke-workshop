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

1. In the same terminal in VS Code, run the command to create the Kubernetes namespace. Use placeholders that will be substituted using environment variables.

	```bash
	<copy>
	envsubst < auth.yml | kubectl apply -f -
	</copy>
	```

2. Create an `ocirsecret` secret for authentication to Container Registry using the command below. The secret is a object to store user credential data (encrypted data), for example, the database username and password. In this case, OKE uses it to authenticate to Container Registry to be able to pull microservices container images.

	```bash
	<copy>
	kubectl create secret docker-registry ocirsecret \
      --namespace=$K8S_NAMESPACE \
      --docker-server=$OCI_REGION.ocir.io \
      --docker-username=$OCIR_USERNAME \
      --docker-password=$AUTH_TOKEN
	</copy>
	```

3. Deploy the microservice container image on OKE. Use placeholders that will be substituted using environment variables.

	```bash
	<copy>
	envsubst < k8s-oci.yml | kubectl apply -f -
	</copy>
	```

4. Run the following command to check the status of the pods and make sure that all of them have the status `Running`:

	```bash
	<copy>
	kubectl get pods -n=$K8S_NAMESPACE
	</copy>
	```

   Output:
      ```txt
      NAME                   READY   STATUS    RESTARTS   AGE
      oci-7696b44fd5-xsdcj   1/1     Running   0          48s
      ```

5.  Run the command to check the status of the microservices:

	```bash
	<copy>
	kubectl get services -n=$K8S_NAMESPACE
	</copy>
	```

   Output:
      ```txt
      NAME   TYPE           CLUSTER-IP    EXTERNAL-IP      PORT(S)             AGE
      oci    LoadBalancer   10.96.9.53    129.146.149.81   8080:31666/TCP      2m9s
      ```

   If the `EXTERNAL-IP` property of the api service has a "pending" status, wait a few seconds and then run the command again. If the <pending> status persists for more than one minute, try the following:

      1. Verify that a Load Balancer was created:

         a. In the Oracle Cloud Console, open the navigation menu.

         b. Click **Networking**, and then click **Load balancers**.

         c. Click **Load balancer**.

         d. Ensure a Load Balancer has been created for your service.

      2. Check Load Balancer quota (if a Load Balancer was not created, you may have reached the quota limit):

         a. In the Oracle Cloud Console, open the navigation menu.

         b. Click **Governance & Administration**. Under **Tenancy Management**, click **Limits, Quotas and Usage**.

         c. Select the Load Balancer quota. If the quota limit has been reached, request a quota increase or delete unused Load Balancers.

Congratulations! In this section, you successfully deployed the application to OKE.

You may now **proceed to the next lab**.

## Acknowledgements

* **Author** - [](var:author)
* **Contributors** - [](var:contributors)
* **Last Updated By/Date** - [](var:last_updated)
