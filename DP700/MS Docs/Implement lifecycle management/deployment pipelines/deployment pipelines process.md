URL Source: http://learn.microsoft.com/en-us/fabric/cicd/deployment-pipelines/understand-the-deployment-process?tabs=new-ui

Markdown Content:
The deployment process lets you clone content from one stage in the deployment pipeline to another, typically from development to test, and from test to production.

During deployment, Microsoft Fabric copies the content from the source stage to the target stage. The connections between the copied items are kept during the copy process. Fabric also applies the configured deployment rules to the updated content in the target stage. Deploying content might take a while, depending on the number of items being deployed. During this time, you can navigate to other pages in the portal, but you can't use the content in the target stage.

You can also deploy content programmatically, using the [deployment pipelines REST APIs](https://learn.microsoft.com/en-us/rest/api/power-bi/pipelines). You can learn more about this process in [Automate your deployment pipeline using APIs and DevOps](https://learn.microsoft.com/en-us/fabric/cicd/deployment-pipelines/pipeline-automation).

Note

The new Deployment pipeline user interface is currently in **preview**. To turn on or use the new UI, see [Begin using the new UI](https://learn.microsoft.com/en-us/fabric/cicd/deployment-pipelines/deployment-pipelines-new-ui#begin-using-the-new-ui).

There are two main parts of the deployment pipelines process:

*   [Define the deployment pipeline structure](http://learn.microsoft.com/en-us/fabric/cicd/deployment-pipelines/understand-the-deployment-process?tabs=new-ui#define-the-deployment-pipeline-structure)
*   [Add content to the stages](http://learn.microsoft.com/en-us/fabric/cicd/deployment-pipelines/understand-the-deployment-process?tabs=new-ui#add-content-to-the-stages)

When you create a pipeline, you define how many stages you want and what they should be called. You can also make one or more stages public. The number of stages and their names are permanent, and can't be changed after the pipeline is created. However, you can change the public status of a stage at any time.

To define a pipeline, follow the instructions in [Create a deployment pipeline](https://learn.microsoft.com/en-us/fabric/cicd/deployment-pipelines/get-started-with-deployment-pipelines#step-1---create-a-deployment-pipeline).

You can add content to a pipeline stage in two ways:

*   [Assigning a workspace to an empty stage](http://learn.microsoft.com/en-us/fabric/cicd/deployment-pipelines/understand-the-deployment-process?tabs=new-ui#assign-a-workspace-to-an-empty-stage)
*   [Deploying content from one stage to another](http://learn.microsoft.com/en-us/fabric/cicd/deployment-pipelines/understand-the-deployment-process?tabs=new-ui#deploy-content-from-one-stage-to-another)

When you assign content to an empty stage, a new workspace is created on a capacity for the stage you deploy to. All the metadata in the reports, dashboards, and semantic models of the original workspace is copied to the new workspace in the stage you're deploying to.

After the deployment is complete, refresh the semantic models so that you can use the newly copied content. The semantic model refresh is required because data isn't copied from one stage to another. To understand which item properties are copied during the deployment process, and which item properties aren't copied, review the [item properties copied during deployment](http://learn.microsoft.com/en-us/fabric/cicd/deployment-pipelines/understand-the-deployment-process?tabs=new-ui#item-properties-copied-during-deployment) section.

For instructions on how to assign and unassign workspaces to deployment pipeline stages, see [Assign a workspace to a Microsoft Fabric deployment pipeline](https://learn.microsoft.com/en-us/fabric/cicd/deployment-pipelines/assign-pipeline).

The first time you deploy content, deployment pipelines checks if you have permissions.

If you have permissions, the content of the workspace is copied to the stage you're deploying to, and a new workspace for that stage is created on the capacity.

If you don't have permissions, the workspace is created but the content isn’t copied. You can ask a capacity admin to add your workspace to a capacity, or ask for assignment permissions for the capacity. Later, when the workspace is assigned to a capacity, you can deploy content to this workspace.

If you're using [Premium Per User (PPU)](https://learn.microsoft.com/en-us/power-bi/enterprise/service-premium-per-user-faq), your workspace is automatically associated with your PPU. In such cases, permissions aren't required. However, if you create a workspace with a PPU, only other PPU users can access it. In addition, only PPU users can consume content created in such workspaces.

The deploying user automatically becomes the owner of the cloned semantic models, and the only admin of the new workspace.

There are several ways to deploy content from one stage to another. You can deploy all the content, or you can [select which items to deploy](https://learn.microsoft.com/en-us/fabric/cicd/deployment-pipelines/deploy-content#selective-deployment).

You can deploy content to any adjacent stage, in either direction.

Deploying content from a working production pipeline to a stage that has an existing workspace, includes the following steps:

*   Deploying new content as an addition to the content already there.

*   Deploying updated content to replace some of the content already there.

When content from the source stage is copied to the target stage, Fabric identifies existing content in the target stage and overwrites it. To identify which content item needs to be overwritten, deployment pipelines uses the connection between the parent item and its clones. This connection is kept when new content is created. The overwrite operation only overwrites the content of the item. The item's ID, URL, and permissions remain unchanged.

In the target stage, [item properties that aren't copied](https://learn.microsoft.com/en-us/fabric/cicd/deployment-pipelines/understand-the-deployment-process#item-properties-that-are-not-copied), remain as they were before deployment. New content and new items are copied from the source stage to the target stage.

In Fabric, when items are connected, one of the items depends on the other. For example, a report always depends on the semantic model connected to it. A semantic model can depend on another semantic model, and can also be connected to several reports that depend on it. If there's a connection between two items, deployment pipelines always tries to maintain this connection.

During deployment, deployment pipelines checks for dependencies. The deployment either succeeds or fails, depending on the location of the item that provides the data that the deployed item depends on.

*   **Linked item exists in the target stage** - Deployment pipelines automatically connect (autobind) the deployed item to the item it depends on in the deployed stage. For example, if you deploy a paginated report from development to test, and the report is connected to a semantic model that was previously deployed to the test stage, it automatically connects to the semantic model in the test stage.

*   **Linked item doesn't exist in the target stage** - Deployment pipelines fail a deployment if an item has a dependency on another item, and the item providing the data isn't deployed and doesn't reside in the target stage. For example, if you deploy a report from development to test, and the test stage doesn't contain its semantic model, the deployment fails. To avoid failed deployments due to dependent items not being deployed, use the _Select related_ button. _Select related_ automatically selects all the related items that provide dependencies to the items you're about to deploy.

Autobinding works only with items that are supported by deployment pipelines and reside within Fabric. To view the dependencies of an item, from the item's _More options_ menu, select _View lineage_.

*   [New View lineage UI](http://learn.microsoft.com/en-us/fabric/cicd/deployment-pipelines/understand-the-deployment-process?tabs=new-ui#tabpanel_1_new-ui)
*   [Original View lineage UI](http://learn.microsoft.com/en-us/fabric/cicd/deployment-pipelines/understand-the-deployment-process?tabs=new-ui#tabpanel_1_old-ui)

![Image 1: A screenshot of the new view lineage option, in an item's more options menu.](https://learn.microsoft.com/en-us/fabric/cicd/deployment-pipelines/media/understand-the-deployment-process/view-lineage-new.png)

Deployment pipelines automatically binds items that are connected across pipelines, if they're in the same pipeline stage. When you deploy such items, deployment pipelines attempts to establish a new connection between the deployed item and the item connected to it in the other pipeline. For example, if you have a report in the test stage of pipeline _A_ that's connected to a semantic model in the test stage of pipeline _B_, deployment pipelines recognizes this connection.

Here's an example with illustrations that to help demonstrate how autobinding across pipelines works:

1.   You have a semantic model in the development stage of pipeline A.

2.   You also have a report in the development stage of pipeline B.

3.   Your report in pipeline B is connected to your semantic model in pipeline A. Your report depends on this semantic model.

4.   You deploy the report in pipeline B from the development stage to the test stage.

5.   The deployment succeeds or fails, depending on whether or not you have a copy of the semantic model it depends on in the test stage of pipeline A:

    *   _If you have a copy of the semantic model the report depends on in the test stage of pipeline A_:

The deployment succeeds, and deployment pipelines connects (autobind) the report in the test stage of pipeline B, to the semantic model in the test stage of pipeline A.

![Image 2: A diagram showing a successful deployment of a report from the development stage to the test stage in pipeline B.](https://learn.microsoft.com/en-us/fabric/cicd/deployment-pipelines/media/understand-the-deployment-process/successful-deployment.png)

    *   _If you don't have a copy of the semantic model the report depends on in the test stage of pipeline A_:

The deployment fails because deployment pipelines can't connect (autobind) the report in the test stage in pipeline B, to the semantic model it depends on in the test stage of pipeline A.

![Image 3: A diagram showing a failed deployment of a report from the development stage to the test stage in pipeline B.](https://learn.microsoft.com/en-us/fabric/cicd/deployment-pipelines/media/understand-the-deployment-process/failed-deployment.png)

In some cases, you might not want to use autobinding. For example, if you have one pipeline for developing organizational semantic models, and another for creating reports. In this case, you might want all the reports to always be connected to semantic models in the production stage of the pipeline they belong to. In this case, avoid using the autobinding feature.

![Image 4: A diagram showing two pipelines. Pipeline A has a semantic model in every stage and pipeline B has a report in every stage.](https://learn.microsoft.com/en-us/fabric/cicd/deployment-pipelines/media/understand-the-deployment-process/no-auto-binding.png)

There are three methods you can use to avoid using autobinding:

*   Don't connect the item to corresponding stages. When the items aren't connected in the same stage, deployment pipelines keeps the original connection. For example, if you have a report in the development stage of pipeline B that's connected to a semantic model in the production stage of pipeline A. When you deploy the report to the test stage of pipeline B, it remains connected to the semantic model in the production stage of pipeline A.

*   Define a parameter rule. This option isn't available for reports. You can only use it with semantic models and dataflows.

*   Connect your reports, dashboards, and tiles to a proxy semantic model or dataflow that isn't connected to a pipeline.

Parameters can be used to control the connections between semantic models or dataflows and the items that they depend on. When a parameter controls the connection, autobinding after deployment doesn't take place, even when the connection includes a parameter that applies to the semantic model’s or dataflow's ID, or the workspace ID. In those cases, rebind the items after the deployment by changing the parameter value, or by using [parameter rules](https://learn.microsoft.com/en-us/fabric/cicd/deployment-pipelines/create-rules).

Note

If you're using parameter rules to rebind items, the parameters must be of type `Text`.

Data in the target item, such as a semantic model or dataflow, is kept when possible. If there are no changes to an item that holds the data, the data is kept as it was before the deployment.

In many cases, when you have a small change such as adding or removing a table, Fabric keeps the original data. For breaking schema changes, or changes in the data source connection, a full refresh is required.

Any [licensed user](https://learn.microsoft.com/en-us/fabric/enterprise/licenses#per-user-licenses) who's a contributor of both the target and source deployment workspaces, can deploy content that resides on a [capacity](https://learn.microsoft.com/en-us/fabric/enterprise/licenses#capacity) to a stage with an existing workspace. For more information, review the [permissions](http://learn.microsoft.com/en-us/fabric/cicd/deployment-pipelines/understand-the-deployment-process?tabs=new-ui#permissions) section.

Folders enable users to efficiently organize and manage workspace items in a familiar way. When you deploy content that contains folders to a different stage, the folder hierarchy of the applied items is automatically applied.

*   [New folders representation UI](http://learn.microsoft.com/en-us/fabric/cicd/deployment-pipelines/understand-the-deployment-process?tabs=new-ui#tabpanel_2_new-ui)
*   [Original folders representation UI](http://learn.microsoft.com/en-us/fabric/cicd/deployment-pipelines/understand-the-deployment-process?tabs=new-ui#tabpanel_2_old-ui)

The workspace content is shown as it's structured in the workspace. Folders are listed, and in order to see their items you need to select the folder. An item’s full path is shown at the top of the items list. Since a deployment is of items only, you can only select a folder that contains supported items. Selecting a folder for deployment means selecting all its items and subfolders with their items for a deployment.

This picture shows the contents of a folder inside the workspace. The full pathname of the folder is shown at the top of the list.

![Image 5: Screenshot showing the contents of a folder with full pathname of the folder. The name includes the name of the folder.](https://learn.microsoft.com/en-us/fabric/cicd/deployment-pipelines/media/understand-the-deployment-process/folder-path-new.png)

In Deployment pipelines, folders are considered part of an item’s name (an item name includes its full path). When an item is deployed, after its path was changed (moved from folder A to folder B, for example), then Deployment pipelines applies this change to its paired item during deployment - the paired item will be moved as well to folder B. If folder B doesn't exist in the stage we're deploying to, it's created in its workspace first. Folders can be seen and managed only on the workspace page.

Deploy items inside a folder from that folder. You can't deploy items from different hierarchies at the same time.

Since folders are considered part of the item’s name, items moved into a different folder in the workspace, are identified on Deployment pipelines page as _Different_ when compared. This item doesn't appear in the compare window since it isn't a schema change but settings change.

*   [Moved folder item in new UI](http://learn.microsoft.com/en-us/fabric/cicd/deployment-pipelines/understand-the-deployment-process?tabs=new-ui#tabpanel_3_new-ui)
*   [Moved folder item in original UI](http://learn.microsoft.com/en-us/fabric/cicd/deployment-pipelines/understand-the-deployment-process?tabs=new-ui#tabpanel_3_old-ui)

![Image 6: Screenshot showing the compare changes screen of with an item in one stage that was moved to a different folder in the new UI.](https://learn.microsoft.com/en-us/fabric/cicd/deployment-pipelines/media/understand-the-deployment-process/moved-folder-item-new.png)

*   Individual folders can't be deployed manually in deployment pipelines. Their deployment is triggered automatically when at least one of their items is deployed.

*   The folder hierarchy of paired items is updated only during deployment. During assignment, after the pairing process, the hierarchy of paired items isn't updated yet.

*   Since a folder is deployed only if one of its items is deployed, an empty folder can't be deployed.

*   Deploying one item out of several in a folder also updates the structure of the items that aren't deployed in the target stage even though the items themselves aren't deployed.

Parent child relationships only appear in the new UI. They looks the same as in the workspace. The child isn't deployed but recreated on the target stage

![Image 7: Screenshot showing the depiction of a parent child relationship in the new UI.](https://learn.microsoft.com/en-us/fabric/cicd/deployment-pipelines/media/understand-the-deployment-process/parent-child.png)

For a list of supported items, see [Deployment pipelines supported items](https://learn.microsoft.com/en-us/fabric/cicd/deployment-pipelines/intro-to-deployment-pipelines#supported-items).

During deployment, the following item properties are copied and overwrite the item properties at the target stage:

*   Data sources ([deployment rules](https://learn.microsoft.com/en-us/fabric/cicd/deployment-pipelines/create-rules) are supported)

*   Parameters​ ([deployment rules](https://learn.microsoft.com/en-us/fabric/cicd/deployment-pipelines/create-rules) are supported)

*   Report visuals​

*   Report pages​

*   Dashboard tiles​

*   Model metadata​

*   Item relationships

*   [Sensitivity labels](https://learn.microsoft.com/en-us/power-bi/enterprise/service-security-sensitivity-label-overview) are copied _only_ when one of the following conditions is met. If these conditions aren't met, sensitivity labels _aren't_ copied during deployment.

    *   A new item is deployed, or an existing item is deployed to an empty stage.

Note

In cases where default labeling is enabled on the tenant, and the default label is valid, if the item being deployed is a semantic model or dataflow, the label is copied from the source item **only** if the label has protection. If the label isn't protected, the default label is applied to the newly created target semantic model or dataflow. 
    *   The source item has a label with protection and the target item doesn't. In this case, a pop-up window asks for consent to override the target sensitivity label.

The following item properties aren't copied during deployment:

*   Data - Data isn't copied. Only metadata is copied

*   URL

*   ID

*   Permissions - For a workspace or a specific item

*   Workspace settings - Each stage has its own workspace

*   App content and settings - To update your apps, see [Update content to Power BI apps](http://learn.microsoft.com/en-us/fabric/cicd/deployment-pipelines/understand-the-deployment-process?tabs=new-ui#update-content-to-power-bi-apps)

*   [Personal bookmarks](https://learn.microsoft.com/en-us/power-bi/consumer/end-user-bookmarks#create-personal-bookmarks-in-the-power-bi-service)

The following semantic model properties are also not copied during deployment:

*   Role assignment

*   Refresh schedule

*   Data source credentials

*   Query caching settings (can be inherited from the capacity)

*   Endorsement settings

Deployment pipelines supports many semantic model features. This section lists two semantic model features that can enhance your deployment pipelines experience:

*   [Incremental refresh](http://learn.microsoft.com/en-us/fabric/cicd/deployment-pipelines/understand-the-deployment-process?tabs=new-ui#incremental-refresh)

*   [Composite models](http://learn.microsoft.com/en-us/fabric/cicd/deployment-pipelines/understand-the-deployment-process?tabs=new-ui#composite-models)

Deployment pipelines supports [incremental refresh](https://learn.microsoft.com/en-us/power-bi/connect-data/incremental-refresh-overview), a feature that allows large semantic models faster and more reliable refreshes, with lower consumption.

With deployment pipelines, you can make updates to a semantic model with incremental refresh while retaining both data and partitions. When you deploy the semantic model, the policy is copied along.

To understand how incremental refresh behaves with dataflows, see [why do I see two data sources connected to my dataflow after using dataflow rules?](https://learn.microsoft.com/en-us/power-bi/create-reports/deployment-pipelines-troubleshooting#why-do-i-see-two-data-sources-connected-to-my-dataflow-after-using-dataflow-rules-)

Note

Incremental refresh settings aren't copied in Gen 1.

To enable incremental refresh, [configure it in Power BI Desktop](https://learn.microsoft.com/en-us/power-bi/connect-data/incremental-refresh-overview), and then publish your semantic model. After you publish, the incremental refresh policy is similar across the pipeline, and can be authored only in Power BI Desktop.

Once your pipeline is configured with incremental refresh, we recommend that you use the following flow:

1.   Make changes to your _.pbix_ file in Power BI Desktop. To avoid long waiting times, you can make changes using a sample of your data.

2.   Upload your _.pbix_ file to the first (usually _development_) stage.

3.   Deploy your content to the next stage. After deployment, the changes you made will apply to the entire semantic model you're using.

4.   Review the changes you made in each stage, and after you verify them, deploy to the next stage until you get to the final stage.

The following are a few examples of how you can integrate incremental refresh with deployment pipelines.

*   [Create a new pipeline](https://learn.microsoft.com/en-us/fabric/cicd/deployment-pipelines/get-started-with-deployment-pipelines#step-1---create-a-deployment-pipeline) and connect it to a workspace with a semantic model that has incremental refresh enabled.

*   Enable incremental refresh in a semantic model that's already in a _development_ workspace.

*   Create a pipeline from a production workspace that has a semantic model that uses incremental refresh. For example, assign the workspace to a new pipeline's _production_ stage, and use backwards deployment to deploy to the _test_ stage, and then to the _development_ stage.

*   Publish a semantic model that uses incremental refresh to a workspace that's part of an existing pipeline.

For incremental refresh, deployment pipelines only supports semantic models that use [enhanced semantic model metadata](https://learn.microsoft.com/en-us/power-bi/connect-data/desktop-enhanced-dataset-metadata). All semantic models created or modified with Power BI Desktop automatically implement enhanced semantic model metadata.

When republishing a semantic model to an active pipeline with incremental refresh enabled, the following changes result in deployment failure due to data loss potential:

*   Republishing a semantic model that doesn't use incremental refresh, to replace a semantic model that has incremental refresh enabled.

*   Renaming a table that has incremental refresh enabled.

*   Renaming noncalculated columns in a table with incremental refresh enabled.

Other changes such as adding a column, removing a column, and renaming a calculated column, are permitted. However, if the changes affect the display, you need to refresh before the change is visible.

Using [composite models](https://learn.microsoft.com/en-us/power-bi/transform-model/desktop-composite-models) you can set up a report with multiple data connections.

You can use the composite models functionality to connect a Fabric semantic model to an external semantic model such as Azure Analysis Services. For more information, see [Using DirectQuery for Fabric semantic models and Azure Analysis Services](https://learn.microsoft.com/en-us/power-bi/connect-data/desktop-directquery-datasets-azure-analysis-services).

In a deployment pipeline, you can use composite models to connect a semantic model to another Fabric semantic model external to the pipeline.

[Automatic aggregations](https://learn.microsoft.com/en-us/power-bi/enterprise/aggregations-auto) are built on top of user defined aggregations and use machine learning to continuously optimize DirectQuery semantic models for maximum report query performance.

Each semantic model keeps its automatic aggregations after deployment. Deployment pipelines doesn't change a semantic model's automatic aggregation. This means that if you deploy a semantic model with an automatic aggregation, the automatic aggregation in the target stage remains as is, and isn't overwritten by the automatic aggregation deployed from the source stage.

To enable automatic aggregations, follow the instructions in [configure the automatic aggregation](https://learn.microsoft.com/en-us/power-bi/enterprise/aggregations-auto-configure).

Hybrid tables are tables with [incremental refresh](https://learn.microsoft.com/en-us/power-bi/connect-data/incremental-refresh-overview) that can have both import and direct query partitions. During a clean deployment, both the refresh policy and the hybrid table partitions are copied. When you're deploying to a pipeline stage that already has hybrid table partitions, only the refresh policy is copied. To update the partitions, refresh the table.

[Power BI apps](https://learn.microsoft.com/en-us/power-bi/consumer/end-user-apps) are the recommended way of distributing content to free Fabric consumers. You can update the content of your Power BI apps using a deployment pipeline, giving you more control and flexibility when it comes to your app's lifecycle.

Create an app for each deployment pipeline stage, so that you can test each update from an end user's point of view. Use the **publish** or **view** button in the workspace card to publish or view the app in a specific pipeline stage.

*   [Publish app - new UI](http://learn.microsoft.com/en-us/fabric/cicd/deployment-pipelines/understand-the-deployment-process?tabs=new-ui#tabpanel_4_new-ui)
*   [Publish app - original UI](http://learn.microsoft.com/en-us/fabric/cicd/deployment-pipelines/understand-the-deployment-process?tabs=new-ui#tabpanel_4_old-ui)

![Image 8: A screenshot showing the publish app button, in the stage options.](https://learn.microsoft.com/en-us/fabric/cicd/deployment-pipelines/media/understand-the-deployment-process/publish-new.png)

In the production stage, you can also update the app page in Fabric, so that any content updates become available to app users.

*   [Update app - new UI](http://learn.microsoft.com/en-us/fabric/cicd/deployment-pipelines/understand-the-deployment-process?tabs=new-ui#tabpanel_5_new-ui)
*   [Update app - original UI](http://learn.microsoft.com/en-us/fabric/cicd/deployment-pipelines/understand-the-deployment-process?tabs=new-ui#tabpanel_5_old-ui)

![Image 9: A screenshot highlighting the update app button in the new UI.](https://learn.microsoft.com/en-us/fabric/cicd/deployment-pipelines/media/understand-the-deployment-process/update-new.png)

Important

The deployment process doesn't include updating the app content or settings. To apply changes to content or settings, you need to manually update the app in the required pipeline stage.

Permissions are required for the pipeline, and for the workspaces that are assigned to it. Pipeline permissions and workspace permissions are granted and managed separately.

*   Pipelines only have one permission, _Admin_, which is required for sharing, editing and deleting a pipeline.

*   Workspaces have different permissions, also called [roles](https://learn.microsoft.com/en-us/fabric/fundamentals/roles-workspaces). Workspace roles determine the level of access to a workspace in a pipeline.

*   Deployment pipelines doesn't support [Microsoft 365 groups](https://learn.microsoft.com/en-us/microsoft-365/admin/create-groups/compare-groups#microsoft-365-groups) as pipeline admins.

To deploy from one stage to another in the pipeline, you must be a pipeline admin, and either a contributor, member, or admin of the workspaces assigned to the stages involved. For example, a pipeline admin that isn't assigned a workspace role, can view the pipeline and share it with others. However, this user can't view the content of the workspace in the pipeline, or in the service, and can't perform deployments.

This section describes the deployment pipeline permissions. The permissions listed in this section might have different applications in other Fabric features.

The lowest deployment pipeline permission is _pipeline admin_, and it's required for all deployment pipeline operations.

| User | Pipeline permissions | Comments |
| --- | --- | --- |
| **Pipeline admin** | * View the pipeline​ * Share the pipeline with others * Edit and delete the pipeline * Unassign a workspace from a stage * Can see workspaces that are tagged as assigned to the pipeline in Power BI service | Pipeline access doesn't grant permissions to view or take actions on the workspace content. |
| **Workspace viewer** (and pipeline admin) | * Consume content * Unassign a workspace from a stage | Workspace members assigned the Viewer role without _build_ permissions, can't access the semantic model or edit workspace content. |
| **Workspace contributor** (and pipeline admin) | * Consume content​ * Compare stages * View semantic models * Unassign a workspace from a stage * Deploy items (except in GCC) (must be at least a contributor in both source and target workspaces) |  |
| **Workspace member** (and pipeline admin) | * View workspace content​ * Compare stages * Deploy items (must be at least a contributor in both source and target workspaces) * Update semantic models * Unassign a workspace from a stage * Configure semantic model rules (you must be the semantic model owner) | If the _block republish and disable package refresh_ setting located in the tenant _semantic model security_ section is enabled, only semantic model owners are able to update semantic models. |
| **Workspace admin** (and pipeline admin) | * View workspace content​ * Compare stages * Deploy items * Assign workspaces to a stage * Update semantic models * Unassign a workspace from a stage * Configure semantic model rules (you must be the semantic model owner) |  |

When you're deploying Power BI items, the ownership of the deployed item might change. Review the following table to understand who can deploy each item and how the deployment affects the item's ownership.

| Fabric Item | Required permission to deploy an existing item | Item ownership after a first time deployment | Item ownership after deployment to a stage with the item |
| --- | --- | --- | --- |
| Semantic model | Workspace member | The user who made the deployment becomes the owner | Unchanged |
| Dataflow | Dataflow owner | The user who made the deployment becomes the owner | Unchanged |
| Datamart | Datamart owner | The user who made the deployment becomes the owner | Unchanged |
| Paginated report | Workspace member | The user who made the deployment becomes the owner | The user who made the deployment becomes the owner |

The following table lists required permissions for popular deployment pipeline actions. Unless specified otherwise, for each action you need all the listed permissions.

| Action | Required permissions |
| --- | --- |
| View the list of pipelines in your organization | No license required (free user) |
| Create a pipeline | A user with one of the following licenses: * Pro * PPU * Premium |
| Delete a pipeline | Pipeline admin |
| Add or remove a pipeline user | Pipeline admin |
| Assign a workspace to a stage | * Pipeline admin * Workspace admin (of the workspace to be assigned) |
| Unassign a workspace to a stage | One of the following roles: * Pipeline admin * Workspace admin (using the [Pipelines - Unassign Workspace](https://learn.microsoft.com/en-us/rest/api/power-bi/pipelines/unassign-workspace) API) |
| Deploy to an empty stage (see note) | * Pipeline admin * Source workspace contributor |
| Deploy items to the next stage (see note) | * Pipeline admin * Workspace contributor to both the source and target stages * To deploy datamarts or dataflows, you must be the owner of the deployed item * If the semantic model tenant admin switch is turned on and you're deploying a semantic model, you need to be the owner of the semantic model |
| View or set a rule | * Pipeline admin * Target workspace contributor, member, or admin * Owner of the item you're setting a rule for |
| Manage pipeline settings | Pipeline admin |
| View a pipeline stage | * Pipeline admin * Workspace reader, contributor, member, or admin. You see the items that your workspace permissions grant access to. |
| View the list of items in a stage | Pipeline admin |
| Compare two stages | * Pipeline admin * Workspace contributor, member, or admin for both stages |
| View deployment history | Pipeline admin |

Note

To deploy content in the GCC environment, you need to be at least a member of both the source and target workspace. Deploying as a contributor isn't supported yet.

This section lists most of the limitations in deployment pipelines.

*   The workspace must reside on a [Fabric capacity](https://learn.microsoft.com/en-us/fabric/enterprise/licenses#capacity).
*   The maximum number of items that can be deployed in a single deployment is 300.
*   Downloading a _.pbix_ file after deployment isn't supported.
*   [Microsoft 365 groups](https://learn.microsoft.com/en-us/microsoft-365/admin/create-groups/compare-groups#microsoft-365-groups) aren't supported as pipeline admins.
*   When you deploy a Power BI item for the first time, if another item in the target stage has the same name and type (for example, if both files are reports), the deployment fails.
*   For a list of workspace limitations, see the [workspace assignment limitations](https://learn.microsoft.com/en-us/fabric/cicd/deployment-pipelines/assign-pipeline#considerations-and-limitations).
*   For a list of supported items, see [supported items](https://learn.microsoft.com/en-us/fabric/cicd/deployment-pipelines/intro-to-deployment-pipelines#supported-items). Any item not on the list isn't supported.
*   The deployment fails if any of the items have circular or self dependencies (for example, item A references item B and item B references item A).
*   PBIR reports aren't supported.

*   Datasets that use real-time data connectivity can't be deployed.

*   A semantic model with DirectQuery or Composite connectivity mode that uses variation or [auto date/time](https://learn.microsoft.com/en-us/power-bi/transform-model/desktop-auto-date-time) tables, isn’t supported. For more information, see [What can I do if I have a dataset with DirectQuery or Composite connectivity mode, that uses variation or calendar tables?](https://learn.microsoft.com/en-us/fabric/cicd/faq#deployment-pipelines-questions).

*   During deployment, if the target semantic model is using a [live connection](https://learn.microsoft.com/en-us/power-bi/connect-data/desktop-report-lifecycle-datasets), the source semantic model must use this connection mode too.

*   After deployment, downloading a semantic model (from the stage it was deployed to) isn't supported.

*   For a list of deployment rule limitations, see [deployment rules limitations](https://learn.microsoft.com/en-us/fabric/cicd/deployment-pipelines/create-rules#considerations-and-limitations).

*   If autobinding is engaged, then:

    *   Native query and DirectQuery together isn't supported. This includes proxy datasets.
    *   The datasource connection must be the first step in the mashup expression.

*   When a Direct Lake semantic model is deployed, it doesn't automatically bind to items in the target stage. For example, if a LakeHouse is a source for a DirectLake semantic model and they're both deployed to the next stage, the DirectLake semantic model in the target stage will still be bound to the LakeHouse in the source stage. Use datasource rules to bind it to an item in the target stage. Other types of semantic models are automatically bound to the paired item in the target stage.

*   Incremental refresh settings aren't copied in Gen 1.

*   When you're deploying a dataflow to an empty stage, deployment pipelines creates a new workspace and sets the dataflow storage to a Fabric blob storage. Blob storage is used even if the source workspace is configured to use Azure data lake storage Gen2 (ADLS Gen2).

*   Service principal isn't supported for dataflows.

*   Deploying common data model (CDM) isn't supported.

*   For deployment pipeline rule limitations that affect dataflows, see [Deployment rules limitations](https://learn.microsoft.com/en-us/fabric/cicd/deployment-pipelines/create-rules#considerations-and-limitations).

*   If a dataflow is being refreshed during deployment, the deployment fails.

*   If you compare stages during a dataflow refresh, the results are unpredictable.

*   Autobinding isn't supported for dataflows Gen2.

*   You can't deploy a datamart with sensitivity labels.

*   You need to be the datamart owner to deploy a datamart.

[Get started with deployment pipelines](https://learn.microsoft.com/en-us/fabric/cicd/deployment-pipelines/get-started-with-deployment-pipelines).