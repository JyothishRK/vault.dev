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
        -
    - Responsibility centers:
    - Modules:

Response structure:
---- ^gzeWqYTL

%%
## Drawing
```compressed-json
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
```
%%