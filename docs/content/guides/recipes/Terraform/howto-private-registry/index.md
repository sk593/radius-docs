---
type: docs
title: "How-To: Setup a private registry for your Terraform Radius Recipes"
linkTitle: "Setup a private registry"
description: "Learn how to configure your Radius Environment to leverage your private registry as storage for your custom Recipe templates"
weight: 500
categories: "How-To"
tags: ["recipes", "terraform"]
---

This how-to guide will provide an overview of how to:

- Configure a Radius Environment that leverages a [Radius secret store]({{< ref secretstore >}}) to register a [private Git registry](https://developer.hashicorp.com/terraform/registry/private) for your Recipe templates.

### Prerequisites

Before you get started, you'll need to make sure you have the following tools and resources:

- [rad CLI]({{< ref "installation#step-1-install-the-rad-cli" >}})
- [Radius Bicep VSCode extension]({{< ref "installation#step-2-install-the-vs-code-extension" >}})
- [kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/)

### Step 1: Define a secret store resource

Radius provides support for defining Kubernetes Secrets as Bicep resources, through the Radius Secret Store. Users have the option to both leverage an existing Kubernetes Secret or define a new Kubernetes Secret in the resource, see the [Radius Secret Store schema]({{< ref secretstore >}}) for more information.

Create a Bicep file `env.bicep` and define your resource:

{{< rad file="snippets/env.bicep" embed=true marker="//SECRETSTORE" >}}

## Step 2: Define your Radius Recipe configurations

Radius provides a property `recipeConfig` which allows users to setup specific Git module sources for your Terraform Radius Recipes. Define this property in your Radius Environment resource, visit the [Radius Environment schema]({{< ref environment-schema >}}) page for more information.

{{< rad file="snippets/env.bicep" embed=true marker="//ENV" >}}

## Step 3: Deploy your Radius Environment

Deploy your new Radius Environment:

```
rad deploy env.bicep
```

## Cleanup

You can delete a Radius Environment by running the following command:

```
rad env delete prod
```

## Further reading

- [Recipes overview]({{< ref "/guides/recipes/overview" >}})
- [Radius Environments]({{< ref "/guides/deploy-apps/environments/overview" >}})
- [`rad recipe CLI reference`]({{< ref rad_recipe >}})
- [`rad env CLI reference`]({{< ref rad_env >}})