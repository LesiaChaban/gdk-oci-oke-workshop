# Build a native executable container image for the application and push it to OCIR

## Introduction

This lab describes how to generate a native executable container image for the application and push it to the OCI Registry.

You will use [GraalVM Native Image](https://docs.oracle.com/en/graalvm/jdk/17/docs/overview/)’s ahead-of-time compilation to build a native executable for the application.

At build time, GraalVM analyzes a Java application and its dependencies to identify just what classes, methods, and fields are absolutely necessary, and then generates a native executable with optimized machine code for just these elements.

Native executables built with GraalVM require less memory, are smaller in size, and start upto 100x faster than just-in-time compiled applications running on a Java Virtual Machine.

Estimated Lab Time: 15 minutes

### Objectives

In this lab, you will:

* Build a native executable for the application
* Push the microservice container image to the OCI Registry

## Task 1: Build a native executable for the application

1. In the same terminal in VS Code, check the version of the GraalVM native-image utility:

	``` bash
	<copy>
	native-image --version
	</copy>
	```

2. To generate a native executable using Maven, run the following command:

	``` bash
	<copy>
	./mvnw clean package -Dpackaging=docker-native -Pgraalvm
	</copy>
	```

   It can take approximately 7-8 minutes to generate the native executable.

## Task 2: Push the microservice container image to the OCI Registry

1. From the same terminal in VS Code run `docker login` to log in Container Registry.

	``` bash
	<copy>
	docker login $OCI_REGION.ocir.io -u $OCIR_USERNAME -p $AUTH_TOKEN
	</copy>
	```

2. Push the microservice container image to the remote repository.

	``` bash
	<copy>
	docker push $OCI_OS_OKE_IMAGE
	</copy>
	```

Congratulations! You've successfully completed this lab. Your microservice container image is now located in the Oracle Container Registry.

You may now **proceed to the next lab**.

## Acknowledgements

* **Author** - [](var:author)
* **Contributors** - [](var:contributors)
* **Last Updated By/Date** - [](var:last_updated)
