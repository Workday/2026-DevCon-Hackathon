# Workday Admin

The Workday Admin is responsible for the functional setup and maintenance of the wizard within the tenant.

* **Access & Setup**: Use the **Configure ConnectR** task to add Extend tasks as sections to a wizard.
* **Change Detection step**: If the underlying wizard definition has changed since the last save (sections added or removed by Workday), the admin is shown a diff of what was added/removed before proceeding.
* **Edit step**: A re-orderable grid showing all wizard sections (Workday-delivered + custom). The admin can:
  * Add custom Extend task sections (up to 5).
  * Give each section a display label (with translation support).
  * Reorder sections (within the constraints described in Special Considerations).
*  **Task Review tab** : A separate grid for configuring the Review & Submit step sections.

The admin configuration is **tenant-specific** and is **OX-enabled**, meaning it can be promoted from one tenant to another via Object Transfer.

The admin can also **Reset to Default** at any time from the **Maintain ConnectR** page, which removes all custom configuration and returns the wizard to the Workday-delivered baseline.

## Extend Developer

Builds the Extend edit task that will be embedded as a section.

## End to End Flow
**Admin**: Run "Configure ConnectR" task  
       └── Select a Workday-delivered wizard (e.g. Change Job)  
           └── Add one or more Extend task sections  
               └── Set section label, ordering, and context (optional)  

**Runtime**:  
  User launches wizard (e.g. Start Job Change)  
  └── Wizard renders each section in order  
      ├── Workday-delivered sections render as normal  
      └── Extend sections render inline via Extend's edit URI  
          └── On "Submit": Task Wizard calls Extend's validate API for all Extend sections  
              ├── Errors are surfaced in the unified error widget and section icons  
              └── On success: Extend sections appear in the Review step as view-only  

**Developer**: Build Extend task  
- Create an edit page for the task (example: page for collecting related data for Change Job).  
-- Create a task with the associated edit page.  
---- Deploy the app to the tenant.  