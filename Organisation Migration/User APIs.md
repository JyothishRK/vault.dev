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
      "programsManaged": {
        "count": 5,
        "diligenceScore": 8.76,
        "performance": [87, 3, 10]
      },
      "programsCollaborated": {
        "count": 2,
        "diligenceScore": 7.45,
        "performance": [72, 18, 10]
      },
      "responsibilitiesAssigned": {
        "count": 5,
        "diligenceScore": 6.92,
        "performance": [68, 20, 12]
      },
      "responsibilitiesEntrustedBy": {
        "count": 12,
        "diligenceScore": 7.28,
        "performance": [70, 22, 8]
      },
      "responsibilitiesEntrustedTo": {
        "count": 20,
        "diligenceScore": 6.56,
        "performance": [60, 20, 20]
      }
    },
    "policy": {
      "policiesAuthored": {
        "count": 4,
        "diligenceScore": 8.12,
        "performance": [87, 3, 10]
      },
      "policiesAssignedForApproval": {
        "count": 17,
        "diligenceScore": 7.34,
        "performance": [70, 22, 8]
      },
      "policiesAllocatedTo": {
        "count": 11,
        "diligenceScore": 6.48,
        "performance": [60, 20, 20]
      }
    },
    "risk": {}
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

Compliance:
    - Programs managed by:
        - getProgramsPerformancesForUser
    -  ^qshokCII

%%
## Drawing
```compressed-json
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=
```
%%