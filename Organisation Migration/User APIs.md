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
}


Stats structure:
----

{
  "stats": {
    "compliance": {
      "programsManagedCount": "0",
      "programsCollaboratedCount": "0",
      "responsibilitiesAssignedCount": "5",
      "responsibilitiesEntrustedByCount": "0"
    },
    "policy": {
      "policiesAuthoredCount": "4",
      "policiesAssignedForApprovalCount": "17",
      "policiesAllocatedToCount": "11"
    }
  }
} ^gzeWqYTL

Detailed Tech Specs
------------------- ^hLIbabT1

utcOffset Calculation:
-----

getUtcOffsetInMinutes(memberDetails.timezone) <- Function in legacy

Here timezone is stored like this: America/Los_Angeles, Europe/London

Business Logic (legacy):
---

function getUtcOffsetInMinutes(timeZone) {
  if(!timeZone) {
    return 0;
  }
  const now = new Date();

  // Convert the current date to the target time zone
  const localeTime = new Intl.DateTimeFormat("en-US", {
    timeZone,
    hour12: false,
    hour: "2-digit",
    minute: "2-digit",
    second: "2-digit",
  }).format(now);

  const [hour, minute, second] = localeTime.split(":").map(Number);

  // Get the same time in UTC
  const utcHour = now.getUTCHours();
  const utcMinute = now.getUTCMinutes();
  const utcSecond = now.getUTCSeconds();

  // Create "pseudo" Date objects for both and subtract to find the offset
  const targetDate = new Date(1970, 0, 1, hour, minute, second);
  const utcDate = new Date(1970, 0, 1, utcHour, utcMinute, utcSecond);

  // Calculate offset in minutes
  const offsetMinutes = (targetDate - utcDate) / (1000 * 60);
  return offsetMinutes;
}


Optimized Approach:
---

function getUtcOffsetInMinutes(timeZone) {
  const now = new Date();

  const tzDate = new Date(
    now.toLocaleString("en-US", { timeZone })
  );

  return (tzDate - now) / (1000 * 60);
}


 ^tlZFORVO

%%
## Drawing
```compressed-json
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
```
%%