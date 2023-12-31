## Kubernetes cluster locally

You can run a Kubernetes cluster in your local development environments using build-in Kubernetes instance of the Docker Desktop.
This solution should be used for local testing only. You can test your application during all development phases.

### Enable Kubernetes in Docker Desktop

Docker Desktop includes a standalone [Kubernetes](https://docs.docker.com/desktop/kubernetes/) server and client, as well as Docker CLI integration that runs on your machine.

To enable Kubernetes in Docker Desktop:
1. From the **Docker Dashboard**, select the **Settings**.
2. Select **Kubernetes** from the left sidebar.
3. Next to **Enable Kubernetes**, select the checkbox.
4. Select **Apply & Restart** to save the settings and then click **Install** to confirm.

If you already have **kubectl** installed and it is pointing to some other environment, such as minikube or a GKE cluster, ensure you change the context so that **kubectl** is pointing to **docker-desktop**:

```sh
$ kubectl config get-contexts
$ kubectl config use-context docker-desktop
```

### Build an image from a Dockerfile

During the deployment of an application to a Kubernetes cluster, you'll typically want one or more images to be pulled from a Docker registry. But as long as we are not able to push our application images to the Azure registry, we will use the **docker build** command to locally build Docker image of **opportunity-dashboard-service** from the existing Dockerfile und use it without pushing it to the external Docker Registry.

From the main folder **/opportunity-dashboard** run the following commands:

```sh
$ docker build -t akros/od-web-backend:0.0.1 -f opportunity-dashboard-service/Dockerfile .
$ docker image ls
```

### Apply a configuration to a resource

We will apply a configuration to a resource from the "kube" directory, that includes definitions of a Deployment and a Service for the backend application and database.
From the service folder **/opportunity-dashboard/opportunity-dashboard-service** submit your resource definitions to Kubernetes with the following command:

```sh
$ kubectl apply -f kube
```

Check your running pods:

```sh
$ kubectl get all
```

Delete pods:

```sh
$ kubectl delete -f kube
```

### Azure Kubernetes Service

The next step will be deploying a managed Kubernetes cluster in Azure.

For this purpose do not forget to change the **imagePullPolicy** in the definition of a Deployment in  **/opportunity-dashboard/opportunity-dashboard-service/kube/od-backend.yaml** file.

The imagePullPolicy specification lets one specify how the Kubelet should handle referenced images if there's any change.
