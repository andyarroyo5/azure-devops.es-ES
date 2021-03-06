---
title: Environment
description: Collection of deployment targets useful for traceability and recoding deployment history
ms.topic: reference
ms.prod: devops
ms.technology: devops-cicd
ms.assetid: 4abec444-5d74-4959-832d-20fd0acee81d
ms.manager: mijacobs
ms.author: jukullam
author: juliakm
ms.date: 05/03/2019
monikerRange: azure-devops
---

# Create and target an environment

[!INCLUDE [include](../includes/version-team-services.md)]

Environment represents a collection of resources such as namespaces within Kubernetes clusters, Azure Web Apps, virtual machines, databases, which can be targeted by deployments from a pipeline. Typical examples of environments include *Dev, Test, QA, Staging and Production.*

The advantages of using environments include the following.

- **Deployment history** - Pipeline name and run details are recorded for deployments to an environment and its resources. In the context of multiple pipelines targeting the same environment or resource, [deployment history](#deployment-history) of an environment is useful to identify the source of changes.
- **Traceability of commits and work items** - View jobs within the pipeline run that targeted an environment and the corresponding [commits and work items](#deployment-history) that were newly deployed to the environment. This allows one to track whether a code change (commit) or feature/bug-fix (work items) reached an environment.
- **Diagnose resource health** - The resource health related information shown in resource views allows one to validate whether the application is functioning at its desired state or whether it has regressed post deployments.
- **Permissions** - User permissions and pipeline permissions can be used to secure environments by specifying which users and pipelines are allowed to target an environment.

## Resources

While environment at its core is a grouping of resources, the resources themselves represent actual deployment targets. The [Kubernetes resource](environments-kubernetes.md) and [virtual machine resource](environments-virtual-machines.md) types are currently supported.

<a name="creation"></a>

## Create an environment

1. Sign in to your Azure DevOps organization and navigate to your project.

2. In your project, navigate to the Pipelines page. Then choose Environments and click on **Create Environment**.

   > [!div class="mx-imgBorder"]
   > ![Environments](media/environments-nav.png)

3. After keying in the name of an environment (required) and the description (optional), one can choose to either create an environment with no resources or create an environment with a Kubernetes resource. Note that resources can be added to an existing environment later as well.

> [!TIP]
> It is possible to create an empty environment and reference the same from deployment jobs to record the deployment history against the environment.

> [!NOTE]
> You can use a Pipeline to create, and deploy to environments as well. To learn more, see the [how to guide](../ecosystems/kubernetes/aks-template.md)

<a name="target-from-deployment-job"></a>

## Target an environment from a deployment job

A [deployment job](deployment-jobs.md) is a collection of steps to be run sequentially. A deployment job can be used to target an entire environment (group of resources) as shown in the following YAML snippet.

```YAML
- stage: deploy
  jobs:
  - deployment: DeployWeb
    displayName: deploy Web App
    pool:
      vmImage: 'Ubuntu-latest'
    # creates an environment if it doesn't exist
    environment: 'smarthotel-dev'
    strategy:
      runOnce:
        deploy:
          steps:
          - script: echo Hello world
```

> [!NOTE]
> If the specified environment doesn't already exist, an empty environment is created using the environment name provided.

<a name="target-resource-from-deployment-job"></a>

## Target a specific resource within an environment from deployment job

It is possible to scope down the target of deployment to a particular resource within the environment as shown below. This allows for recording deployment history on a specific resource within the environment as opposed to recoding the history on all resources in the environment. Also, the steps of the deployment job **automatically inherit** the service connection details from resource targeted by the deployment job as shown in the following example. 

```YAML
environment: 'smarthotel-dev.bookings'
strategy: 
 runOnce:
   deploy:
     steps:
     - task: KubernetesManifest@0
       displayName: Deploy to Kubernetes cluster
       inputs:
         action: deploy
         namespace: $(k8sNamespace)
         manifests: $(System.ArtifactsDirectory)/manifests/*
         imagePullSecrets: $(imagePullSecret)
         containers: $(containerRegistry)/$(imageRepository):$(tag)
         # value for kubernetesServiceConnection input automatically passed down to task by environment.resource input
```

<a name="in-run-details"></a>

## Environment in run details

All  environments targeted by deployment jobs of a specific run of a pipeline can be found under the *Environments* tab of pipeline run details.

  > [!div class="mx-imgBorder"]
  > ![Environments in run details](media/environments-run.png)

## Approvals

You can manually control when a stage should run using approval checks. This is commonly used to control deployments to production environments. Checks are a mechanism available to the *resource owner* to control if and when a stage in a pipeline can consume a resource. As an owner of a resource, such as an environment, you can define checks that must be satisfied before a stage consuming that resource can start. 

Currently, manual approval checks are supported on environments. 
For more information, see [Approvals](approvals.md).

<a name="deployment-history"></a>

## Deployment history within environments

The deployment history view within environments provides the following advantages.

1. View jobs from all pipelines that are targeting a specific environment. Consider the scenario where two microservices, each having its own pipeline, are deploying to the same environment. In that case, the deployment history listing helps identify all pipelines that are impacting this environment and also helps visualize the sequence of deployments by each of these pipelines.

   > [!div class="mx-imgBorder"]
   > ![Deployment history](media/environments-deployment-history.png)


2. Drilldown into the job details reveals the listing of commits and work items that were newly deployed to the environment.

   > [!div class="mx-imgBorder"]
   > ![Commits under deployment history](media/environments-deployment-history-commits.png)

## Security

### User permissions
You can control who can create, view, use and manage the environments with user permissions. You have four roles i.e. Creator (scope: all environments), Reader, User and Administrator roles to manage each of these actions. In the specific environment's **user permissions** panel, you can set the permissions which are inherited and you can override the roles for each environment. 

-  Navigate to the specific **environment** that you would like to authorize. 
-  Click on overflow menu button located at the top right part of the page next to "Add resource" and choose **Security** to view the settings.
-  In the **User permissions** blade, click on **+Add** to add a **User or group** and select a suitable **Role**. 

| Role on an environment | Purpose |
|------------------------------------|---------|
| Creator | Global role, available from enviroments hub security option. Members of this role can create the environment in the project. Contributors are added as members by default. Not applicable for environments auto created from YAML pipeline.|
| Reader | Members of this role can view the environment. |
| User | Members of this role can use the environment when authoring yaml pipelines. |
| Administrator | In addition to using the environment, members of this role can manage membership of all other roles for the environment in the project. Creators are added as members by default. |

> [!NOTE]
> - In case of auto created environments from YAML, contributors and project administrators will be granted **Administrator** role. Typically used in provisioning Dev/Test environments.
> - In case the environment is created from UI, only the creator will be granted the **Administrator** role. Hence it is reccommended to create protected environments such as production from UI.

### Pipeline permissions

Pipeline permissions can be used to authorize either all or specific pipelines for deploying to the environment of concern.

- To remove **Open access** on the environment/resource to all the pipelines in the project, click on **Restrict permission** button in the **Pipeline permissions** blade.
- To allow specific pipelines to deploy to the environment or a specific resource, click on **+** button and choose from the list of pipelines.
