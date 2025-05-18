In this article
---------------

1.   [Microsoft Fabric workspace roles](http://learn.microsoft.com/en-us/fabric/fundamentals/roles-workspaces#-workspace-roles)
2.   [Related content](http://learn.microsoft.com/en-us/fabric/fundamentals/roles-workspaces#related-content)

Workspace roles let you manage who can do what in a Microsoft Fabric workspace. Microsoft Fabric workspaces sit on top of OneLake and divide the data lake into separate containers that can be secured independently. Workspace roles in Microsoft Fabric extend the Power BI workspace roles by associating new Microsoft Fabric capabilities such as data integration and data exploration with existing workspace roles. For more information on Power BI roles, see [Roles in workspaces in Power BI](http://learn.microsoft.com/en-us/power-bi/collaborate-share/service-new-workspaces).

You can either assign roles to individuals or to security groups, Microsoft 365 groups, and distribution lists. To grant access to a workspace, assign those user groups or individuals to one of the workspace roles: Admin, Member, Contributor, or Viewer. Here's how to [give users access to workspaces](http://learn.microsoft.com/en-us/fabric/fundamentals/give-access-workspaces).

To create a new workspace, see [Create a workspace](http://learn.microsoft.com/en-us/fabric/fundamentals/create-workspaces).

Everyone in a user group gets the role that you assign. If someone is in several user groups, they get the highest level of permission that's provided by the roles that they're assigned. If you nest user groups and assign a role to a group, all the contained users have permissions.

Users in workspace roles have the following Microsoft Fabric capabilities, in addition to the existing Power BI capabilities associated with these roles.

[Workspace roles](http://learn.microsoft.com/en-us/fabric/fundamentals/roles-workspaces#-workspace-roles)
Microsoft Fabric workspace roles
--------------------------------

Expand table

| Capability                                                                                                                                            | Admin | Member | Contributor | Viewer |
| ----------------------------------------------------------------------------------------------------------------------------------------------------- | ----- | ------ | ----------- | ------ |
| Update and delete the workspace.                                                                                                                      | ✅     |        |             |        |
| Add or remove people, including other admins.                                                                                                         | ✅     |        |             |        |
| Add members or others with lower permissions.                                                                                                         | ✅     | ✅      |             |        |
| Allow others to reshare items.1                                                                                                                       | ✅     | ✅      |             |        |
| Create or modify database mirroring items.                                                                                                            | ✅     | ✅      | ✅           |        |
| Create or modify warehouse items.                                                                                                                     | ✅     | ✅      | ✅           |        |
| Create or modify SQL database items.                                                                                                                  | ✅     | ✅      | ✅           |        |
| View and read content of data pipelines, notebooks, Spark job definitions, ML models and experiments, and eventstreams.                               | ✅     | ✅      | ✅           | ✅      |
| View and read content of KQL databases, KQL query-sets, and real-time dashboards.                                                                     | ✅     | ✅      | ✅           | ✅      |
| Connect to SQL analytics endpoint of Lakehouse or the Warehouse                                                                                       | ✅     | ✅      | ✅           | ✅      |
| Read Lakehouse and Data warehouse data and shortcuts 2 with T-SQL through TDS endpoint (ReadData).                                                    | ✅     | ✅      | ✅           | ✅      |
| Read Lakehouse and Data warehouse data and shortcuts 2 through OneLake APIs and Spark (ReadAll).                                                      | ✅     | ✅      | ✅           |        |
| Read Lakehouse data through Lakehouse explorer (ReadAll).                                                                                             | ✅     | ✅      | ✅           |        |
| Subscribe to OneLake events.                                                                                                                          | ✅     | ✅      | ✅           |        |
| Write or delete data pipelines, notebooks, Spark job definitions, ML models, and experiments, and eventstreams.                                       | ✅     | ✅      | ✅           |        |
| Write or delete Eventhouses 3, KQL Querysets, Real-Time Dashboards, and schema and data of KQL Databases, Lakehouses, data warehouses, and shortcuts. | ✅     | ✅      | ✅           |        |
| Execute or cancel execution of notebooks, Spark job definitions, ML models, and experiments.                                                          | ✅     | ✅      | ✅           |        |
| Execute or cancel execution of data pipelines.                                                                                                        | ✅     | ✅      | ✅           |        |
| View execution output of data pipelines, notebooks, ML models and experiments.                                                                        | ✅     | ✅      | ✅           | ✅      |
| Schedule data refreshes via the on-premises gateway.4                                                                                                 | ✅     | ✅      | ✅           |        |
| Modify gateway connection settings.4                                                                                                                  | ✅     | ✅      | ✅           |        |

1 Contributors and Viewers can also share items in a workspace, if they have Reshare permissions.

2 Other permissions are needed to read data from shortcut destination. Learn more about [shortcut security model.](http://learn.microsoft.com/en-us/fabric/onelake/onelake-shortcuts?#types-of-shortcuts)

3 Other permissions are needed to perform certain operations on data in an Eventhouse. Learn more about the [hybrid role-based access control model](http://learn.microsoft.com/en-us/kusto/access-control/role-based-access-control?view=microsoft-fabric&preserve-view=true).

4 Keep in mind that you also need permissions on the gateway. Those permissions are managed elsewhere, independent of workspace roles and permissions.

[](http://learn.microsoft.com/en-us/fabric/fundamentals/roles-workspaces#related-content)
Related content
---------------