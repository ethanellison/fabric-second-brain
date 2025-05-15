Title: Getting Started with the SQL Database Projects Extension - Azure Data Studio

URL Source: https://learn.microsoft.com/en-us/azure-data-studio/extensions/sql-database-project-extension-getting-started?view=sql-server-ver16

Markdown Content:
Getting started with the SQL Database Projects extension
========================================================

*   Article
*   02/06/2025
*    4 contributors 

Feedback

Create an empty database project
--------------------------------

In the **Database Projects** view, select the **New Project** button and enter a project name in the text input that appears. In the "Select a Folder" dialog that appears, select a directory for the project's folder, `.sqlproj` file, and other contents to reside in. The empty project is opened and visible in the **Database Projects** view for editing.

[](https://learn.microsoft.com/en-us/azure-data-studio/extensions/sql-database-project-extension-getting-started?view=sql-server-ver16#open-an-existing-project)
Open an existing project
------------------------

In the **Database Projects** view, select the **Open Project** button and open an existing `.sqlproj` file from the file picker that appears. Existing projects can originate from Azure Data Studio, VS Code or [Visual Studio SQL Server Data Tools](https://learn.microsoft.com/en-us/sql/ssdt/sql-server-data-tools).

The existing project is opened and its contents are visible in the **Database Projects** view for editing.

[](https://learn.microsoft.com/en-us/azure-data-studio/extensions/sql-database-project-extension-getting-started?view=sql-server-ver16#create-a-database-project-from-an-existing-database)
Create a database project from an existing database
---------------------------------------------------

Instead of starting from an empty project, you can quickly populate a SQL Database Project with the existing objects from a database.

[](https://learn.microsoft.com/en-us/azure-data-studio/extensions/sql-database-project-extension-getting-started?view=sql-server-ver16#in-object-explorer)
### In Object Explorer

In the **Connections** view, connect to the SQL instance that contains the database to be extracted. Right-click on the database and select **Create Project from Database** from the context menu.

![Image 10: Screenshot of create Project from Database dialog.](https://learn.microsoft.com/en-us/azure-data-studio/extensions/media/sql-database-projects-extension/create-project-from-database.png)

The folder structure setting is set to _Schema/Object Type_ by default and offers different ways to automatically organize the existing objects when they are scripted out. The options for the folder structure setting are:

*   File: a single file is created for all the objects
*   Flat: a single folder is created for all the objects in individual files
*   Object Type: a folder is created per object type and each object is scripted out to a file
*   Schema: a folder is created per schema and each object is scripted out to a file
*   Schema/Object Type: a folder is created per schema and within the folder a folder is created per object type and each object is scripted out to a file

[](https://learn.microsoft.com/en-us/azure-data-studio/extensions/sql-database-project-extension-getting-started?view=sql-server-ver16#in-database-projects-view)
### In Database Projects view

In the **Project** view, select the **Import Project from Database** button and connect to a SQL instance. Once the connection is established, select a database from the list available databases and set the name of the project.

Finally, select a folder structure of the extraction. The new project is opened and contains SQL scripts for the contents of the selected database.

[](https://learn.microsoft.com/en-us/azure-data-studio/extensions/sql-database-project-extension-getting-started?view=sql-server-ver16#further-actions)
Further actions
---------------

[](https://learn.microsoft.com/en-us/azure-data-studio/extensions/sql-database-project-extension-getting-started?view=sql-server-ver16#build-and-publish)
### Build and publish

Deploying the database project is achieved in the SQL Database Projects extension by building the project into a [data-tier application file](https://learn.microsoft.com/en-us/sql/relational-databases/data-tier-applications/data-tier-applications) (dacpac) and publishing to a supported platform. For more on this process, see [Build and Publish a Project](https://learn.microsoft.com/en-us/azure-data-studio/extensions/sql-database-project-extension-build).

[](https://learn.microsoft.com/en-us/azure-data-studio/extensions/sql-database-project-extension-getting-started?view=sql-server-ver16#schema-compare)
### Schema compare

The SQL Database Projects extension interacts with the [Schema Compare extension](https://learn.microsoft.com/en-us/azure-data-studio/extensions/schema-compare-extension), if installed, to compare the contents of a project to a dacpac, existing database, or another project. The resulting schema comparison can be used to view and apply the differences from source to target.

![Image 11: Screenshot of schema compare dialog comparing a SQL project to a database.](https://learn.microsoft.com/en-us/azure-data-studio/extensions/media/sql-database-projects-extension/sql-project-schema-compare.png)

[](https://learn.microsoft.com/en-us/azure-data-studio/extensions/sql-database-project-extension-getting-started?view=sql-server-ver16#update-project-from-database)
### Update project from database

If changes are made to a database that are not yet made to the SQL project, the SQL project can be updated from the state of a database. This update is done by selecting **Update Project from Database** from the context menu of a database in the **Connections** view or from the context menu of a SQL project in the **Database Projects** view.

![Image 12: Screenshot of update Project from Database dialog.](https://learn.microsoft.com/en-us/azure-data-studio/extensions/media/sql-database-projects-extension/update-project-from-database.png)

[](https://learn.microsoft.com/en-us/azure-data-studio/extensions/sql-database-project-extension-getting-started?view=sql-server-ver16#next-steps)