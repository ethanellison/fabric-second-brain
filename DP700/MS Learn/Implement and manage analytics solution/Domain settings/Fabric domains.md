Title: Domains - Microsoft Fabric

URL Source: https://learn.microsoft.com/en-us/fabric/governance/domains

Markdown Content:
This article introduces key concepts of domains in Fabric, and shows how to set up and manage them. To get started planning domains for your organization, see [[Fabric domain best practices]]

Today, organizations are facing massive growth in data, and there's an increasing need to be able to organize and manage that data in a logical way that facilitates more targeted and efficient use and governance.

To meet this challenge, organizations are shifting from traditional IT centric data architectures, where the data is governed and managed centrally, to more federated models organized according to business needs. This federated data architecture is called _data mesh_. A data mesh is a decentralized data architecture that organizes data by specific business domains, such as marketing, sales, human resources, etc.

Currently, Microsoft Fabric's data mesh architecture primarily supports organizing data into domains and enabling data consumers to be able to filter and find content by domain. It also enables federated governance, which means that some governance currently controlled at the tenant level can be [delegated to domain-level control](https://learn.microsoft.com/en-us/fabric/governance/domains#domain-settings-delegation), enabling each business unit/department to define its own rules and restrictions according to its specific business needs.

In Fabric, a domain is a way of logically grouping together all the data in an organization that is relevant to a particular area or field. One of the most common uses for domains is to group data by business department, making it possible for departments to manage their data according to their specific regulations, restrictions, and needs.

