---
title: Visualizing your cluster using Service Fabric Explorer | Microsoft Docs
description: Service Fabric Explorer is a web-based tool for inspecting and managing cloud applications and nodes in a Microsoft Azure Service Fabric cluster.
services: service-fabric
documentationcenter: .net
author: seanmck
manager: timlt
editor: ''

ms.assetid: c875b993-b4eb-494b-94b5-e02f5eddbd6a
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/05/2017
ms.author: seanmck

---
# Visualize your cluster with Service Fabric Explorer
Service Fabric Explorer is a web-based tool for inspecting and managing applications and nodes in an Azure Service Fabric cluster. Service Fabric Explorer is hosted directly within the cluster, so it is always available, regardless of where your cluster is running.

## Video tutorial

To learn how to use Service Fabric Explorer, watch the following Microsoft Virtual Academy video:

[<center><img src="./media/service-fabric-visualizing-your-cluster/SfxVideo.png" WIDTH="360" HEIGHT="244"></center>](https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=bBTFg46yC_9806218965)

## Connect to Service Fabric Explorer
If you have followed the instructions to [prepare your development environment](service-fabric-get-started.md), you can launch Service Fabric Explorer on your local cluster by navigating to http://localhost:19080/Explorer.

> [!NOTE]
> If you are using Internet Explorer with Service Fabric Explorer to manage a remote cluster, you need to configure some Internet Explorer settings. To ensure that all information loads correctly, go to **Tools** > **Compatibility View Settings** and uncheck **Display intranet sites in Compatibility View**.
>
>

## Understand the Service Fabric Explorer layout
You can navigate through Service Fabric Explorer by using the tree on the left. At the root of the tree, the cluster dashboard provides an overview of your cluster, including a summary of application and node health.

![Service Fabric Explorer cluster dashboard][sfx-cluster-dashboard]

### View the cluster's layout
Nodes in a Service Fabric cluster are placed across a two-dimensional grid of fault domains and upgrade domains. This placement ensures that your applications remain available in the presence of hardware failures and application upgrades. You can view how the current cluster is laid out by using the cluster map.

![Service Fabric Explorer cluster map][sfx-cluster-map]

### View applications and services
The cluster contains two subtrees: one for applications and another for nodes.

You can use the application view to navigate through Service Fabric's logical hierarchy: applications, services, partitions, and replicas.

In the example below, the application **MyApp** consists of two services, **MyStatefulService** and **WebService**. Since **MyStatefulService** is stateful, it includes a partition with one primary and two secondary replicas. By contrast, WebSvcService is stateless and contains a single instance.

![Service Fabric Explorer application view][sfx-application-tree]

At each level of the tree, the main pane shows pertinent information about the item. For example, you can see the health status and version for a particular service.

![Service Fabric Explorer essentials pane][sfx-service-essentials]

### View the cluster's nodes
The node view shows the physical layout of the cluster. For a given node, you can inspect which applications have code deployed on that node. More specifically, you can see which replicas are currently running there.

## Actions
Service Fabric Explorer offers a quick way to invoke actions on nodes, applications, and services within your cluster.

For example, to delete an application instance, choose the application from the tree on the left, and then choose **Actions** > **Delete Application**.

![Deleting an application in Service Fabric Explorer][sfx-delete-application]

> [!TIP]
> You can perform the same actions by clicking the ellipsis next to each element.
>
>

The following table lists the actions available for each entity:

| **Entity** | **Action** | **Description** |
| --- | --- | --- |
| Application type |Unprovision type |Removes the application package from the cluster's image store. Requires all applications of that type to be removed first. |
| Application |Delete Application |Delete the application, including all its services and their state (if any). |
| Service |Delete Service |Delete the service and its state (if any). |
| Node |Activate |Activate the node. |
| Deactivate (pause) |Pause the node in its current state. Services continue to run but Service Fabric does not proactively move anything onto or off it unless it is required to prevent an outage or data inconsistency. This action is typically used to enable debugging services on a specific node to ensure that they do not move during inspection. | |
| Deactivate (restart) |Safely move all in-memory services off a node and close persistent services. Typically used when the host processes or machine need to be restarted. | |
| Deactivate (remove data) |Safely close all services running on the node after building sufficient spare replicas. Typically used when a node (or at least its storage) is being permanently taken out of commission. | |
| Remove node state |Remove knowledge of a node's replicas from the cluster. Typically used when an already failed node is deemed unrecoverable. | |
| Node | Restart | Simulate a node failure by restarting the node. More information [here](https://docs.microsoft.com/en-us/powershell/servicefabric/vlatest/Restart-ServiceFabricNode) ||

Since many actions are destructive, you may be asked to confirm your intent before the action is completed.

> [!TIP]
> Every action that can be performed through Service Fabric Explorer can also be performed through PowerShell or a REST API, to enable automation.
>
>

You can also use Service Fabric Explorer to create application instances for a given application type and version. Choose the application type in the tree view, then click the **Create app instance** link next to the version you'd like in the right pane.

![Creating an application instance in Service Fabric Explorer][sfx-create-app-instance]

> [!NOTE]
> Application instances created through Service Fabric Explorer cannot currently be parameterized. They are created using default parameter values.
>
>

## Connect to a remote Service Fabric cluster
If you know the cluster's endpoint and have sufficient permissions you can access Service Fabric Explorer from any browser. This is because Service Fabric Explorer is just another service that runs in the cluster.

### Discover the Service Fabric Explorer endpoint for a remote cluster
To reach Service Fabric Explorer for a given cluster, point your browser to:

http://&lt;your-cluster-endpoint&gt;:19080/Explorer

For Azure clusters, the full URL is also available in the cluster essentials pane of the Azure portal.

### Connect to a secure cluster
You can control client access to your Service Fabric cluster either with certificates or using Azure Active Directory (AAD).

If you attempt to connect to Service Fabric Explorer on a secure cluster, then depending on the cluster's configuration you'll be required to present a client certificate or log in using AAD.

## Next steps
* [Testability overview](service-fabric-testability-overview.md)
* [Managing your Service Fabric applications in Visual Studio](service-fabric-manage-application-in-visual-studio.md)
* [Service Fabric application deployment using PowerShell](service-fabric-deploy-remove-applications.md)

<!--Image references-->
[sfx-cluster-dashboard]: ./media/service-fabric-visualizing-your-cluster/SfxClusterDashboard.png
[sfx-cluster-map]: ./media/service-fabric-visualizing-your-cluster/SfxClusterMap.png
[sfx-application-tree]: ./media/service-fabric-visualizing-your-cluster/SfxApplicationTree.png
[sfx-service-essentials]: ./media/service-fabric-visualizing-your-cluster/SfxServiceEssentials.png
[sfx-delete-application]: ./media/service-fabric-visualizing-your-cluster/SfxDeleteApplication.png
[sfx-create-app-instance]: ./media/service-fabric-visualizing-your-cluster/SfxCreateAppInstance.png
