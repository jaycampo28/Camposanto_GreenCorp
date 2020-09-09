# Camposanto_GreenCorp

1) Brand the Service Portal to your liking
Dev Notes:
- This is the Service Portal for a fictitious company called Green Corp, a seller of futuristic trees called Super Trees.
- Updated the \sp with new background and logo
- Sources: 
  logo - https://www.greenamerica.org/
  background - https://wallpaperscraft.com/download/trees_aerial_view_forest_129167/938x1668
  
2) Create a new Incident Record Producer for an end user and ensure the Service Portal links to it from the homepage
Dev Notes:
- The record producer created is called Technical Issue
- Routing of the assignment group is scripted based on the variable, Type of issue (hardware vs software)
- Category, Short description and Description are set accordingly
- An icon link widget in the Service Portal homepage links to this record producer

3) Create a new Service Catalog item for a device of your choosing that requires Manager approval and generates two Catalog Tasks after approval is received. 
Dev Notes:
- Order Electric Vehicle catalog item has been created and is available for Green Corp employees from the Electric Vehicle category (how lucky are they?)
- A simple workflow is attached that sends a RITM approval request to the Request for's manager
- Once approved, two SCTASKs are created (Order Electric Vehicle Task and Deliver Electric Vehicle Task)

4) Publish 3 Knowledge base Articles and ensure these can be browsed to in the Service Portal. 
Dev Notes:
The following KB articles have been created, published, and approved. These all belong to the IT Knowledge Base are found under Applications > Super Trees category.
- KB article 1 Super Trees Breeding (source: https://www.csmonitor.com/1984/0703/070304.html) 
- KB article 2 New Super Trees (source: cnn.com) - this was created using the Article import tool
- KB artice 3 Future of Super Trees

5) Create a new Task type table and form showing at least the following fields: Number, Business Service, Configuration Item, Change Request (Reference to the change_request table), Knowledge Checkbox, State (Open, In Progress, Complete), Impact, Assignment Group, Assigned to, Short Description and Description
On Change of the Change Request field, append the referenced Change Request record’s Short Description to the current form’s Description field.
Restrict the Configuration Item field to only pull in Application CIs
Hide the Impact field for non admin users
On change of Impact to 1 pull back the following Configuration Item attributes and append to the Description field: Name, Operational status, Owned by, Updated and Update by 
Dev Notes:
- Custom table, Super Tree [u_super_tree] has been extended from the Task table since the most of fields needed already exist
- A custom reference field, Change request [u_change_request] was added with reference to the Change Request [change request] table
- The State [state] field choices were updated to include the custom state choices
- The Configuration Item [cmdb_ci] field has been overriden such that the reference qualifier is only pointing to Application CIs
- The Impact field is displayed/hidden based on the logged in user (see the Hide Impact for non-admins OnLoad client script)
- The Description field on the Super Tree form is being set by the referenced Change Request record's Short Description using GlideAjax (see the Set Change request to Short description OnChange client script that calls the getChangeRequest method on the SuperTreeUtilsAjax script include)
- The Description field onthe Super Tree form is being set by the Configuration Item's Name, Operational status, Owned by, Updated, and Updated by fields when the Impact is set to 1 using GlideAjax (see the Set CI info to Description OnChange client script that calls the getConfigItemAttributes method on the SuperTreeUtilsAjax script include)
- Note that the custom role u_super_tree_user was added to the Hardware group for demo purposes

6) (Advanced Candidates) Understanding of APIs
Utilize an external tool to utilize the Incident Management REST API and pull back the details of an Incident record looked up by Incident Number
Utilize an external tool to utilize the Incident Management REST API to Create an Incident record
Create a spreadsheet with at least 10 rows and the following columns: Name, Version and Operational Status. Import to the Application CI table.
Dev Notes:
- Postman was used to make a GET call (e.g. https://<instance>.service-now.com/api/now/table/incident?sysparm_limit=1&sysparm_number=INC0000060) to retrieve an incident. Here's a snippet of the response body:
  {
    "result": [
        {
            "parent": "",
            "made_sla": "true",
            "caused_by": "",
            "watch_list": "",
            "upon_reject": "cancel",
            "sys_updated_on": "2016-12-14 02:46:44",
            "child_incidents": "0",
            "hold_reason": "",
            "approval_history": "",
            "number": "INC0000060",
            "resolved_by":
- Postman was used to make a POST call (e.g. https://<instance>.service-now.com/api/now/table/incident) to generate an incident on the instance. Verified that the incident (INC0010010) was successfully created on the instance. Mandatory fields were passed as part of the request body. Here's the request body:
  {
    "caller_id":"62826bf03710200044e0bfc8bcbe5df1",
    "short_description":"test incident short description",
    "description":"test incident description"
}
  Here's a snippet of the response body:
  {
    "result": {
        "parent": "",
        "made_sla": "true",
        "caused_by": "",
        "watch_list": "",
        "upon_reject": "cancel",
        "sys_updated_on": "2020-09-09 03:04:07",
        "child_incidents": "0",
        "hold_reason": "",
        "approval_history": "",
        "number": "INC0010010",
        "resolved_by": "",
        "sys_updated_by": "admin",
        "opened_by": {
- To import Application CI records, an Import set table, Application CI Import Set was created. It was then transformed using Application CI Transform Map. See imported records App1 to App10 on the Application CI table.
