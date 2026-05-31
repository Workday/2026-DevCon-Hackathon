# Concept: ASOR Agent Resource Search API

You can use the asor/agentResourceSearch endpoint (secured to the Setup: Agents domain in the Agent System of Record functional area) to find the WIDs of public Workday APIs to use with your agent tools.

## Query Parameters

The endpoint supports these query parameters:

* searchString:
  * You can provide a string for this query parameter that searches through the list of public operations that contain that string. Example: if you enter "Location", then we return all operations related to "Location", such as **Put Inventory Storage Location**.
* operationType:
  * This query parameter is a string that searches by operation type. It supports these values: Create, Patch, View. For SOAP operations, it maps Patch to Put and View to Get.
* toolType:
  * This query parameter is a string that searches by tool type. It supports these values: SOAP, REST, MCP.

## Sample Response:
```
`{  "type": "SOAP","name": "Put Inventory Storage Location",
     "description": "This service operation creates the Inventory Storage Location.",
     "wid": "2c6143cd33da10000d22312205450221", 
     "mcpEnabled": false,
     "httpEnabled": true
      {    "type": "SOAP",
            "name": "Put Campus Location Decision Policy",
            "wid": "c1941a9b6f071000142d9832a9865172",
            "mcpEnabled": false, 
            "httpEnabled": true  }, 
                                               } `
```