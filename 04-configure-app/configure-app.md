# Configure the application to use OCI Monitoring

## Introduction

This lab provides instructions to configure the application to point to OCI Monitoring.

Estimated Lab Time: 05 minutes

### Objectives

In this lab, you will:

* Configure the Application to Use the OCI Monitoring

## Task 1: Configure the Application to Use the OCI Monitoring

1. From the Oracle Cloud Console, open the navigation menu, navigate to  **Identity & Security >> Compartments** , find your workshop compartment in the list, open it, in the **Compartment Information** section click **Copy** to copy the value of the compartment OCID.

2. Open a new terminal in VS Code using the **Terminal > New Terminal** menu.

3. Set the environment variable `COMPARTMENT_OCID` using the compartment OCID you copied.

	```
	<copy>
	export COMPARTMENT_OCID=ocid1.compartment.oc1...
	</copy>
	```

4. Confirm the value set by running the following command:

	```
	<copy>
	echo $COMPARTMENT_OCID
	</copy>
	```


Congratulations! In this lab, you configured the application to publish metrics to OCI Monitoring.

You may now **proceed to the next lab**.

## Acknowledgements

* **Author** - [](var:author)
* **Contributors** - [](var:contributors)
* **Last Updated By/Date** - [](var:last_updated)
