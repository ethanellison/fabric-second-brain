**Video Summary:**

This video discusses security and governance in Microsoft Fabric, specifically focusing on levels of administration, access control, and dynamic data masking. It covers tenant-level, capacity-level, workspace-level, item-level, object/file-level, and row/column-level administration. The presenter elaborates on the nuances of each level, explains workspace access control, the principle of least privilege, and outlines different roles like admin, member, contributor, and viewer, and their corresponding permissions. The video also introduces dynamic data masking and underscores that this should never be implemented as a security feature, but always in addition to RLS & CLS. Finally, it touches on data governance features like sensitivity labeling and item endorsement.

**Detailed Explanation of Each Topic:**

- **Levels of administration in Fabric:**
    
    - Fabric allows administration at multiple levels.
        
    - The highest level is the **tenant-level**.
        
    - Below the tenant are **capacities**, and a tenant can have one or more capacities.
        
    - Capacities are linked to one or more **workspaces**.
        
    - Within each workspace, you have **items** such as data warehouses, lakehouses, and semantic models.
        
    - Data warehouses and lakehouses support more granular access controls, including **object/file-level, row-level, and column-level** access.
        
- **Workspace-level access control:**
    
    - Workspace-level access control allows granting access to everything within a workspace to a user or a group (M365 or Entra ID security group).
        
    - Access is controlled by assigning a workspace-level role: Admin, Member, Contributor, or Viewer.
        
    - Each role applies to all items in the workspace.
        
    - Administrators have the highest level of control and can do almost anything.
        
    - Members have nearly all capabilities as Admins except the capability to add admins and delete the workspace.
        
    - Contributors can write or delete data pipelines, notebooks, Spark job definitions, ML models, and event streams.
        
    - Viewers can only view data, as opposed to modify or create new items.
        
- **Item-level access control:**
    
    - Provides a more granular approach, where access can be granted to individual Data Warehouses or Lakehouses instead of an entire Workspace.
        
    - When using item-level access control, the user cannot see the Workspace in Fabric.
        
    - They can only access the item that has been explicitly shared.
        
- **Dynamic Data Masking (DDM):**
    
    - Dynamic data masking changes the way data is presented to the user in query results, while the underlying data remains unchanged.
        
    - If you share a Data Warehouse or Lakehouse with ReadData permission, the user will be able to see all tables regardless of the OLS setting.
        
    - DDM is not recommended as a security feature. Use RLS and CLS in conjunction with DDM.
        
- **Data Governance Features**
    
    - **Sensitivity Labelling:** A data governance feature for information protection where labels need to be managed in Microsoft Purview before they become accessible in Fabric.
        
    - **Endorsements:** Used to endorse quality content to help colleagues discover it. There are different levels or badges of endorsements that we can assign to items in Fabric. These include promoted, certified, and master data
        

**Step-by-Step UI Tutorials:**

**How to Grant Workspace-Level Access Control:**

1. Within your Fabric Workspace, click the **Manage access** button at the top right.
    
2. In the "Add people" window, start typing the user or group name in the **Enter a name or email address** field.
    
3. Select the desired user or group from the suggestions.
    
4. Choose the appropriate role for the user or group: **Viewer, Contributor, Member, or Admin**.
    
5. Click the **Add** button.
    

**How to Share an Item:**

1. Navigate to the item (e.g., Data Warehouse, Lakehouse) that you want to share.
    
2. Right-click on the item in the workspace list.
    
3. Select **Share** from the context menu.
    
4. In the "Grant people access" window, enter the name or email address of the user or group.
    
5. Choose the desired additional permissions such as: **Read all data using SQL (ReadData), Read all OneLake data (ReadALL), Build reports on the default semantic model (Build)**
    
6. Click the **Grant access** button to finalize sharing.
    

**DP-700 Exam-Style Questions & Answers:**

