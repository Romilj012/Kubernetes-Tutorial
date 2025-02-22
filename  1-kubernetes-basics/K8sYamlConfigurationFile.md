3 parts of Configuration file

# Kubernetes Configuration File

A configuration file in Kubernetes has three parts:

1. **Metadata**
2. **Specification**
3. **Status**

## Metadata

The first part is where the metadata of the component that you're creating resides. One of the metadata is obviously the name of the component itself.

![alt text](k8s-basics-images/image-57.png)

## Specification

The second part in the configuration file is the specification. Each component's configuration file will have a specification where you basically put every kind of configuration that you want to apply for that component. The first two lines here, as you see, are just declaring what you want to create. Here we are creating a deployment and a service. Each component has a different API version.

![alt text](k8s-basics-images/image-58.png)

Inside the specification part, the attributes will be specific to the kind of component that you're creating. For example, a deployment will have its own attributes that only apply to deployments, and a service will have its own attributes.

![alt text](k8s-basics-images/image-77.png)

![alt text](k8s-basics-images/image-78.png)

## Status

The third part is the status, which is automatically generated and added by Kubernetes. Kubernetes will always compare the desired state and the actual state (or status) of the component. If the status and desired state do not match, Kubernetes knows there's something to be fixed and will try to fix it. This is the basis of the self-healing feature that Kubernetes provides.

For example, if you specify you want two replicas of an nginx deployment, Kubernetes will adhere to the status of your deployment and update that state continuously. If at some point the status says only one replica is running, Kubernetes will compare that status with the specification and know there is a problem, and another replica needs to be created.

![alt text](k8s-basics-images/image-79.png)

![alt text](k8s-basics-images/image-80.png)

Kubernetes gets the status data to automatically adhere or update continuously from etcd, which is one of the master processes that stores the cluster data. Etcd holds the current state of any Kubernetes component at any time, and that's where the status information comes from.

![alt text](k8s-basics-images/image-81.png)

![alt text](k8s-basics-images/image-82.png)

![alt text](k8s-basics-images/image-59.png)


## YAML Configuration

The format of the configuration files is YAML, which is straightforward to understand but very strict about indentations. If something is wrongly indented, the file will be invalid. It's a good practice to use an online YAML validator for long configuration files.

![alt text](k8s-basics-images/image-60.png)

### Storing Configuration Files

Another thing is where do you actually store those configuration files. A usual practice is to store them with your code because since the deployment and service are going to be applied to your application, it's a good practice to store these configuration files in your application code. This is part of the whole infrastructure as a code concept, or you can also have its own git repository just for the configuration files.

### Template in Specification

In the specification part of a deployment, you see a template. The template also has its own metadata and specification, so it's basically a configuration file inside of a configuration file. This configuration applies to a pod, so the pod should have its own configuration inside of the deployment's configuration file. This is how all the deployments will be defined, and this is going to be the blueprint for a pod, like which image it should be based on, which port it should open, what is going to be the name of the container, etc.


![alt text](k8s-basics-images/image-61.png)

![alt text](k8s-basics-images/image-62.png)

### Labels and Selectors

The connection is established using labels and selectors. The metadata part contains the labels, and the specification part contains selectors. In the metadata, you give components like deployment or pod a key-value pair. For example, we have `app: nginx`, and that label just sticks to that component. We give pods created using this blueprint the label `app: nginx`, and we tell the deployment to connect or to match all the labels with `app: nginx` to create that connection. This way, the deployment will know which pods belong to it.

![alt text](k8s-basics-images/image-63.png)

![alt text](k8s-basics-images/image-64.png)


### Applying Configuration Files

Once we have those files, we can apply them or create components using them. For example, to create both deployment and service, we use the command:

```sh
kubectl apply -f <filename>
```

This will create the deployment and the nginx service. We can then check the pods and services using:

```sh
kubectl get pods
kubectl get services
```

![alt text](k8s-basics-images/image-65.png)

![alt text](k8s-basics-images/image-66.png)


### Creating and Managing Pods

Whenever you want to create some pods, you would actually create a deployment, and it will take care of the rest.


### Deployment and Service Configuration

In the specification of a service, we define a selector which makes a connection between the service and the deployment or its pods. The service must know which pods belong to it, and this connection is made through the selector of the label.



### Ports Configuration in Service and Pod


Another thing that must be configured in the service and pod is the ports. The service has a port where the service itself is accessible. If other services send a request to the nginx service, it needs to send it on port 80. The service needs to know to which pod it should forward the request and at which port the pod is listening. This is the target port, which should match the container port.

![alt text](k8s-basics-images/image-67.png) 

![alt text](k8s-basics-images/image-68.png)

![alt text](k8s-basics-images/image-69.png)

![alt text](k8s-basics-images/image-70.png)

![alt text](k8s-basics-images/image-71.png)


### Validating Service Endpoints

To validate that the service has the right pods that it forwards the requests to, use:

```sh
kubectl describe service <service-name>
```

![alt text](k8s-basics-images/image-72.png)

This will show the endpoints, which are the IP addresses and ports of the pods that the service must forward the request to. Match these IP addresses with the pods using:

```sh
kubectl get pods -o wide
```
![alt text](k8s-basics-images/image-73.png)

![alt text](k8s-basics-images/image-74.png)

### Status Part of Configuration

The status part of the configuration file is automatically generated and updated by Kubernetes. Get the deployment status in YAML format using:

```sh
kubectl get deployment <deployment-name> -o yaml
```

This will show the updated configuration of the deployment, including the status part, which contains information like how many replicas are running and the state of those replicas.

![alt text](k8s-basics-images/image-75.png)

![alt text](k8s-basics-images/image-76.png)


### Cleaning Configuration Files

If you want to copy a deployment that you already have using automated scripts, you will have to remove most of the generated stuff from the configuration file. Clean the deployment configuration file first, and then you can create another deployment from that blueprint configuration.


### Deleting Components

To delete the deployment and the service, you can use the configuration file as well using:

```sh
kubectl delete -f <filename>
```

This will delete the deployment and the service.
