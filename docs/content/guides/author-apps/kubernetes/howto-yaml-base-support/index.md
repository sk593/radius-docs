---
type: docs
title: "How-To: Incremental onboard from existing Kubernetes resources"
linkTitle: "Incremental onboard with YAML"
description: "Learn how to load a Kubernetes YAML deployment manifest into Radius and have Radius override properties on top of the user-given base resources"
weight: 200
categories: "How-To"
tags: ["containers","Kubernetes"]
---

This guide will provide an overview of how to:

- Incrementally onboard your Kubernetes resources using your Kubernetes [Deployment YAML](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/#writing-a-deployment-spec) definitions

### Prerequisites

Before you get started, you'll need to make sure you have the following tools and resources:

- [kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/)
- [rad CLI]({{< ref getting-started >}})
- [Radius initialized with `rad init`]({{< ref howto-environment >}})

### Step 1: Define a Radius application and container

Define a Radius Application resource that will contain your Kubernetes resource container.

> Radius resources, when deployed to a Kubernetes environment, are mapped to one or more Kubernetes objects. For more information, visit [Radius on Kubernetes platform]({{< ref "guides/operations/kubernetes#resource-mapping" >}}).

{{< rad file="snippets/basemanifest.bicep" embed=true marker="//APPLICATION" >}}

### Step 2: Define a base Kubernetes YAML file

You'll need to create a Radius container resource that will consume the information found in your Kubernetes base YAML file.

#### YAML Deployment template

{{< rad file="snippets/basemanifest.yaml" embed=true lang="yaml">}}

### Step 3: Add the Kubernetes base YAML configurations to your Radius application definition

- Standardize object names

Begin by assuring that your `ServiceAccount`, `Deployment`, and `Service` objects found in your Kubernetes base YAML file have the same name as the Radius container resource that lives inside your application-scoped namespace.

- Container configurations

Define your Radius container resource port with the port of your Kubernetes `Service` as well as the image container location that your service is dependent on.

- Runtime configuration

Load your Kubernetes YAML file into your Radius container resource through the `properties.runtimes.kubernetes.base` property which expects a string value containing all your Kubernetes data.

{{< rad file="snippets/basemanifest.bicep" embed=true marker="//CONTAINER" >}}

You can now deploy your Radius Application and verify that your Kubernetes objects are created in the desired namespace. 

### Step 4: Deployment


1. Deploy your application with the command:

    ```bash
    rad deploy app.bicep
    ```

2. Your console output should look similar to:

```
Building app.bicep...
Deploying template 'app.bicep' into environment 'default' from workspace 'default'...

Deployment In Progress...

Completed            my-microservice Applications.Core/applications
..                   my-microservice Applications.Core/containers

Deployment Complete

Resources:
    my-microservice Applications.Core/applications
    my-microservice Applications.Core/containers
```

To verify and follow the Kubernetes pod creations run `kubectl get pods -A -w` during your deployment and the following output should be seen:

```
radius-system     controller-585dcd4c9b-5g2c9        1/1     Running            5 (91s ago)     13m
3. Run the following command to verify and follow the Kubernetes pod creations: 
    ```bash
    kubectl get pods -A -w
    ```
    4. Your console output should look similar to:
```bash
radius-system   controller-585dcd4c9b-5g2c9        1/1     Running            5 (91s ago)     13m
my-microservice   my-microservice-5c464f66d4-s7n7w   0/1     Pending            0               0s
my-microservice   my-microservice-5c464f66d4-s4tq8   0/1     Pending            0               0s
my-microservice   my-microservice-5c464f66d4-tnvx4   0/1     Pending            0               0s
my-microservice   my-microservice-5c464f66d4-s7n7w   0/1     Pending            0               0s
my-microservice   my-microservice-5c464f66d4-tnvx4   0/1     Pending            0               0s
```

## Further reading

- [Kubernetes authoring resources overview]({{< ref "/guides/author-apps/kubernetes/overview" >}})
- [Container resource overview]({{< ref "guides/author-apps/containers/overview#kubernetes" >}})
- [Container resource schema]({{< ref "container-schema#runtimes" >}})