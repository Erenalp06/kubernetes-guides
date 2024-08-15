
# Deploying Jaeger on Kubernetes

This document provides a step-by-step guide on how to deploy Jaeger on a Kubernetes cluster using the Jaeger Operator. The deployment includes setting up cert-manager and using the Jaeger Operator to create a Jaeger instance.

## Prerequisites

Before starting, ensure that you have the following:

- A running Kubernetes cluster.
- `kubectl` installed and configured to interact with your Kubernetes cluster.

## Step 1: Install Cert-Manager

Jaeger Operator uses webhooks to validate Jaeger custom resources, which require cert-manager. To install cert-manager, run the following command:

```bash
kubectl apply -f https://github.com/cert-manager/cert-manager/releases/download/v1.9.0/cert-manager.yaml
```

To verify that cert-manager is installed correctly, use the following command:

```bash
kubectl get pods -n cert-manager
```

You should see `cert-manager`, `cert-manager-cainjector`, and `cert-manager-webhook` pods in a `Running` state.

## Step 2: Install Jaeger Operator

You can install the Jaeger Operator using Kubernetes deployment files.

1. First, create a new namespace called `observability`:

    ```bash
    kubectl create ns observability
    ```

2. Install the Jaeger Operator by applying the following deployment file:

    ```bash
    kubectl create -f https://github.com/jaegertracing/jaeger-operator/releases/download/v1.36.0/jaeger-operator.yaml -n observability
    ```

3. Verify that the Jaeger Operator is installed and running:

    ```bash
    kubectl get deployment jaeger-operator -n observability
    ```

    You should see `jaeger-operator` deployment with 1/1 pods available.

## Step 3: Deploy Jaeger All-In-One Instance

Now that the Jaeger Operator is installed, you can create a Jaeger instance.

1. Create a file called `simplest.yaml` with the following content:

    ```yaml
    apiVersion: jaegertracing.io/v1
    kind: Jaeger
    metadata:
      name: simplest
    spec:
      strategy: allInOne
    ```

2. Apply the YAML file to create the Jaeger instance:

    ```bash
    kubectl apply -f simplest.yaml
    ```

3. After a few moments, a new Jaeger instance will be available. You can verify the instance by listing the Jaeger objects:

    ```bash
    kubectl get jaegers
    ```

## Step 4: Access Jaeger UI

Jaeger UI can be accessed via the `simplest-query` service.

1. To get the service details, run:

    ```bash
    kubectl get svc -n observability
    ```

2. Look for the service named `simplest-query`. Note the `NodePort` or `LoadBalancer` IP and port where the UI is exposed.

3. Access Jaeger UI using a web browser at:

    ```plaintext
    http://<NodeIP>:<NodePort>
    ```

Replace `<NodeIP>` and `<NodePort>` with the appropriate values from the `simplest-query` service.

## Conclusion

You have successfully deployed Jaeger on Kubernetes using the Jaeger Operator. You can now use Jaeger for distributed tracing in your applications.
