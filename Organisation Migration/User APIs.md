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
                        i. /api/v1/organization/users/:id/?recipe=
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
                a. User Minimal Details:
                    - umd
                    - Details, roles, MFA Status, SSO info
                b. UGs
                    - gd
                    - User group details
                c. RCs
                    - rcd
                    - Responsibility center details
                d. Modules
                    - ms
                    - stats: { compliance : {}, policy: {} }
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
        - role: Should exist, should be active
        
    - Update user entity
    - Role calculation
    - Queue up for background tasks
    - Serialize and send response

* api/services/OrganisationService.js
--
Worker method:
- processUserUpdate(_payload):
    - Validation:
        - Ensure user created
    - User -> Role mapping (user_role_map insertion)
    - UTC offset calculation (Logic required)
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

Get users:
-----

Endpoint: GET api/v1/organization/users

Files and logic breakdown:
----

* Organization/UserController.js
--
Export method:
- getUsers(req, res):

Logical method:
- getUsers(user, data):
    - Based on the user fetch the paginated user details
    - Response: Users + Pagination details
    - Serialize and send response

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
},
{
    "id": 3795,
    "uid": "ID1YB8A6ASGB",
    "uuid": "ID1YB8A6ASGB",
    "name": "User 1",
    "email": "user@test1.com",
    "jobTitle": "",
    "licenseType": "light",
    "imagePath": "",
    "status": "active",
    "lastPasswordUpdatedAt": "2026-04-29T09:30:00Z"
} ^JMIRYhJ6

Get user details:
-----

Endpoint: GET api/v1/organization/users/:id/?recipe=

Supported Recipes:
    a. User Minimal Details:
        - umd
        - Details, roles, MFA Status, SSO info
    b. UGs
        - gd
        - User group details
    c. RCs
        - rcd
        - Responsibility center details
    d. Modules
        - ms
        - stats: { compliance : {}, policy: {} }

Files and logic breakdown:
----

* Organization/UserController.js
--
Export method:
- getUserDetails(req, res):
    - If userId query param missing early exit
    - If user not found, then early exit

Logical method:
- getUserDetails(user, data):
    - Based on the recipe, call the helper function
    - User minimal details:
        - getUserMinimalDetails(user)
    - User groups:
        - getUserGroupDetails(user)
    - Responsibility centers:
        - getResponsibilityCenterDetails(user)
    - Modules:
        - getModuleStats(user)
- getUserMinimalDetails:
    - User details (users)
    - Roles (roles)
    - MFA Status, SSO Info
- getUserGroupDetails:
    - User groups (ids) where user is associated with (user_group_map)
- getResponsibilityCenterDetails
    - Responsibility centers where user is associated with (user_responsibility_center_map)
- getModuleStats
    - Reuse the logic from Organization Internal APIs

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
    "lastPasswordUpdatedAt": "2026-04-29T09:30:00Z",
    "userGroups": [...ids],
    "roles": [...ids],
    "responsibilityCenters": [...ids],
    "stats": { compliance : {}, policy: {} }
}, ^gzeWqYTL

Detailed Tech Specs
------------------- ^hLIbabT1

%%
## Drawing
```compressed-json
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=
```
%%