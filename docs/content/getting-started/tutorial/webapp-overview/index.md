---
type: docs
title: "Website tutorial app overview"
linkTitle: "App overview"
description: "Overview of the website tutorial application"
weight: 1000
slug: "overview"
---

You will be deploying an application, `todoapp`, with the following resources:

1. [`frontend`](#frontend-container): A containerized to-do list frontend written in Node.JS
1. [`frontend-route`](#todoroute-httproute): An [HttpRoute]({{< ref httproute >}}) that declares the connection to the frontend container
1. [`gateway`](#gateway-gateway): A [Gateway]({{< ref gateway >}}) that exposes the application to the internet
1. [`db`](#db-connector): A [MongoDB connector]({{< ref mongodb >}}) to save to-do items in. Can be backed by either:
   - A MongoDB container
   - An Azure CosmosDB w/ Mongo API

<img src="todoapp-diagram.png" width=700 alt="Simple app diagram">

## Resources

### `frontend` container

The example website is a single-page-application (SPA) with a Node.JS backend running in a [container]({{< ref container >}}). The SPA sends HTTP requests to the Node.JS backend to read and store todo items.

The website listens on port 3000 for HTTP requests.

The website uses the MongoDB protocol to read and store data in a database. The website reads the environment variable `CONNECTION_ITEMSTORE_CONNECTIONSTRING` to discover the database connection string. If the connection string is not set the website will store the todo items in memory and not persist them.

#### (optional) Download the source code

You can view and download the source code in the [samples repo](https://github.com/project-radius/samples). For access fill out [this form](https://aka.ms/ProjectRadius/GitHubAccess).

### `frontend-route` HttpRoute

An [HttpRoute]({{< ref httproute >}}) is used to define communication to the container app `frontend`.

### `gateway` Gateway

In order for users to connect to `todoapp` over the internet, a [Gateway]({{< ref gateway >}}) is used to define the appropriate route paths.

### `db` Connector

The database is provided by a [MongoDB connector]({{< ref mongodb >}}). You can choose between a MongoDB container or an Azure CosmosDB w/ Mongo API to back the connector.

## The Radius mindset

In this tutorial you will build out this app using the Radius application model. As you progress, keep in mind the following benefits that it provides:

- Both infrastructure (`db`) and services (`frontend`) are modeled as part of the application within the same template
- Relationships between resources are fully specified with protocols and other strongly-typed information
- Connectors provide abstraction and portability across local and cloud environments

<br>{{< button text="Next: Deploy the website frontend" page="webapp-initial-deployment" >}}