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

{
    "success": true,
    "statusCode": 0,
    "statusMessage": "User created successfully",
    "data": {
        "user": {
            "id": 3,
            "uid": "GCHT8MG4JI",
            "uuid": "8TUNNY51WHF3",
            "name": "Light User",
            "email": "user@light.com",
            "jobTitle": "Officer",
            "licenseType": "light",
            "imagePath": null,
            "status": "active",
            "passwordUpdatedAt": null
        }
    }
} ^K2qp6fkU

Edit Users
---------------

{
    "success": true,
    "statusCode": 0,
    "statusMessage": "User updated successfully",
    "data": {
        "user": {
            "id": 3,
            "uid": "GCHT8MG4JI",
            "uuid": "8TUNNY51WHF3",
            "name": "Light [User]",
            "email": "user@light.com",
            "jobTitle": "Officer",
            "licenseType": "light",
            "imagePath": null,
            "status": "active",
            "passwordUpdatedAt": null
        }
    }
} ^43R93xwA

Get All Users
---------------

{
    "success": true,
    "statusCode": 0,
    "statusMessage": "Users fetched successfully",
    "data": {
        "users": [
            {
                "id": 1,
                "uid": "IDP5VB5NDHHT",
                "uuid": "IDP5VB5NDHHT",
                "name": "Priya Nair",
                "email": "priya.nair@vertexlabs.ai",
                "jobTitle": "Officer",
                "licenseType": "power",
                "imagePath": null,
                "status": "active",
                "passwordUpdatedAt": null
            },
            {
                "id": 2,
                "uid": "ZU0GJIOKI8",
                "uuid": "UH8SFR3G9EUM",
                "name": "R K Jyothish",
                "email": "jo@test.com",
                "jobTitle": "Intern",
                "licenseType": "power",
                "imagePath": null,
                "status": "active",
                "passwordUpdatedAt": null
            },
            {
                "id": 3,
                "uid": "GCHT8MG4JI",
                "uuid": "8TUNNY51WHF3",
                "name": "Light [User]",
                "email": "user@light.com",
                "jobTitle": "Officer",
                "licenseType": "light",
                "imagePath": null,
                "status": "active",
                "passwordUpdatedAt": null
            },
            {
                "id": 4,
                "uid": "P67P1VG78O",
                "uuid": "YOOZRCIRFEPX",
                "name": "Om SAI RAM",
                "email": "sai@gmail.com",
                "jobTitle": "God",
                "licenseType": "power",
                "imagePath": null,
                "status": "active",
                "passwordUpdatedAt": null
            }
        ],
        "page": 1,
        "limit": 30,
        "total": 4
    }
} ^1ntTcqvx

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
    "success": true,
    "statusCode": 0,
    "statusMessage": "User created successfully",
    "data": {
        "user": {
            "id": 3,
            "uid": "GCHT8MG4JI",
            "uuid": "8TUNNY51WHF3",
            "name": "Light User",
            "email": "user@light.com",
            "jobTitle": "Officer",
            "licenseType": "light",
            "imagePath": null,
            "status": "active",
            "passwordUpdatedAt": null
        }
    }
} ^dIOn9gDn

User update:
-----

Endpoint: PUT api/v1/organization/users/:id

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
    "success": true,
    "statusCode": 0,
    "statusMessage": "User updated successfully",
    "data": {
        "user": {
            "id": 3,
            "uid": "GCHT8MG4JI",
            "uuid": "8TUNNY51WHF3",
            "name": "Light [User]",
            "email": "user@light.com",
            "jobTitle": "Officer",
            "licenseType": "light",
            "imagePath": null,
            "status": "active",
            "passwordUpdatedAt": null
        }
    }
} ^oYF4RNiO

Get users:
-----

Endpoint: GET api/v1/organization/users?recipe=aud

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
    "success": true,
    "statusCode": 0,
    "statusMessage": "Users fetched successfully",
    "data": {
        "users": [
            {
                "id": 1,
                "uid": "IDP5VB5NDHHT",
                "uuid": "IDP5VB5NDHHT",
                "name": "Priya Nair",
                "email": "priya.nair@vertexlabs.ai",
                "jobTitle": "Officer",
                "licenseType": "power",
                "imagePath": null,
                "status": "active",
                "passwordUpdatedAt": null
            },
            {
                "id": 2,
                "uid": "ZU0GJIOKI8",
                "uuid": "UH8SFR3G9EUM",
                "name": "R K Jyothish",
                "email": "jo@test.com",
                "jobTitle": "Intern",
                "licenseType": "power",
                "imagePath": null,
                "status": "active",
                "passwordUpdatedAt": null
            },
            {
                "id": 3,
                "uid": "GCHT8MG4JI",
                "uuid": "8TUNNY51WHF3",
                "name": "Light [User]",
                "email": "user@light.com",
                "jobTitle": "Officer",
                "licenseType": "light",
                "imagePath": null,
                "status": "active",
                "passwordUpdatedAt": null
            },
            {
                "id": 4,
                "uid": "P67P1VG78O",
                "uuid": "YOOZRCIRFEPX",
                "name": "Om SAI RAM",
                "email": "sai@gmail.com",
                "jobTitle": "God",
                "licenseType": "power",
                "imagePath": null,
                "status": "active",
                "passwordUpdatedAt": null
            }
        ],
        "page": 1,
        "limit": 30,
        "total": 4
    }
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
        - Required details from the user model
        - Array of role ids where user is associated
            - user_role_map
        - roles to be assigned within the user Object 
    - Roles (roles) -> Mark as [@TODO] (Will be added as separate object for mapping purpose)
    - MFA Status & SSO Info -> Mark as [@TODO]
- getUserGroupDetails:
    - User groups (ids) where user is associated with (user_group_map)
- getResponsibilityCenterDetails
    - Responsibility centers (ids) where user is associated with (user_responsibility_center_map)
- getModuleStats
    - Stats to be sent: (Functions already existing in the respective files)
        - Compliance:
            - programsManagedCount -> getProgramsManagedCountByUserId
            - programsCollaboratedCount -> getProgramsCollaboratedCountByUserId
            - responsibilitiesAssignedCount -> getResponsibilitiesAssignedCountByUserId
            - responsibilitiesEntrustedByCount -> getResponsibilitiesEntrustedByCountByUserId
        - Policy:
            - policiesAuthoredCount -> getPoliciesAuthoredCountByUserId
            - policiesAssignedForApprovalCount -> getPoliciesAssignedForApprovalCountByUserId
            - policiesAllocatedToCount -> getPoliciesAllocatedToCountByUserId
        - Risk (Mark as @TODO)

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

u4DdVWKLCACaYQAA
```
%%