---

excalidraw-plugin: parsed
tags: [excalidraw]

---
==⚠  Switch to EXCALIDRAW VIEW in the MORE OPTIONS menu of this document. ⚠== You can decompress Drawing data with the command palette: 'Decompress current Excalidraw file'. For more info check in plugin settings under 'Saving'


# Excalidraw Data

## Text Elements
Create Users
---------------

        1. Request 
                a. URL 
                        i. /api/v1/organization/users/
                b. method
                        i. POST
                c. headers
                        i. token
                d. body 
                        i. name
                        ii. email
                        iii. jobTitle
                        iv. timezone
                        v. roles
        2. Authentication
                a. token check 
                b. org must be active
        3. Authorization
                a. root/admin user/
                b. power user 
                        i. Permission: manage Users
        4. Validations
                a. name
                        i. Required
                b. email
                        i. Required
                        ii. Unique
                        iii. Valid email address
                c. roles
                        i. must exist
                        ii. Roles must be active
        5. Actions
                a. create User entry
                b. Serialize 
                c. Send Response
        6. Bg action
                a. License_type calculation
                b. utc_offset calculation.
                c. Password generation
                d. User - Role mapping in User_Role_Map for all roles linked
                e. Onboarding email to user
                f. Activity Log for user crete in organization_activity_logs
        7. Response
 ^9Zd9IFvu

Edit Users
---------------

        1. Request 
                a. URL 
                        i. /api/v1/organization/users/
                b. method
                        i. PUT
                c. headers
                        i. token
                d. body 
                        i. name
                        ii. job_title
                        iii. timezone
                        iv. roles
        2. Authentication
                a. token check 
                b. org must be active
        3. Authorization
                a. root/admin user/
                b. power user 
                        i. Permission: manage Users
        4. Validations
                a. name
                        i. Required
                b. roles
                        i. must exist
                        ii. Roles must be active
        5. Actions
                a. Update User entry
                b. Serialize 
                c. Send Response
        6. Bg action
                a. License_type calculation
                b. utc_offset calculation.
                c. UserRole Mapping updation for all added and removed roles.
                d. Activity Log for user edit in organization_activity_logs
                        i. extras: values before update?
        7. Response
 ^qfRDYK6W

Get All Users
---------------

        1. Request 
                a. URL 
                        i. /api/v1/organization/users/
                b. method
                        i. GET
                c. headers
                        i. token
        2. Authentication
                a. token check 
                b. org must be active
        3. Authorization
                a. root/admin user/
                b. power user 
                        i. Permission: manage Users
        4. Actions
                a. Fetch user details
                b. Serialize 
                c. Send Response
        5. Response
 ^igtv1aEp


Get User Details
---------------

        1. Request 
                a. URL 
                        i. /api/v1/organization/users/?recipe=
                b. method
                        i. GET
                c. headers
                        i. token
        2. Authentication
                a. token check 
                b. org must be active
        3. Authorization
                a. root/admin user/
                b. power user 
                        i. Permission: manage Users
        4. Supported Recipes:
                a. Details
                b. Roles
                c. UGs
                d. RCs
                e. Modules
        5. Actions
                a. Fetch user details based on recipe
                b. Serialize 
                c. Send Response
        6. Response
 ^QlPP5UhG

User APIs ^wxrAU9IV

Create Users
---------------

 ^K2qp6fkU

Edit Users
---------------

 ^43R93xwA

Get All Users
---------------

 ^1ntTcqvx

Get User Details
---------------


 ^H6YOkLob

API Responses ^1d3NeOtR

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

* api/services/OrganisationService.js
--
Worker method:
- processUserCreate(_payload):
    - Validation:
        - Ensure user created
    - uuid generation & insertion (Logic required)
    - User -> Role mapping (user_role_map insertion)
    - UTC offset calculation (Logic required)
    - Password generation (Logic required)
    - User Onboarding email - Put as TO-DO
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
} ^dIOn9gDn

User update:
-----

Endpoint: PUT api/v1/organization/users

Files and logic breakdown:
----

* Organization/UserController.js
--
Export method:
- updateUser(req, res):
    - Check if user exists

Logical method:
- updateUser(user, data):
    - Validation
        - name: Required
        - email: Required, valid
        - role: Should exist, should be active
        
    - Create user entity
    - Role calculation
    - Queue up for background tasks
    - Serialize and send response

* api/services/OrganisationService.js
--
Worker method:
- processUserCreate(_payload):
    - Validation:
        - Ensure user created
    - uuid generation & insertion (Logic required)
    - User -> Role mapping (user_role_map insertion)
    - UTC offset calculation (Logic required)
    - Password generation (Logic required)
    - User Onboarding email - Put as TO-DO
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
} ^oYF4RNiO

%%
## Drawing
```compressed-json
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
```
%%