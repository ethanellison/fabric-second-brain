Title: Audit schema for domains in Fabric - Microsoft Fabric

URL Source: https://learn.microsoft.com/en-us/fabric/governance/domains-audit-schema

Markdown Content:
Audit schema for domains in Fabric
==================================

*   Article
*   03/24/2024
*    1 contributor 

Feedback

In this article
---------------

Whenever a domain is created, edited, or deleted, that activity is recorded in the audit log for Fabric. You can track these activities using [Microsoft Purview Audit](https://compliance.microsoft.com/auditlogsearch).

This article explains the information in the Fabric auditing schema that's specific to domains. This information is recorded in the OperationProperties section of the details side pane that opens when you select a domain-related activity on the Audit search page.

On the Audit search page:

1.   Search for activities by their operation names.
2.   When the search finishes, select the search to open the search results page.
3.   On the search results page, select one of the search results to open the side pane that displays the record details.

For domains, the domain-specific details are found under the **OperationProperties** section of the side pane, in json format.

| Field               | Type     | Must appear in the schema | Value                                                |
| ------------------- | -------- | ------------------------- | ---------------------------------------------------- |
| OperationName       | Edm.Enum | Yes                       | Activity name as described in the following table.   |
| OperationProperties | Edm.Enum | Yes                       | Per the properties described in the following table. |

| Activity flow                                            | Activity operation name                       | Properties                                                                                                                                                                                                                                                                                                                                                |
| -------------------------------------------------------- | --------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Create domain/sub-domain                                 | InsertDataDomainAsAdmin                       | **operationName**: - InsertDataDomainAsAdmin **operationProperties**: - DataDomainObjectId: <guid> - DataDomainDisplayName: <string> - ParentObjectId?: <guid>                                                                                                                                                                                            |
| Delete domain/sub-domain                                 | DeleteDataDomainAsAdmin                       | **operationName**: - DeleteDataDomainAsAdmin **operationProperties**: - DataDomainObjectId: <guid> - DataDomainDisplayName: <string> - ParentObjectId?: <guid>                                                                                                                                                                                            |
| Update domain/sub-domain                                 | UpdateDataDomainAsAdmin                       | **operationName**: - UpdateDataDomainAsAdmin **operationProperties**: - DataDomainObjectId: <guid> - DataDomainDisplayName: <DataDomainObjectId> - ParentObjectId?: <guid>                                                                                                                                                                                |
| Assign/Unassign workspace to the domain                  | UpdateDataDomainFoldersRelationsAsAdmin       | **operationName**: - UpdateDataDomainFoldersRelationsAsAdmin **operationProperties**: - DataDomainObjectId: <guid> - DataDomainDisplayName: <string> - ParentObjectId?: <guid> - FoldersToSetCounter?: <long> - FoldersToUnsetCount?: <long>                                                                                                              |
| Unassign all workspaces to the domain                    | DeleteAllDataDomainFoldersRelationsAsAdmin    | **operationName**: - DeleteAllDataDomainFoldersRelationsAsAdmin **operationProperties**: - DataDomainObjectId: <guid> - DataDomainDisplayName: <string> - ParentObjectId?: <guid>                                                                                                                                                                         |
| Assign/Unassign workspaces to the domain as contributor  | UpdateDataDomainFoldersRelationsAsContributor | **operationName**: - UpdateDataDomainFoldersRelationsAsContributor **operationProperties**: - DataDomainObjectId: <guid> - DataDomainDisplayName: <string> - ParentObjectId?: <guid> - FoldersToSetCounter?: <long> - FoldersToUnsetCount?: <long>                                                                                                        |
| Remove domain from workspace settings as workspace owner | DeleteDataDomainFolderRelationsAsFolderOwner  | **operationName**: - DeleteDataDomainFoldersRelationsAsFolderOwner **operationProperties**: - DataDomainObjectId: <guid> - DataDomainDisplayName: <string> - ParentObjectId?: <guid> - FolderId?: <long>                                                                                                                                                  |
| Initiate/Process bulk assign domain by workspace owners  | BulkAssignDataDomainByWsOwnersAsAdmin?        |                                                                                                                                                                                                                                                                                                                                                           |
| Initiate/Process bulk assign domain by capacities        | BulkAssignDataDomainByCapacitiesAsAdmin?      |                                                                                                                                                                                                                                                                                                                                                           |
| Add/Delete/Update domain access                          | UpdateDataDomainAccessAsAdmin                 | **operationName**: - UpdateDataDomainAccessAsAdmin **operationProperties**: - DataDomainObjectId: <guid> - DataDomainDisplayName: <string> - ParentObjectId?: <guid> - Value: <long>: * 0 - None * 7 - Contributor * 15 - Admin - UsersToSetCounter?: <long> - UsersToUnsetCounter?: <long> - GroupsToSetCounter?: <long> - GroupsToUnsetCounter?: <long> |
| Add/Delete/Update default domain                         | UpdateDefaultDataDomainAsAdmin                | **operationName**: - UpdateDefaultDataDomainAsAdmin **operationProperties**: - DataDomainObjectId: <guid> - DataDomainDisplayName: <string> - ParentObjectId?: <guid> - UsersToSetCounter?: <long> - UsersToUnsetCounter?: <long> - GroupsToSetCounter?: <long> - GroupsToUnsetCounter?: <long>                                                           |
| Add/Delete/Update contributors                           | UpdateDataDomainContributorsScopeAsAdmin      | **operationName**: - UpdateDataDomainContributorsScopeAsAdmin **operationProperties**: - DataDomainObjectId: <guid> - DataDomainDisplayName: <string> - ParentObjectId?: <guid> - Value: <long>: * 0 - AllTenant * 1 - SpecificUsersAndGroups * 2 - AdminsOnly                                                                                            |
| Set/Remove domain branding                               | UpdateDataDomainBrandingAsAdmin               | **operationName**: - UpdateDataDomainBrandingAsAdmin **operationProperties**: - DataDomainObjectId: <guid> - DataDomainDisplayName: <string> - ParentObjectId?: <guid> - Value: <long> // Branding ID                                                                                                                                                     |
| Updated delegation at domain level                       | UpdateDomainTenantSettingDelegation           |                                                                                                                                                                                                                                                                                                                                                           |
| Deleted override at domain level                         |                                               |                                                                                                                                                                                                                                                                                                                                                           |

[](https://learn.microsoft.com/en-us/fabric/governance/domains-audit-schema#related-content)
Related content
---------------

*   [Domains](https://learn.microsoft.com/en-us/fabric/governance/domains)
*   [Domains management tenant settings](https://learn.microsoft.com/en-us/fabric/admin/service-admin-portal-domain-management-settings)
*   [Microsoft Fabric REST Admin APIs for domains](https://learn.microsoft.com/en-us/rest/api/fabric/admin/domains)
*   [Track user activities in Fabric](https://learn.microsoft.com/en-us/fabric/admin/track-user-activities)

* * *

