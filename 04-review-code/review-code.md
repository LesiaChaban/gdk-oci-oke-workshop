# Review the Sample Application Source Code

## Introduction

This lab reviews the sample Micronaut application code used in the workshop. The application source code and build scripts are available for review in VS Code.

Estimated Time: 10 minutes

### Objectives

In this lab, you will:

* Review the sample application source code

## Task 1: Review the Application Configuration

The build file contains the following properties:

_oci/pom.xml_

	```properties
	<properties>
        ...
        <exec.mainClass>com.example.Application</exec.mainClass>
        <!-- Runtime base container image to package the native executable -->
        <micronaut.native-image.base-image-run>frolvlad/alpine-glibc:alpine-3.16</micronaut.native-image.base-image-run>
        <!-- Using the latest GFTC Oracle GraalVM for JDK 17 Native Image container image to build the native executable -->
        <jib.from.image>container-registry.oracle.com/graalvm/native-image:17-ol8</jib.from.image>
        <!-- Using the OCIR Repo to push the generated runtime image containing the native executable -->
        <jib.to.image>${env.OCI_OS_OKE_IMAGE}</jib.to.image>
    </properties>
	```

The build file enable `jib-maven-plugin` for the application:

_oci/pom.xml_

	```properties
	<build>
    <plugins>
      <plugin>
        <groupId>com.google.cloud.tools</groupId>
        <artifactId>jib-maven-plugin</artifactId>
        <configuration>
          <to>
            <!-- <image>${project.name}</image> -->
            <image>${jib.to.image}</image>
          </to>
          <from>
            <image>${jib.from.image}</image>
          </from>
        </configuration>
      </plugin>
	```

## Task 2: Review the Application Dependencies

The build files contains the following dependency to enable OCI OKE Workload Identity Authentication.

_oci/pom.xml_

    <dependency>
        <groupId>io.micronaut.oraclecloud</groupId>
        <artifactId>micronaut-oraclecloud-oke-workload-identity</artifactId>
    </dependency>

_lib/pom.xml_

    <dependency>
        <groupId>io.micronaut.oraclecloud</groupId>
        <artifactId>micronaut-oraclecloud-oke-workload-identity</artifactId>
    </dependency>

## Task 3: Review the auth.yml configuration

_auth.yml_

```
 apiVersion: v1
kind: Namespace # <1>
metadata:
  name: ${K8S_NAMESPACE}
---
apiVersion: v1
kind: ServiceAccount # <2>
metadata:
  namespace: ${K8S_NAMESPACE}
  name: gdk-service-acct
---
kind: Role # <3>
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  namespace: ${K8S_NAMESPACE}
  name: gdk_service_role
rules:
  - apiGroups: [""]
    resources: ["services", "endpoints", "configmaps", "secrets", "pods"]
    verbs: ["get", "watch", "list"]
---
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding # <4>
metadata:
  namespace: ${K8S_NAMESPACE}
  name: gdk_service_role_bind
subjects:
  - kind: ServiceAccount
    name: gdk-service-acct
roleRef:
  kind: Role
  name: gdk_service_role
  apiGroup: rbac.authorization.k8s.io
```

1. `K8S_NAMESPACE` placeholder for a `Namespace` that will be populated by Kubernetes

2. Create a service account named `gdk-service-acct`

3. Create a role named `gdk_service_role`

4. Bind the `gdk_service_role` role to the `gdk-service-acct` service account.

## Task 4: Review the k8s-oci.yml configuration

_k8s-oci.yml_

```
apiVersion: apps/v1
kind: Deployment
metadata:
  namespace: ${K8S_NAMESPACE} # <1>
  name: "oci"
spec:
  selector:
    matchLabels:
      app: "oci"
  template:
    metadata:
      labels:
        app: "oci"
    spec:
      serviceAccountName: gdk-service-acct # <2>
      automountServiceAccountToken: true # <4>
      containers:
        - name: "oci"
          image: ${OCI_OS_OKE_IMAGE} # <3>
          imagePullPolicy: Always # <5>
          env:
          - name: OCI_OS_NS # <6>
            value: ${OCI_OS_NS}
          - name: OCI_OS_BUCKET_NAME # <6>
            value: ${OCI_OS_BUCKET_NAME}
          - name: MICRONAUT_ENVIRONMENTS # <7>
            value: "oraclecloud"
          ports:
            - name: http
              containerPort: 8080
          readinessProbe:
            httpGet:
              path: /health/readiness
              port: 8080
            initialDelaySeconds: 5
            timeoutSeconds: 3
          livenessProbe:
            httpGet:
              path: /health/liveness
              port: 8080
            initialDelaySeconds: 5
            timeoutSeconds: 3
            failureThreshold: 10
      imagePullSecrets: # <8>
        - name: ocirsecret
---
apiVersion: v1
kind: Service
metadata:
  namespace: ${K8S_NAMESPACE} # <i>
  name: "oci"
  annotations: # <ii>
    oci.oraclecloud.com/load-balancer-type: "lb"
    service.beta.kubernetes.io/oci-load-balancer-shape: "flexible"
    service.beta.kubernetes.io/oci-load-balancer-shape-flex-min: "10"
    service.beta.kubernetes.io/oci-load-balancer-shape-flex-max: "10"
spec:
  selector:
    app: "oci"
  type: LoadBalancer
  ports:
    - protocol: "TCP"
      port: 8080
```

1. Add `metadata.namespace` to segregate user-defined resources and to simplify resource cleanup.
2. Add `spec.serviceAccountName` to associate the service account with the pod.
3. Update the `spec.containers[0].image` to point to your image in OCIR.
4. Add `spec.automountServiceAccountToken` and set it to `true`
5. Add `spec.containers[0].imagePullPolicy`.
6. Add `spec.containers[0].env` for the environment variables `OCI_OS_NS` and `OCI_OS_BUCKET_NAME`.
7.
8. Add `imagePullSecrets` for OKE to pull a private container image from OCIR.

You may now **proceed to the next lab**.

## Acknowledgements

* **Author** - [](var:author)
* **Contributors** - [](var:contributors)
* **Last Updated By/Date** - [](var:last_updated)
