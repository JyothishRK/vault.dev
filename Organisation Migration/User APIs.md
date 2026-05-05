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
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=
```
%%