To group data into domains, workspaces are associated with domains. When a workspace is associated with a domain, all the items in the workspace are also associated with the domain, and they receive a domain attribute as part of their metadata. Currently, the association of workspaces and the items in them with domains primarily enables a better consumption experience. For instance, in the [OneLake catalog](https://learn.microsoft.com/en-us/fabric/governance/onelake-catalog-overview), users can filter content by domain in order find content that is relevant to them. In addition, some tenant-level settings for managing and governing data can be [delegated to the domain level](https://learn.microsoft.com/en-us/fabric/governance/domains#domain-settings-delegation), thus allowing domain-specific configuration of those settings.

Note

Domain assignment doesn't affect item visibility or accessibility for tenant users. Item discovery, visibility, and access depend on such things as workspace role and item permissions, but not domain assignment.

Likewise, all users within a tenant can see all the domains defined in the tenant, regardless of their specific domain roles. For example, users who are neither contributors nor admins of a domain called "Finance" in their tenant can still see this domain in the [domains filter](https://learn.microsoft.com/en-us/fabric/governance/onelake-catalog-explore#scope-the-catalog-to-a-particular-domain) of the Onelake Catalog.

A subdomain is a way for fine tuning the logical grouping of your data. You can create subdomains under domains. For information about how to create subdomains, see [Create subdomains](https://learn.microsoft.com/en-us/fabric/governance/domains#create-subdomains).

There are three roles involved in the creation and management of domains:

*   **Fabric admin** (or higher): Fabric admins can create and edit domains, specify domain admins and domain contributors, and associate workspaces with domains. Fabric admins see all the defined domains on the Domains tab in the admin portal, and they can edit and delete domains.

*   **Domain admin**: Ideally, the domain admins of a domain are the business owners or designated experts. They should be familiar with the data in their area and the regulations and restrictions that are relevant to it.

Domain admins can access to the **Domains** tab in the admin portal, but they can only see and edit the domains they're admins of. Domain admins can update the domain description, define/update domain contributors, and associate workspaces with the domain. They also can define and update the [domain image](https://learn.microsoft.com/en-us/fabric/governance/domains#domain-image) and [override tenant settings](https://learn.microsoft.com/en-us/fabric/governance/domains#domain-settings-delegation) for any specific settings the tenant admin has delegated to the domain level. They can't delete the domain, change the domain name, or add/delete other domain admins.

*   **Domain contributor**: Domain contributors are [workspace admins](https://learn.microsoft.com/en-us/fabric/fundamentals/roles-workspaces) whom a domain or Fabric admin has authorized to assign the workspaces they're the admins of to a domain, or to change the current domain assignment.

Domain contributors assign the workspaces they're an admin of in the settings of the workspace itself. They don't have access to the **Domains** tab in the admin portal.

Note

Remember, to be able to assign a workspace to a domain, a domain contributor must be a workspace admin (that is, have the [Admin role](https://learn.microsoft.com/en-us/fabric/fundamentals/roles-workspaces) in the workspace). 

To allow domain-specific configuration, some tenant-level settings for managing and governing data can be [delegated to the domain level](https://learn.microsoft.com/en-us/fabric/governance/domains#delegate-settings-to-the-domain-level). Domain settings delegation enables each business unit/department to define its own rules and restrictions according to its specific business needs.

When users look for data items in the OneLake catalog, they might want to see only the data items that belong to a particular domain. To do this, they can select the domain in the domain selector in the OneLake catalog to display only items belonging to that domain. To remind them which domain's data items they're seeing, you can choose an image to represent your domain. Then, when your domain is selected in the domain selector, the image becomes part of OneLake catalog's theme, as illustrated in the following image.

![Image 1: Screenshot of the OneLake catalog with a domain image.](https://learn.microsoft.com/en-us/fabric/governance/media/domains/domain-image-onelake-catalog.png)

For information about how to specify an image for a domain, see [Specify a domain image](https://learn.microsoft.com/en-us/fabric/governance/domains#specify-a-domain-image).

A default domain is a domain that has been defined as the default domain for specified users and/or security groups. When you define a domain as the default domain for specified users and/or security groups, the following happens:

1.   The system scans the organization's workspaces. When it finds a workspace whose admin is a specified user or member of a specified security group: 
    *   If the workspace already has a domain assignment, it is preserved. The default domain doesn't override the current assignment.
    *   If the workspace is unassigned, it is assigned to the default domain.

2.   After this, whenever a specified user or member of a specified security group creates a new workspace, it is assigned to the default domain.

The specified users and/or members of the specified security groups generally automatically become domain contributors of workspaces that are assigned in this manner.

For information about defining a domain as a default domain, see [Define the domain as a default domain](https://learn.microsoft.com/en-us/fabric/governance/domains#define-the-domain-as-a-default-domain).

Before you start creating domains for your organization, it is recommended to review [Best practices for planning and creating domains in Microsoft Fabric](https://learn.microsoft.com/en-us/fabric/governance/domains-best-practices).

To create domain, you must be a Fabric admin.

1.   Open the admin portal and select the **Domains** tab.

2.   On the **Domains** tab, select **Create new domain**.

[![Image 2: Screenshot of domains page.](https://learn.microsoft.com/en-us/fabric/governance/media/domains/domains-page.png)](https://learn.microsoft.com/en-us/fabric/governance/media/domains/domains-page.png#lightbox)

3.   In the **New domain** dialog that appears, provide a name (mandatory) and specify domain admins (optional). If you don't specify domain admins, you can do this later in the domain settings.

![Image 3: Screenshot of domains new details section.](https://learn.microsoft.com/en-us/fabric/governance/media/domains/domains-new-name.png)

4.   Select **Create**. The domain is created, and you can continue configuring the domain as described in the following sections.

Once you create some domains, you can refine the logic of the way you're structuring your data by creating subdomains for the domains.

You organize your data into the appropriate domains and subdomains by assigning the workspaces the data is located in to the relevant domain or subdomain. When a workspace is assigned to a domain, all the items in the workspace are associated with the domain.

To create subdomains for a domain, you must be Fabric admin or domain admin.

1.   Open the domain you want to create a subdomain for and select **New subdomain**.

![Image 4: Screenshot of open domain menu option.](https://learn.microsoft.com/en-us/fabric/governance/media/domains/select-new-subdomain.png)

2.   Provide a name for the subdomain in the **New subdomain** dialog that appears. When done, select **Create**.

![Image 5: Screenshot of new subdomain dialog.](https://learn.microsoft.com/en-us/fabric/governance/media/domains/new-subdomain-dialog.png)

Note

Subdomains don't have their own domain admins. A subdomain's domain admins are the domain admins of its parent domain. 

To assign workspaces to a domain or subdomain in the admin portal, you must be a Fabric admin or a domain admin.

1.   Go to the domain or subdomain's page and select **Assign workspaces**.

![Image 6: Screenshot showing assign workspaces link.](https://learn.microsoft.com/en-us/fabric/governance/media/domains/domain-assign-workspaces-link.png)

2.   In the **Assign workspaces to this domain** side pane, select how to assign the workspaces.

![Image 7: Screenshot showing assign workspaces side pane.](https://learn.microsoft.com/en-us/fabric/governance/media/domains/domain-assign-workspaces-to-this-domain.png)

*   **Assign by workspace name**

    *   Some organizations have naming conventions for workspaces that make it easy to identify the data's business context.
    *   You can search for and select multiple workspaces at once
    *   If a workspace is already associated with another domain, you'll see an icon next to the specific name. If you chose to continue the action, a warning message pops up, but you'll be able to continue and override the previous association.

*   **Assign by workspace admin**

    *   You can select specific users or security groups as per your business structure. When you confirm the selection, all the workspaces the users and security groups are admins of will be associated to the domain.
    *   This action excludes "My workspaces".
    *   If some of the workspaces are already associated with another domain, a warning message will pop up, but you'll be able to continue and override the previous association.
    *   This action affects existing workspaces only. It won't affect workspaces the selected users create after the action has been performed.

*   **Assign by capacity**

    *   Some organizations have dedicated capacities per department/business unit.
    *   You can search for and select multiple capacities at once. When you confirm your selection, all the workspaces associated to the selected capacities will be assigned to the domain.
    *   If some of the workspaces are already associated with another domain, a warning message will pop up, but you'll be able to continue and override the previous association.
    *   This action excludes "My workspaces".
    *   This action affects existing workspaces only. It won't affect workspaces that are assigned to the specified capacities after the action has been performed.

To unassign a workspace from a domain or subdomain, select the checkbox next to the workspace name and then select the **Unassign** button above the list. You can select several checkboxes to unassign more than one workspace at a time.

![Image 8: Screenshot showing how to unassign workspaces.](https://learn.microsoft.com/en-us/fabric/governance/media/domains/domain-unassign-workspace.png)

You configure domain and subdomain settings on the domain or subdomain's **Domain settings** side pane.

The domain settings side pane has the following tabs:

*   [General settings](https://learn.microsoft.com/en-us/fabric/governance/domains#edit-name-and-description): Edit domain name and description
*   [Image](https://learn.microsoft.com/en-us/fabric/governance/domains#specify-a-domain-image): Specify domain image
*   [Admins](https://learn.microsoft.com/en-us/fabric/governance/domains#specify-domain-admins): Specify domain admins
*   [Contributors](https://learn.microsoft.com/en-us/fabric/governance/domains#specify-domain-contributors): Specify domain contributors
*   [Default domain](https://learn.microsoft.com/en-us/fabric/governance/domains#define-the-domain-as-a-default-domain): Set up domain as a default domain
*   [Delegated settings](https://learn.microsoft.com/en-us/fabric/governance/domains#delegate-settings-to-the-domain-level): Override tenant-level settings

Note

Subdomains currently have general settings only.

To open the **Domain settings** side pane, open the domain or subdomain and select **Domain settings** (for subdomains, **Subdomain settings**).

![Image 9: Screenshot showing how to open the domain settings pane.](https://learn.microsoft.com/en-us/fabric/governance/media/domains/open-domain-settings.png)

Alternatively, for domains, you can hover over the domain on the Domain tab, select **More options (...)**, and choose **Settings**.

![Image 10: Screenshot of the domain settings menu option.](https://learn.microsoft.com/en-us/fabric/governance/media/domains/open-domain-settings-menu-option.png)

1.   Select **General settings** and then edit the name and description fields as desired.

![Image 11: Screenshot showing the domains name and description fields.](https://learn.microsoft.com/en-us/fabric/governance/media/domains/domain-edit-details.png)

Note

Domain admins can only edit the description field. 
2.   When done, select **Apply**.

Select **Image** and then select **Select an image**.

In the photo gallery that pops up you can choose an image or color to represent your domain in the OneLake catalog when your domain is selected.

![Image 12: Screenshot showing the domains image gallery.](https://learn.microsoft.com/en-us/fabric/governance/media/domains/domain-image-gallery.png)

You must be a Fabric admin to specify domain admins.

Select **Admins** and then specify who can change domain settings and add or remove workspaces. When done, select **Apply**.

![Image 13: Screenshot showing domain admins specification section.](https://learn.microsoft.com/en-us/fabric/governance/media/domains/domain-specify-domain-admins.png)

You must be a domain admin of the domain or a Fabric admin to specify domain contributors.

Select **Contributors** and then specify who can assign workspaces to the domain. You can specify everyone in the organization (default), specific users/groups only, or you can allow only tenant admins and the specific domain admins to assign workspaces to the domain. When done, select **Apply**.

![Image 14: Screenshot showing domain contributor specification section.](https://learn.microsoft.com/en-us/fabric/governance/media/domains/domain-specify-domain-contributors.png)

Note

For domain contributors to be able to associate their workspaces with their domains, they must have an admin role in the workspaces they are trying to associate with the domain.

To define a domain as a default domain, you must be a Fabric admin or a domain admin of the domain.

Select **Default domain** and specify users and/or security groups. When you add people to the default domain list, unassigned workspaces they're admins of, and new workspaces they create, will automatically be assigned to the domain. For a detailed description of the process, see [Default domain](https://learn.microsoft.com/en-us/fabric/governance/domains#default-domain).

![Image 15: Screenshot showing default domain specification section.](https://learn.microsoft.com/en-us/fabric/governance/media/domains/domain-specify-default-domain.png)

Note

The users and/or members of the security groups specified in the default domain definition generally automatically become domain contributors of the workspaces that get assigned to the domain via the default domain mechanism.

Some tenant-level settings can potentially be overridden at the domain level. To see these settings, select **Delegated Settings**. The following admin settings can potentially be overridden.

If the [domain-level default sensitivity label feature](https://learn.microsoft.com/en-us/fabric/governance/domain-default-sensitivity-label) is enabled in your organization, you can specify a sensitivity label that will be applied by default to items in workspaces that are assigned to the domain.

To specify a default sensitivity label for your domain, you must be a Fabric admin or a domain admin of the domain.

Expand **Delegated Settings** and choose **Information protection**. You'll see the option **Set a default label for this domain**. Select the drop down menu and select the desired sensitivity label. The label will be applied to items in workspaces associated with the domain according to the logic described in [Domain-level default sensitivity labels in Microsoft Fabric](https://learn.microsoft.com/en-us/fabric/governance/domain-default-sensitivity-label).

Certification is a way for organizations to label items that it considers to be quality items. For more information about certification, see [Endorsement](https://learn.microsoft.com/en-us/fabric/governance/endorsement-overview).

Certification settings at the domain level mean you can:

*   Enable or disable certification of items that belong to the domain.
*   Specify certifiers who are experts in the domain.
*   Provide a URL to documentation that is relevant to certification in the domain.

To override the tenant-level certification settings, expand the certification section, select the **Override tenant admin selection** checkbox, and configure the settings as desired.

![Image 16: Screenshot of certification override.](https://learn.microsoft.com/en-us/fabric/governance/media/domains/domain-override-tenant-admin-selection.png)

For descriptions of the things you need to set, see [Set up certification](https://learn.microsoft.com/en-us/fabric/admin/endorsement-setup#set-up-certification).

Most of the actions available from the UI are available through the Fabric REST Admin APIs for domains. For more information, see [Domains API reference](https://learn.microsoft.com/en-us/rest/api/fabric/admin/domains).

Whenever a domain is created, edited, or deleted, that activity is recorded in the audit log for Fabric. You can track these activities in the unified audit log or in the Fabric activity log. For information about the information in the Fabric auditing schema that's specific to domains, see [Audit schema for domains](https://learn.microsoft.com/en-us/fabric/governance/domains-audit-schema).

*   [Domain management tenant settings](https://learn.microsoft.com/en-us/fabric/admin/service-admin-portal-domain-management-settings)
*   [Domain design - best practices](https://learn.microsoft.com/en-us/fabric/governance/domains-best-practices)
*   [Domain-level default sensitivity labels](https://learn.microsoft.com/en-us/fabric/governance/domain-default-sensitivity-label)
*   [Microsoft Fabric REST Admin APIs for domains](https://learn.microsoft.com/en-us/rest/api/fabric/admin/domains)
*   [Audit schema for domains](https://learn.microsoft.com/en-us/fabric/governance/domains-audit-schema)
*   [Admin role in workspaces](https://learn.microsoft.com/en-us/fabric/fundamentals/roles-workspaces)
*   [Blog: Easily implement data mesh architecture with domains in Fabric](https://blog.fabric.microsoft.com/blog/easily-implement-data-mesh-architecture-with-domains-in-fabric/)
