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
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
```
%%