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