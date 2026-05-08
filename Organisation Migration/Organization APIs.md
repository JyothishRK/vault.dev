---

excalidraw-plugin: parsed
tags: [excalidraw]

---
==⚠  Switch to EXCALIDRAW VIEW in the MORE OPTIONS menu of this document. ⚠== You can decompress Drawing data with the command palette: 'Decompress current Excalidraw file'. For more info check in plugin settings under 'Saving'


# Excalidraw Data

## Text Elements
Organization APIs
----------------- ^gDPaE6kC

Create Users
---------------

 ^xaCPnjgx

Edit Users
---------------

 ^s9hogVNK

Get All Users
---------------

 ^ztJw8FZ2

Get User Details
---------------


 ^5B7DStiZ

API Responses ^4A69wcx8

Organization Update
---------------

1. Request 
        a. URL 
                i. /api/v1/organization/
        b. method
                i. PUT
        c. headers
                i. token
        d. body 
                i. name
                ii. timezone
                iii. imagePath
2. Authentication
        a. token check 
        b. org must be active
3. Authorization
        a. Root user
4. Validations
5. Actions
        a. Update Organization entry
        b. Serialize
        c. Send Response
6. Bg action
        a. utc_offset calculation.
        b. Activity Log for organization update in organization_activity_logs
7. Response
 ^oOsMQPyP

Get Organization Details
---------------

1. Request 
        a. URL 
                i. /api/v1/organization/:id
        b. method
                i. GET
        c. headers
                i. token
2. Authentication
        a. token check 
        b. org must be active
3. Authorization
        a. Root user
4. Validations
5. Actions
        a. Fetch Organization details
        b. Serialize
        c. Send Response
6. Response
 ^oZH3JHsL

Organization Update
-----

Endpoint: PUT api/v1/organization/

Files and logic breakdown:
----

* Organization/OrganizationController.js
--
Export method:
- async updateOrganization(req, res):

Logical method:
- updateOrganization(user, data):
    - payload = validateUpdateOrganization(user, data)
        - Organization name - Non empty
    - Update organization entry
    - Queue for background tasks
    - Serialize and send response

* api/services/OrganizationService.js
--
Worker method:
- processOrganizationUpdate(_payload):
    - Validation:
        - Ensure Organization exists or not
    - UTC offset calculation
        - Usage of the new DateService function
    - Activity log insertion


Response structure:
----

{
    "id": 3794,
    "uid": "IDCCS4T028ND",
    "uuid": "IDCCS4T028ND",
    "name": "Key Admin",
    "email": "admin@triggertesting.com",
    "jobTitle": null,
    "licenseType": "power",
    "imagePath": "",
    "status": "active",
    "lastPasswordUpdatedAt": "2026-04-29T09:30:00Z"
} ^O9wrSSMf

User create:
-----

Endpoint: POST api/v1/organization/users

Files and logic breakdown:
----

* Organization/UserController.js
--
Export method:
- createUser(req, res):

Logical method:
- createUser(user, data):
    - Validation
        - name: Required
        - email: Required, valid
        - role: Should exist, should be active
        
    - Create user entity
    - Role calculation
    - Queue up for background tasks
    - Serialize and send response

* api/services/UserService.js
--
Worker method:
- processUserCreate(_payload):
    - Validation:
        - Ensure user created
    - uuid generation & insertion (Logic required)
    - User -> Role mapping (user_role_map insertion)
    - UTC offset calculation (Logic required)
    - Password generation (Logic required) - Mark as TO-DO
    - User Onboarding email - Mark as TO-DO
    - Activity log insertion


Response structure:
----

{
    "id": 3794,
    "uid": "IDCCS4T028ND",
    "uuid": "IDCCS4T028ND",
    "name": "Key Admin",
    "email": "admin@triggertesting.com",
    "jobTitle": null,
    "licenseType": "power",
    "imagePath": "",
    "status": "active",
    "lastPasswordUpdatedAt": "2026-04-29T09:30:00Z"
} ^r60D8zva

%%
## Drawing
```compressed-json
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=
```
%%