<details>  
<summary>Question 1: Which of the following is NOT a level of administration in Microsoft Fabric?</summary>  
<br>  
A) Tenant-level  
B) Capacity-level  
C) Item-level  
D) Region-level  

<br>  
Answer: D) Region-level  
<br>  
Explanation: The video explains different administration levels in Fabric, including tenant-level, capacity-level, item-level, and more. There is no mention of a "Region-level" administration, making it the correct answer.  
</details>  

<details>  
<summary>Question 2: What action is a 'Member' role in a Fabric workspace NOT authorized to perform?</summary>  
<br>  
A) Create Data Pipelines  
B) Delete the workspace  
C) View Output of ML Models  
D) Execute notebooks  
<br>  
Answer: B) Delete the workspace  
<br>  
Explanation: According to the video, Members can do nearly everything except Add Admins or delete the workspace. The ability to create data pipelines, view output of ML models, and execute notebooks are all granted to Members.  
</details>  

<details>  
<summary>Question 3: Dynamic data masking, by itself, offers robust security against unauthorized access to sensitive data.</summary>  
<br>  
A) True  
B) False  
C) Only if using the Email masking option.  
D) Only if using Random Masking option.  
<br>  
Answer: B) False  
<br>  
Explanation: The presenter underscores that dynamic data masking SHOULD NOT be implemented as a security feature. It is for information protection only, not security.  
</details>  

<details>  
<summary>Question 4: A data engineer needs to grant a user access to a single table within a Fabric Data Warehouse, but not to the entire warehouse. Which method should the engineer use?</summary>  
<br>  
A) Grant the user workspace-level access.  
B) Grant the user item-level access.  
C) Grant the user file-level access.  
D) Use Dynamic Data Masking on all sensitive columns.  
<br>  
Answer: B) Grant the user item-level access.  
<br>  
Explanation: Item-level access provides the granularity to grant access to specific items like a data warehouse table and not the workspace which would automatically grant the user access to ALL items in the workspace.  
</details>  

<details>  
<summary>Question 5: Where does the presenter state labels need to be created and managed before they can be accessible in Fabric?</summary>  
<br>  
A) In the OneLake Data Catalog  
B) Within Fabric Workspaces  
C) Microsoft Purview  
D) Microsoft Entra ID  
<br>  
Answer: C) Microsoft Purview  
<br>  
Explanation: The presenter specifically mentions sensitivity labels are created and managed in Microsoft Purview, a centralized governance solution from Microsoft.  
</details>  

<details>  
<summary>Question 6: A Fabric Administrator wants to promote a certified Lakehouse for sharing. Which endorsement level should they assign?</summary>  
<br>  
A) None  
B) Promoted  
C) Certified  
D) Master data  
<br>  
Answer: B) Promoted  
<br>  
Explanation: The presenter defines Promoted endorsement as “is ready for sharing and reuse.” Certified and Master Data have more restrictive permissions and definitions, so only users specified by the Fabric administrator can label data items as Master Data and any user can request certification for an item, but only users specified by the Fabric administrator can actually certify items, implying certified is not meant for all users to simply share data.  
</details>  

<details>  
<summary>Question 7: A data engineer needs to ensure users can query a Data Warehouse using Spark and restrict access to specific columns for various users. Which configuration steps are needed to achieve this level of security?</summary>  
<br>  
A) Implement object-level security using GRANT SELECT statements.  
B) Implement row-level security using CREATE FUNCTION and SECURITY POLICY  
C) Implement column-level security using GRANT SELECT [cols] statements  
D) The engineer can't restrict column level security using spark endpoints because column and row level security are not currently supported in the Spark endpoint of the Lakehouse.  

<br>  
Answer: D) The engineer can't restrict column level security using spark endpoints because column and row level security are not currently supported in the Spark endpoint of the Lakehouse.  
<br>  
Explanation: The grid table showed in the video from minute 11:11 - 11:27 indicates OLS/CLS cannot be enabled in Spark.  
</details>