Title: Overview of Fabric Git integration - Microsoft Fabric

URL Source: https://learn.microsoft.com/en-us/fabric/cicd/git-integration/intro-to-git-integration?tabs=azure-devops

Markdown Content:
Save

This article explains to developers how to integrate Git version control with the Microsoft Fabric Application lifecycle management (ALM) tool.

Note

Some of the items for Git integration are in preview. For more information, see the list of [supported items](https://learn.microsoft.com/en-us/fabric/cicd/git-integration/intro-to-git-integration?tabs=azure-devops#supported-items).

Git integration in Microsoft Fabric enables developers to integrate their development processes, tools, and best practices straight into the Fabric platform. It allows developers who are developing in Fabric to:

*   Backup and version their work
*   Revert to previous stages as needed
*   Collaborate with others or work alone using Git branches
*   Apply the capabilities of familiar source control tools to manage Fabric items

The integration with source control is on a workspace level. Developers can version items they develop within a workspace in a single process, with full visibility to all their items. The workspace structure, including [subfolders](https://learn.microsoft.com/en-us/fabric/cicd/git-integration/git-integration-process#folders), is preserved in the Git repository.

See the list of [supported items](https://learn.microsoft.com/en-us/fabric/cicd/git-integration/intro-to-git-integration?tabs=azure-devops#supported-items).

*   Read up on basic [Git](https://learn.microsoft.com/en-us/devops/develop/git/what-is-git) and [version control](https://learn.microsoft.com/en-us/devops/develop/git/what-is-version-control) concepts.

*   Read more about the [Git integration process](https://learn.microsoft.com/en-us/fabric/cicd/git-integration/git-integration-process).

*   Read about the best way to manage your [Git branches](https://learn.microsoft.com/en-us/fabric/cicd/git-integration/manage-branches).

Before you enable Git integration, make sure you review the following privacy statements:

*   [Microsoft privacy statement](https://go.microsoft.com/fwlink/?LinkId=521839)
*   [Azure DevOps Services Data protection overview](https://learn.microsoft.com/en-us/azure/devops/organizations/security/data-protection)
*   [GitHub Data protection agreement](https://github.com/customer-terms/github-data-protection-agreement)

The following Git providers are supported:

*   [Git in Azure Repos](https://learn.microsoft.com/en-us/azure/devops/user-guide/code-with-git) with the _same tenant_ as the Fabric tenant
*   [GitHub](https://github.com/) (cloud versions only)
*   [GitHub Enterprise](https://github.com/enterprise) (cloud versions only)

The following items currently support Git integration:

*   Data Engineering items:

    *   [Environment](https://learn.microsoft.com/en-us/fabric/data-engineering/environment-git-and-deployment-pipeline#integrate-git-for-fabric-environments)
    *   [GraphQL](https://learn.microsoft.com/en-us/fabric/data-engineering/graphql-source-control-and-deployment#api-for-graphql-git-integration)_(preview)_
    *   [Lakehouse](https://learn.microsoft.com/en-us/fabric/data-engineering/lakehouse-git-deployment-pipelines#lakehouse-git-integration)_(preview)_
    *   [Notebooks](https://learn.microsoft.com/en-us/fabric/data-engineering/notebook-source-control-deployment#notebook-git-integration)
    *   [Spark Job Definitions](https://learn.microsoft.com/en-us/fabric/data-engineering/spark-job-definition-source-control)_(preview)_
    *   User Data Functions _(preview)_

*   Data Factory items:

    *   [Copy Job](https://learn.microsoft.com/en-us/fabric/data-factory/cicd-copy-job#get-started-with-git-integration-for-copy-job)_(preview)_
    *   [Dataflow gen2](https://learn.microsoft.com/en-us/fabric/data-factory/dataflow-gen2-cicd-and-git-integration)
    *   [Data pipeline](https://learn.microsoft.com/en-us/fabric/data-factory/git-integration-deployment-pipelines)
    *   [Mirrored database](https://learn.microsoft.com/en-us/fabric/database/mirrored-database/mirrored-database-cicd#mirrored-database-git-integration)
    *   Mount ADF _(preview)_
    *   [Variable library](https://learn.microsoft.com/en-us/fabric/cicd/variable-library/variable-library-cicd#variable-libraries-and-git-integration)_(preview)_

*   Real-time Intelligence items:

    *   [Activator](https://learn.microsoft.com/en-us/fabric/real-time-intelligence/git-deployment-pipelines)_(preview)_
    *   [Eventhouse](https://learn.microsoft.com/en-us/fabric/real-time-intelligence/git-deployment-pipelines#eventhouse-files)
    *   [EventStream](https://learn.microsoft.com/en-us/fabric/real-time-intelligence/event-streams/eventstream-cicd)
    *   [KQL database](https://learn.microsoft.com/en-us/fabric/real-time-intelligence/git-deployment-pipelines#kql-database-files)
    *   [KQL Queryset](https://learn.microsoft.com/en-us/fabric/real-time-intelligence/git-deployment-pipelines#kql-queryset-files)
    *   [Real-time Dashboard](https://learn.microsoft.com/en-us/fabric/real-time-intelligence/git-deployment-pipelines#real-time-dashboard-files)

*   Data Warehouse items:

    *   [Warehouse](https://learn.microsoft.com/en-us/fabric/data-warehouse/source-control#git-integration)_(preview)_

*   Power BI items:

    *   Metrics Set _(preview)_
    *   [Org app](https://learn.microsoft.com/en-us/power-bi/consumer/org-app-items/org-app-cicd)_(preview)_
    *   [Paginated report](https://learn.microsoft.com/en-us/power-bi/paginated-reports/paginated-github-integration)_(preview)_
    *   [Report](https://learn.microsoft.com/en-us/fabric/cicd/git-integration/source-code-format#report-files) (except reports connected to semantic models hosted in [Azure Analysis Services](https://learn.microsoft.com/en-us/azure/analysis-services/analysis-services-overview), [SQL Server Analysis Services](https://learn.microsoft.com/en-us/analysis-services/analysis-services-overview), or reports exported by Power BI Desktop that depend on semantic models hosted in [MyWorkspace](https://learn.microsoft.com/en-us/fabric/admin/portal-workspaces#govern-my-workspaces)) _(preview)_
    *   [Semantic model](https://learn.microsoft.com/en-us/fabric/cicd/git-integration/source-code-format#semantic-model-files) (except push datasets, live connections to Analysis Services, model v1) _(preview)_

*   Database items:

    *   [SQL database](https://learn.microsoft.com/en-us/fabric/database/sql/source-control)_(preview)_

*   Industry solutions:

    *   [Healthcare](https://learn.microsoft.com/en-us/industry/healthcare/healthcare-data-solutions/application-lifecycle-management)_(preview)_
    *   HealthCare Cohort _(preview)_

If the workspace or Git directory has unsupported items, it can still be connected, but the unsupported items are ignored. They aren't saved or synced, but they're not deleted either. They appear in the source control panel but you can't commit or update them.

*   The [authentication method](https://learn.microsoft.com/en-us/entra/identity/authentication/concept-authentication-methods-manage#authentication-methods-policy) in Fabric must be at least as strong as the authentication method for Git. For example, if Git requires multifactor authentication, Fabric needs to require multifactor authentication as well.
*   Power BI Datasets connected to Analysis Services aren't supported at this time.
*   Submodules aren't supported.
*   Sovereign clouds aren't supported.

*   [Azure DevOps limitations](https://learn.microsoft.com/en-us/fabric/cicd/git-integration/intro-to-git-integration?tabs=azure-devops#tabpanel_1_azure-devops)
*   [GitHub limitations](https://learn.microsoft.com/en-us/fabric/cicd/git-integration/intro-to-git-integration?tabs=azure-devops#tabpanel_1_github)

*   The Azure DevOps account must be registered to the same user that is using the Fabric workspace.
*   Azure DevOps isn't supported if [Enable IP Conditional Access policy validation](https://learn.microsoft.com/en-us/azure/devops/organizations/accounts/change-application-access-policies#cap-support-on-azure-devops) is enabled.
*   The tenant admin must enable [cross-geo exports](https://learn.microsoft.com/en-us/fabric/admin/git-integration-admin-settings#users-can-export-items-to-git-repositories-in-other-geographical-locations-preview) if the workspace and Git repo are in two different geographical regions.
*   If your organization configured [conditional access](https://learn.microsoft.com/en-us/appcenter/general/configuring-aad-conditional-access), make sure the **Power BI Service** has the same [conditions set](https://learn.microsoft.com/en-us/fabric/security/security-conditional-access) for authentication to function as expected.
*   The commit size is limited to 125 MB.

Some GitHub Enterprise settings aren't supported. For example:

*   IP allowlist

*   Only the workspace admin can manage the connections to the [Git Repo](https://learn.microsoft.com/en-us/azure/devops/repos/get-started) such as connecting, disconnecting, or adding a branch.

 Once connected, anyone with [permission](https://learn.microsoft.com/en-us/fabric/cicd/git-integration/git-integration-process#permissions) can work in the workspace.
*   Workspaces with template apps installed can't be connected to Git.
*   [MyWorkspace](https://learn.microsoft.com/en-us/fabric/admin/portal-workspaces#govern-my-workspaces) can't connect to a Git provider.

*   Maximum length of branch name is 244 characters.
*   Maximum length of full path for file names is 250 characters. Longer names fail.
*   Maximum file size is 25 MB.
*   Folder structure is maintained up to 10 levels deep.
*   You can’t download a report/dataset as _.pbix_ from the service after deploying them with Git integration.
*   If the item’s display name has any of these characteristics, The Git folder is renamed to the logical ID (Guid) and type: 
    *   Has more than 256 characters
    *   Ends with a . or a space
    *   Contains any forbidden characters as described in [directory name limitations](https://learn.microsoft.com/en-us/fabric/cicd/git-integration/intro-to-git-integration?tabs=azure-devops#directory-name-limitations)

*   When you connect a workspace that has folders to Git, you need to commit changes to the Git repo if that [folder structure](https://learn.microsoft.com/en-us/fabric/cicd/git-integration/git-integration-process#folders) is different.

*   The name of the directory that connects to the Git repository has the following naming restrictions:

    *   The directory name can't begin or end with a space or tab.
    *   The directory name can't contain any of the following characters: "/:<>\*?|

*   The item folder (the folder that contains the item files) can't contain any of the following characters: ":<>\*?|. If you rename the folder to something that includes one of these characters, Git can't connect or sync with the workspace and an error occurs.

*   Branch out requires permissions listed in [permissions table](https://learn.microsoft.com/en-us/fabric/cicd/git-integration/git-integration-process#fabric-permissions-needed-for-common-operations).
*   There must be an available capacity for this action.
*   All [workspace](https://learn.microsoft.com/en-us/fabric/cicd/git-integration/intro-to-git-integration?tabs=azure-devops#workspace-limitations) and [branch naming limitations](https://learn.microsoft.com/en-us/fabric/cicd/git-integration/intro-to-git-integration?tabs=azure-devops#branch-and-folder-limitations) apply when branching out to a new workspace.
*   Only [Git supported items](https://learn.microsoft.com/en-us/fabric/cicd/git-integration/intro-to-git-integration#supported-items) are available in the new workspace.
*   The related branches list only shows branches and workspaces you have permission to view.
*   [Git integration](https://learn.microsoft.com/en-us/fabric/admin/git-integration-admin-settings) must be enabled.
*   When branching out, a new branch is created and the settings from the original branch aren't copied. Adjust any settings or definitions to ensure that the new meets your organization's policies.
*   When branching out to an existing workspace: 
    *   The target workspace must support a Git connection.
    *   The user must be an admin of the target workspace.
    *   The target workspace must have capacity.
    *   The workspace can't have template apps.

*   **Note that when you branch out to a workspace, any items that aren't saved to Git can get lost. We recommend that you [commit](https://learn.microsoft.com/en-us/fabric/cicd/git-integration/git-integration-process#commit-to-git) any items you want to keep before branching out.**

*   You can only sync in one direction at a time. You can’t commit and update at the same time.
*   Sensitivity labels aren't supported and exporting items with sensitivity labels might be disabled. To commit items that have sensitivity labels without the sensitivity label, [ask your administrator](https://learn.microsoft.com/en-us/fabric/admin/git-integration-admin-settings#users-can-export-workspace-items-with-applied-sensitivity-labels-to-git-repositories-preview) for help.
*   Works with [limited items](https://learn.microsoft.com/en-us/fabric/cicd/git-integration/intro-to-git-integration#supported-items). Unsupported items in the folder are ignored.
*   Duplicating names isn't allowed. Even if Power BI allows name duplication, the update, commit, or undo action fails.
*   B2B isn’t supported.
*   [Conflict resolution](https://learn.microsoft.com/en-us/fabric/cicd/git-integration/conflict-resolution) is partially done in Git.
*   During the _Commit to Git_ process, the Fabric service deletes files _inside the item folder_ that aren't part of the item definition. Unrelated files not in an item folder aren't deleted.
*   After you commit changes, you might notice some unexpected changes to the item that you didn't make. These changes are semantically insignificant and can happen for several reasons. For example: 
    *   Manually changing the item definition file. These changes are valid, but might be different than if done through the editors. For example, if you rename a semantic model column in Git and import this change to the workspace, the next time you commit changes to the semantic model, the _bim_ file will register as changed and the modified column pushed to the back of the `columns` array. This is because the AS engine that generates the _bim_ files pushes renamed columns to the end of the array. This change doesn't affect the way the item operates.
    *   Committing a file that uses _CRLF_ line breaks. The service uses _LF_ (line feed) line breaks. If you had item files in the Git repo with _CRLF_ line breaks, when you commit from the service these files are changed to _LF_. For example, if you open a report in desktop, save the project file (_.pbip_) and upload it to Git using _CRLF_.

*   Refreshing a semantic model using the [Enhanced refresh API](https://learn.microsoft.com/en-us/power-bi/connect-data/asynchronous-refresh) causes a Git diff after each refresh.

*   [Get started with Git integration](https://learn.microsoft.com/en-us/fabric/cicd/git-integration/git-get-started)
*   [Understand the Git integration process](https://learn.microsoft.com/en-us/fabric/cicd/git-integration/git-integration-process)
