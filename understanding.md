# Understanding

## How Data Resides

| `Catlog` (Varibles:{CatalogName, CatalogId} )
|:-
| `Category` (Varibles:{ *CatalogId* (Unique value differenct from parent), *CatageoryId*, *CatageoryName*} )
| `Category` (Varibles:{ *CatalogId* (Unique value differenct from parent), *CatageoryId*, *CatageoryName*} )

### Typically
```
Catlog (Varibles:{Catalog Name, CatalogId} )
 |- Category (Varibles:{CatalogId (Unique value differenct from parent), CatageoryId,CatageoryName} )
 |- Category (Varibles:{CatalogId (Unique value differenct from parent), CatageoryId,CatageoryName} )
Catlog
 |- Category (Varibles:{CatalogId (Unique value differenct from parent), CatageoryId,CatageoryName} )
 |- Category (Varibles:{CatalogId (Unique value differenct from parent), CatageoryId,CatageoryName} )
Catlog
 |- Category (Varibles:{CatalogId (Unique value differenct from parent), CatageoryId,CatageoryName} )
 |- Category (Varibles:{CatalogId (Unique value differenct from parent), CatageoryId,CatageoryName} )
```

## API understanding

`API - 1`
### SR_GetCatalogListDetails
* **input**: <Nothing>
* **output**: [**JSON**](../JSONS/SR_GetCatalogListDetails.json)

  Shows all the catalogs to choose

`API - 2`
### SR_GetServiceCatalogDetails
* **input**: **SearchKeyword** [ *can be any word, pharse in categeory* ]
* **output**: [**JSON**](../JSONS/SR_GetServiceCatalogDetails.json)

  AI model works here send any catalog name (truncated, pharse and small words also works)

`API - 3`
### SR_GetSelectedCatalogDetailsList
* **input**: **catalogId**
* **output**: [**JSON**](../JSONS/SR_GetSelectedCatalogDetailsList.json)
  
  `Custom Attributes` that needs to take input from user 
  * Dropdown lists
  * Input values
  * Count (Numbers)

`API - 4`
### SR_GetApprovals
* **input**: **catalogId**
* **output**: [**JSON**](../JSONS/SR_GetApprovals.json)

`API - 5`
### SR_LogServiceCatalog_Workflow
* **input**: 
  1. send all the `CustomAttributes` in `strCustomAttributes` or `strMvCustomAttributes` using `isMultivalued` : True or False

## Mulivalued : **False** 
[`strCustomAttributes`]

|SR_LogServiceCatalog_Workflow|SR_GetSelectedCatalogDetailsList       |
|:----------------------------|:--------------------------------------|
|`strCustomAttributes`        |`CustomAttributes`                  |
|Attribute_ID                 |CustomAttributes.SR_CtAttribute_ID     |
|Attribute_Name               |CustomAttributes.SR_CtAttribute_Name   |
|Attribute_Type               |CustomAttributes.SR_CtAttribute_DispalyModeText|
|Attribute_Value              |CustomAttributes.ListValues.Value      |
|||

## Mulivalued : **True** 
[`strMvCustomAttributes`]

|SR_LogServiceCatalog_Workflow|SR_GetSelectedCatalogDetailsList       |
|:----------------------------|:--------------------------------------|
|`strMvCustomAttributes`        |`CustomAttributes`                  |
|Attribute_ID                 |CustomAttributes.SR_CtAttribute_ID     |
|Attribute_Name               |CustomAttributes.SR_CtAttribute_Name   |
|Attribute_Type               |CustomAttributes.SR_CtAttribute_DispalyModeText|
|Attribute_Value              |CustomAttributes.ListValues.Value      |
|AttributeType                |CustomAttributes.SR_CtAttribute_DispalyModeText|
|IsActive                     |True                                   |
|RowID Ascending              |Number                                 |
|

2. send all Individual Approver in `Activities` from `SR_GetApprovals` into `ListSRApprovals`

|SR_LogServiceCatalog_Workflow. Request|Response Field Names              | Method Name   |
|:-------------------------------------|:---------------------------------|:--------------|
|WorkflowID                            |approvals.RID                     |SR_GetApprovals|
|AllApprover                           |Activities.ApproverID             |SR_GetApprovals|
|ApprovalLevel                         |Activities.ApprovalLevel          |SR_GetApprovals|
|ApproverID                            |Activities.ApproverID             |SR_GetApprovals|
|WorkflowActivityID                    |RID                               |SR_GetApprovals|
|ApproverRole                          |Activities.ApproverRole+“ Approval”|SR_GetApprovals|
|CurrentApprover                       |false                             |Hardcoded|
|LevelName                             |Activities.Title                  |SR_GetApprovals|
|OrgID                                 | 1                                |Hardcoded|
|ReturnType                            |JSON                              |Hardcoded|
|LogSRForUserMode                      |false                             |Hardcoded|
|DelegationMode                        |false                             |Hardcoded|
|DelegateeUserID                       |Summit User ID for User or 0      |Get user id from ADM_SearchAll User By user email id|
|ServiceCatalogID                      |CatalogID                         |SR_GetSelectedCatalogDetailsList|
|HasCompleteApprovalFlow               |true                              |Hardcoded|
|ServiceCategoryName                   |CategoryName                      |SR_GetSelectedCatalogDetailsList|
|SupportFunction                       |SupportFunction                   |SR_GetSelectedCatalogDetailsList|
|ServiceCategoryID                     |CategoryID                        |SR_GetSelectedCatalogDetailsList|
|ServiceCatalogName                    |CatalogName                       |SR_GetSelectedCatalogDetailsList|
|WorkGroupID                           |Workgroupid                       |SR_GetSelectedCatalogDetailsList|
|JustificationRemarks                  |Description (Free text)           |SR_GetSelectedCatalogDetailsList|
|


* **output**: [**JSON**](../JSONS)