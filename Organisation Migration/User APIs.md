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

MIAA
```
%%