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

Organization Profile
-----

Endpoint: PUT api/v1/organization/users/profile

Files and logic breakdown:
----

* Organization/OrganizationController.js
--
Export method:
- async updateOrganization(req, res):

Logical method:
- getOrganizationProfile(orgId):
    - No changes required in existing logic
--

* 


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
} ^aD6j4OBS

%%
## Drawing
```compressed-json
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
```
%%