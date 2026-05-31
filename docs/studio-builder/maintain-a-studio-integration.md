The Workday Developer Site treats Studio integrations as integration apps. Within the integration app, each Studio integration also belongs to an artefact called a collection.

1. On the Workday Developer Site home page, click **Build an Integration App**, then click **Start from Scratch**.
2. Provide a **Name** and, optionally, a **Description**. The **Reference ID** field populates automatically but is editable.
3. Click **Create and Go to Overview** to display your app's home page.
4. Under the **Studio Collections** heading, click **Maintain a Studio Collection**.
5. Select one of the options for opening an existing collection or create a new one:

   |**Option**|**Notes**|
   |---|---|
   |**Load from a Tenant**|Select the tenant where the integration has been deployed, and click **Sign In**.<br><br>|
   |**Import a .clar file from Disk**|Drop the .clar file onto the upload panel or click **Select a File** and browse for it.<br><br>The collection's existing **Name** and **Description** are imported and a **Reference ID** is automatically created. You can edit these fields.<br><br>Click **Create and Edit**.<br><br>|
   |**Import Project(s) from Disk**|Drop the project .zip file onto the upload panel or click **Browse .zip File** and browse for it. Alternatively, click **Browse Folder** and select the project folder. <br><br>The collection's existing **Name** is imported and a **Reference ID** is automatically created. You can edit these fields.<br><br>Click **Create and Edit**.<br><br>|
   |**Start from Scratch**|Provide a **Name** for the new collection and, optionally, a **Description**.<br><br>Click **Create**.<br><br>
6. Studio Builder opens and displays the assembly diagram for the imported project. Click **Semantic View** to quickly filter and navigate directly to components on the diagram, instantly bringing selected components into focus. For more information, see: [The Studio Builder Interface](/docs/studio-builder/the-studio-builder-interface.md).<br><br>In terms of assembly editing, Studio Builder behaves much like Workday Studio:
     * Click a component to open its properties panel.
     * Add new components by dragging them into the assembly from the **Components** panel.
     * Connect components by dragging the connector line from the output of one to the input of the other.
     * Arrange components in swimlanes for clarity. Each swimlane is collapsible.
     * Use the buttons at the top of the assembly panel to align the assembly diagram and to expand or collapse all swimlanes.
7. Click **Validate** to verify the XML underlying your assembly.
8. Use the save button at the top of the assembly panel to save updates to the `assembly.xml` file. Click **Save to App Hub** to save the integration app in its entirety to App Hub.
9. To deploy the integration, click **Sign in to Tenant**, select a tenant, and provide credentials. Then click **Deploy**.