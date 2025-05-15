Title: Git integration process - Microsoft Fabric

URL Source: https://learn.microsoft.com/en-us/fabric/cicd/git-integration/git-integration-process?tabs=Azure,azure-devops

Markdown Content:
Save

This article explains basic Git concepts and the process of integrating Git with your Microsoft Fabric workspace.

*   Your organization's administrator must [enable Git integration](https://learn.microsoft.com/en-us/fabric/admin/git-integration-admin-settings).
*   The tenant admin must [enable cross-geo export](https://learn.microsoft.com/en-us/fabric/admin/git-integration-admin-settings#users-can-export-items-to-git-repositories-in-other-geographical-locations) if the workspace and _Azure_ repo are in two different regions. This restriction doesn't apply to GitHub.
*   The permissions you have in both the workspace and Git, as listed in the next sections, determine the actions you can take.

The following list shows what different workspace roles can do depending on their permissions in their Git repo:

*   **Admin**: Can perform any operation on the workspace, limited only by their Git role.
*   **Member/Contributor**: Once they connect to a workspace, a member/contributor can commit and update changes, depending on their Git role. For actions related to the workspace connection (for example, connect, disconnect, or switch branches) seek help from an Admin.
*   **Viewer**: Can't perform any actions. The viewer can't see any Git related information in the workspace.

The following table describes the permissions needed in the Fabric workspace to perform various common operations:

| **Operation** | **Workspace role** |
| --- | --- |
| Connect workspace to Git repo | Admin |
| Sync workspace with Git repo | Admin |
| Disconnect workspace from Git repo | Admin |
| Switch branch in the workspace (or any change in connection setting) | Admin |
| View Git connection details | Admin, Member, Contributor |
| See workspace 'Git status' | Admin, Member, Contributor |
| Update from Git | All of the following roles: Contributor in the workspace (WRITE permission on all items) Owner of the item (if the tenant switch blocks updates for nonowners) BUILD on external dependencies (where applicable) |
| Commit workspace changes to Git | All of the following roles: Contributor in the workspace (WRITE permission on all items) Owner of the item (if the tenant switch blocks updates for nonowners) BUILD on external dependencies (where applicable) |
| Create new Git branch from within Fabric | Admin |
| Branch out to another workspace | Admin, Member, Contributor |

The following table describes the Git permissions needed to perform various common operations:

*   [Azure Repos](https://learn.microsoft.com/en-us/fabric/cicd/git-integration/git-integration-process?tabs=Azure,azure-devops#tabpanel_1_Azure)
*   [GitHub Repos](https://learn.microsoft.com/en-us/fabric/cicd/git-integration/git-integration-process?tabs=Azure,azure-devops#tabpanel_1_GitHub)

| **Operation** | **Git permissions** |
| --- | --- |
| Connect workspace to Git repo | Read=Allow |
| Sync workspace with Git repo | Read=Allow |
| Disconnect workspace from Git repo | No permissions are needed |
| Switch branch in the workspace (or any change in connection setting) | Read=Allow (in target repo/directory/branch) |
| View Git connection details | Read or None |
| See workspace 'Git status' | Read=Allow |
| Update from Git | Read=Allow |
| Commit workspace changes to Git | Read=Allow Contribute=Allow branch policy should allow direct commit |
| Create new Git branch from within Fabric | Role=Write Create branch=Allow |
| Branch out to another workspace | Read=Allow Create branch=Allow |

Only a workspace admin can connect a workspace to a Git Repos, but once connected, anyone with permissions can work in the workspace. If you're not an admin, ask your admin for help with connecting.

When you [connect a workspace to Git](https://learn.microsoft.com/en-us/fabric/cicd/git-integration/git-get-started#connect-a-workspace-to-a-git-repo), Fabric syncs between the two locations so they have the same content. During this initial sync, if either the workspace or Git branch is empty while the other has content, the content is copied from the nonempty location to the empty one. If both the workspace and Git branch have content, you have to decide which direction the sync should go.

*   If you commit your workspace to the Git branch, all supported workspace content is exported to Git and overwrites the current Git content.
*   If you update the workspace with the Git content, the workspace content is overwritten, and you lose your workspace content. Since a Git branch can always be restored to a previous stage while a workspace can’t, if you choose this option, you're asked to confirm.

![Image 1: Screenshot of dialog asking which direction to sync if both Git and the workspace have content.](https://learn.microsoft.com/en-us/fabric/cicd/git-integration/media/git-integration-process/git-sync-direction.png)

If you don’t select which content to sync, you can’t continue to work.

![Image 2: Screenshot notification that you can't continue working until workspace is synced.](https://learn.microsoft.com/en-us/fabric/cicd/git-integration/media/git-integration-process/sync-direction-continue.png)

When connected and synced, the workspace structure is mirrored in the Git repository, including folders structure. Workspace items in folders are exported to folders with the same name in the Git repo. Conversely, items in Git folders are imported to folders with the same name in the workspace.

Note

Since folder structure is retained, if your workspace has folders and the connected Git folder doesn't yet have subfolders, they're considered to be different. You get an _uncommitted changes_ status in the source control panel and you need to commit the changes to Git before updating the workspace. If you update first, the Git folder structure **overwrites the workspace** folder structure. For more information, see [Handling folder changes safely](https://learn.microsoft.com/en-us/fabric/cicd/git-integration/git-integration-process?tabs=Azure,azure-devops#handling-folder-changes-safely).

![Image 3: Screenshot of workspace and corresponding Git branch with subfolders.](https://learn.microsoft.com/en-us/fabric/cicd/git-integration/media/git-integration-process/git-subfolders.png)

*   Empty folders aren't copied to Git. When you create or move items to a folder, the folder is created in Git.
*   Empty folders in Git are deleted automatically.
*   Empty folders in the workspace aren't deleted automatically even if all items are moved to different folders.
*   Folder structure is retained up to 10 levels deep.

If your workspace has folders and the connected Git folder doesn't yet have subfolders, they're considered to be different because the folder structure is different. When you connect a workspace that has folders to Git, you get an _uncommitted changes_ status in the source control panel and you need to commit the changes to Git before updating the workspace.

If you can't make changes to the connected branch directly, due to branch policy or permissions, we recommend using the _Checkout Branch_ option:

1.   [Checkout a New Branch](https://learn.microsoft.com/en-us/fabric/cicd/git-integration/conflict-resolution#resolve-conflict-in-git): Use the checkout branch feature to create a branch with the updated state of your Fabric workspace.
2.   Commit Folder Changes: Any workspace folder changes can then be committed to this new branch.
3.   Merge Changes: Use your regular pull request (PR) and merge processes to integrate these updates back into the original branch.

If you try connecting to a workspace that's already [connected to Git](https://learn.microsoft.com/en-us/fabric/cicd/git-integration/manage-branches), you might get the following message:

![Image 4: Screenshot of error message telling you to sign in to a Git account.](https://learn.microsoft.com/en-us/fabric/cicd/git-integration/media/git-integration-process/sign-into-git.png)

Go to the **Accounts** tab on the right side of the Source control panel, choose an account, and connect to it.

![Image 5: Screenshot of Accounts tab with user connecting to a GitHub account.](https://learn.microsoft.com/en-us/fabric/cicd/git-integration/media/git-integration-process/connect.png)

After you connect, the workspace displays a _Git status_ column that indicates the sync state of each item in the workspace in relation to the items in the remote branch.

![Image 6: Screenshot if items in a workspace with their Git status outlined.](https://learn.microsoft.com/en-us/fabric/cicd/git-integration/media/git-integration-process/git-status.png)

Each item has one of the following statuses:

*   ![Image 7](https://learn.microsoft.com/en-us/fabric/cicd/git-integration/media/git-integration-process/synced-icon.png) Synced (the item is the same in the workspace and Git branch)
*   ![Image 8](https://learn.microsoft.com/en-us/fabric/cicd/git-integration/media/git-integration-process/conflict-icon.png) Conflict (the item was changed in both the workspace and Git branch)
*   ![Image 9](https://learn.microsoft.com/en-us/fabric/cicd/git-integration/media/git-integration-process/unsupported-icon.png) Unsupported item
*   ![Image 10](https://learn.microsoft.com/en-us/fabric/cicd/git-integration/media/git-integration-process/uncommitted-icon.png) Uncommitted changes in the workspace
*   ![Image 11](https://learn.microsoft.com/en-us/fabric/cicd/git-integration/media/git-integration-process/update-required-icon.png) Update required from Git
*   ![Image 12](https://learn.microsoft.com/en-us/fabric/cicd/git-integration/media/git-integration-process/warning.png) Item is identical in both places but needs to be updated to the last commit

As long as you’re connected, the following information appears at the bottom of your screen:

*   Connected branch
*   Time of last sync
*   Link to the last commit that the workspace is synced to

![Image 13: Screenshot of sync information that appears on the bottom of the screen when connected to Git.](https://learn.microsoft.com/en-us/fabric/cicd/git-integration/media/git-integration-process/sync-info.png)

On top of the screen is the **Source control** icon. It shows the number of items that are different in the workspace and Git branch. When changes are made either to the workspace or the Git branch, the number is updated. When the workspace is synced with the Git branch, the Source control icon displays a _0_.

![Image 14: Screenshot of the source control icon showing zero items changed.](https://learn.microsoft.com/en-us/fabric/cicd/git-integration/media/git-integration-process/source-control-zero.png)

Select the Source control icon to open the **Source control** panel.

The source control pane has three tabs on the side:

*   [Commits and updates](https://learn.microsoft.com/en-us/fabric/cicd/git-integration/git-integration-process?tabs=Azure,azure-devops#commits-and-updates)
*   [Branches](https://learn.microsoft.com/en-us/fabric/cicd/git-integration/git-integration-process?tabs=Azure,azure-devops#branches)
*   [Account details](https://learn.microsoft.com/en-us/fabric/cicd/git-integration/git-integration-process?tabs=Azure,azure-devops#account-details)

When changes are made either to the workspace or the Git branch, the source control icon shows the number of items that are different. Select the source control icon to open the Source control panel.

The **Commit and update** panel has two sections.

**Changes** shows the number of items that were changed in the workspace and need to be committed to Git.

**Updates** shows the number of items that were modified in the Git branch and need to be updated to the workspace.

In each section, the changed items are listed with an icon indicating the status:

*   ![Image 15](https://learn.microsoft.com/en-us/fabric/cicd/git-integration/media/git-integration-process/new-icon.png) new
*   ![Image 16](https://learn.microsoft.com/en-us/fabric/cicd/git-integration/media/git-integration-process/modified-icon.png) modified
*   ![Image 17](https://learn.microsoft.com/en-us/fabric/cicd/git-integration/media/git-integration-process/deleted-icon.png) deleted
*   ![Image 18](https://learn.microsoft.com/en-us/fabric/cicd/git-integration/media/git-integration-process/conflict-icon.png) conflict

The Refresh button ![Image 19](https://learn.microsoft.com/en-us/fabric/cicd/git-integration/media/git-integration-process/refresh-icon.png) on top of the panel updates the list of changes and updates.

![Image 20: Screenshot of the source control panel showing the status of the changed items.](https://learn.microsoft.com/en-us/fabric/cicd/git-integration/media/git-integration-process/source-control-panel-items.png)

*   Items in the workspace that were changed are listed in the _Changes_ section. When there's more than one changed item, you can select which items to commit to the Git branch.
*   If there were updates made to the Git branch, commits are disabled until you update your workspace.

*   Unlike _commit_ and _undo_, the _Update_ command always updates the entire branch and syncs to the most recent commit. You can’t select specific items to update.
*   If changes were made in the workspace and in the Git branch _on the same item_, updates are disabled until the [conflict is resolved](https://learn.microsoft.com/en-us/fabric/cicd/git-integration/conflict-resolution).

Read more about how to [commit](https://learn.microsoft.com/en-us/fabric/cicd/git-integration/git-get-started#commit-changes-to-git) and [update](https://learn.microsoft.com/en-us/fabric/cicd/git-integration/git-get-started#update-workspace-from-git). Read more about the update process and how to [resolve conflicts](https://learn.microsoft.com/en-us/fabric/cicd/git-integration/conflict-resolution).

The _Branches_ tab of the Source control panel enables you to manage your branches and perform branch related actions. It has two main sections:

*   **Actions you can take on the current branch**:

    *   [_Branch out to another workspace_](https://learn.microsoft.com/en-us/fabric/cicd/git-integration/manage-branches#scenario-2---develop-using-another-workspace) (contributor and above): Creates a new workspace, or switches to an existing workspace based on the last commit to the current workspace. It then connects to the target workspace and branch.
    *   [_Checkout new branch_](https://learn.microsoft.com/en-us/fabric/cicd/git-integration/conflict-resolution#resolve-conflict-in-git) (must be workspace admin): Creates a new branch based on the last synced commit in the workspace and changes the Git connection in the current workspace. It doesn't change the workspace content.
    *   [_Switch branch_](https://learn.microsoft.com/en-us/fabric/cicd/git-integration/manage-branches#switch-branches) (must be workspace admin): Syncs the workspace with another new or existing branch and overrides all items in the workspace with the content of the selected branch.

![Image 21: Screenshot of the branch out tab in the source control panel.](https://learn.microsoft.com/en-us/fabric/cicd/git-integration/media/git-integration-process/branch-out.png)

*   **Related branches**.

 The _Branches_ tab also has a list of related workspaces you can select and switch to. A related workspace is one with the same connection properties as the current branch, such as the same organization, project, repository, and git folder.

 This feature allows you to navigate to workspaces connected to other branches related to the context of your current work, without having to look for them in your list of Fabric workspaces.

 To open the relevant workspace, select item in the list.

![Image 22: Screenshot showing a list of related branches that the user can switch to.](https://learn.microsoft.com/en-us/fabric/cicd/git-integration/media/git-integration-process/related-branches.png)

For more information, see [Branching out limitations](https://learn.microsoft.com/en-us/fabric/cicd/git-integration/git-integration-process?tabs=Azure,azure-devops#branching-out-limitations).

The Account details tab shows details of the GitHub account that the user is connected to. It has two sections. The top section shows the Git provider and the account name. The bottom section shows the repository and branch that the workspace is connected to. Currently, this tab is only available for workspaces connected to GitHub.

GitHub account details include:

*   Git account details

    *   Provider
    *   Account name

*   Git repository

*   Branch

![Image 23: Screenshot of accounts tab in Source control panel showing the Git details and repository and branch names.](https://learn.microsoft.com/en-us/fabric/cicd/git-integration/media/git-integration-process/github-account-details.png)

*   The [authentication method](https://learn.microsoft.com/en-us/entra/identity/authentication/concept-authentication-methods-manage#authentication-methods-policy) in Fabric must be at least as strong as the authentication method for Git. For example, if Git requires multifactor authentication, Fabric needs to require multifactor authentication as well.
*   Power BI Datasets connected to Analysis Services aren't supported at this time.
*   Submodules aren't supported.
*   Sovereign clouds aren't supported.

*   [Azure DevOps limitations](https://learn.microsoft.com/en-us/fabric/cicd/git-integration/git-integration-process?tabs=Azure,azure-devops#tabpanel_1_azure-devops)
*   [GitHub limitations](https://learn.microsoft.com/en-us/fabric/cicd/git-integration/git-integration-process?tabs=Azure,azure-devops#tabpanel_1_github)

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
    *   Contains any forbidden characters as described in [directory name limitations](https://learn.microsoft.com/en-us/fabric/cicd/git-integration/git-integration-process?tabs=Azure,azure-devops#directory-name-limitations)

*   When you connect a workspace that has folders to Git, you need to commit changes to the Git repo if that [folder structure](https://learn.microsoft.com/en-us/fabric/cicd/git-integration/git-integration-process#folders) is different.

*   The name of the directory that connects to the Git repository has the following naming restrictions:

    *   The directory name can't begin or end with a space or tab.
    *   The directory name can't contain any of the following characters: "/:<>\*?|

*   The item folder (the folder that contains the item files) can't contain any of the following characters: ":<>\*?|. If you rename the folder to something that includes one of these characters, Git can't connect or sync with the workspace and an error occurs.

*   Branch out requires permissions listed in [permissions table](https://learn.microsoft.com/en-us/fabric/cicd/git-integration/git-integration-process#fabric-permissions-needed-for-common-operations).
*   There must be an available capacity for this action.
*   All [workspace](https://learn.microsoft.com/en-us/fabric/cicd/git-integration/git-integration-process?tabs=Azure,azure-devops#workspace-limitations) and [branch naming limitations](https://learn.microsoft.com/en-us/fabric/cicd/git-integration/git-integration-process?tabs=Azure,azure-devops#branch-and-folder-limitations) apply when branching out to a new workspace.
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

*   [Manage branches](https://learn.microsoft.com/en-us/fabric/cicd/git-integration/manage-branches)
*   [Resolve errors and conflicts](https://learn.microsoft.com/en-us/fabric/cicd/git-integration/conflict-resolution)
