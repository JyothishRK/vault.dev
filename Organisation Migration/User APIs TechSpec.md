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
} ^JMIRYhJ6

%%
## Drawing
```compressed-json
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
```
%%