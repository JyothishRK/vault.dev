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
N4KAkARALgngDgUwgLgAQQQDwMYEMA2AlgCYBOuA7hADTgQBuCpAzoQPYB2KqATLZMzYBXUtiRoIACyhQ4zZAHoFAc0JRJQgEYA6bGwC2CgF7N6hbEcK4OCtptbErHALRY8RMpWdx8Q1TdIEfARcZgRmBShcZQUebQBGABZtAGYaOiCEfQQOKGZuAG1wMFAwMogSbggATgAtYmqASQAxeiF0sshYRCqoLCgO8sxuZxSAVgB2VInEgA4ANkSxlOqA

BlmeBf5ymBGUue151eqlxNX5scux6u3IChJ1blmU+dupBEJlaW54+LHVt7WZTBbgA4oCKCkNgAawQAGE2Pg2KQqgBieIIDEYwaQTS4bDQ5RQoQcYgIpEoiSQ6zMOC4QK5HEQABmhHw+AAyrAQRJBB4mcxITCEAB1B6Sbh8cEQQVQ2FcmA89B8ypvYlfDjhfJoeJvNh07BqXY61ZgzoQInCOCNYja1AFAC6b2Z5GyNu4HCE7LehFJWCquFWTOJpM1

zDtnu90rCCGI3GqPEWm0mYzejBY7C4aDmaaYrE4ADlOGIftVZhNjrME4kfcwACKZPpxtDMghhN6aYSkgCiwWyuTtjreQjgxFwTZ+Eyn5fmEzGf3ir2lRA40I9XvwbyRBNj3Fb+Hb0r6mAGEjhgXHCFQAFUwiwADouZzPl+v1+Px+oL/fr/xbSoAAlBAAEchHCKBUE/H9oJg3B/2vACABlII4GC0PQ79CH/BRcDgQgFHoeJbFIZRrEIIxx0zBQhDv

CIoIw6CdFQbJ1DYYh6IYzisNQAAFAB5DkABUOM41BsH/SQQmIPMRNEtDuKgYUOFkhjiH/TtiBgFC5K4/8OFwbIVJ0zDuKyXA2SM4zUEIGz/wAKzsQS1GCSzjMIeh/ygQhsiMTgEFcnSPNQKFgmYIy4lQABBIR1ByLy8C8zgAuguDUEU2FUOwSSCW04ymORZRmJoiDNCvfEvMYIyUn/aLWNIcjKKS1DjNSqE2CgHDiH0X1UBopgFGSn8mLgNgKCYX

q71yqyYO4nimG68NMzQfRrGiK9bxk5qYOSVAADUCBIRqODCra5NS/TDNO6bMP/IDQMIQJ2KuzimLMiznuu7i7qEB7Y0GrjuOvDhCFA/yPummzuP2jxUDe/BUFwYgyC1f6v3E4LEXCVGMO4/RithzBCEFbH0NswDMeYIrBVQUqEewCqwfQsYavpzMTqs1LsAvPob0muLSBgbGmI5JgrCIIwr2x9GRdJQDwhG47GbQ+Z/wAIUK8rM2x1LEPMHIwgAf

W6K93GwL0jqF/8YuwA22GZZkwgg03zcSjhtCl/8eNCZgKGRYhUGUHImAt8H0LU3nxuccngmY3C8I4Qqeo20gDYAzGDYAWVw1BmWRBH2Qx0LUBXWEnqshB/z4jhO3pRwE9hla2TStgJqYbHmRZiqjVQRC2EK3PSFbweuYQHmeoKsiKNdg3NbMWADaRZR2bQqY5dpTgwkfYNKEE/oqnPEIeeTk631P99lPBv85dB6ntfgpCpuum7UBwvCCKIifganq

i+pYAbQ7QkxFikg2Ik3kp7ASwkAEwXRpJRGm0n7QQUkpbG4cNJaTATNPSBklaIK/GTOGmCkFkwcpoJyUAXLQLku5Ty3kEC+U1EQn8QUQpY3BhFWqklcjmBDhzTySkxLZWhI/US+USJUxKmVVmlVwbVSijFEB9Vv5NT4RjdqnVuqoV/v/Kyw1RrjV/iIp+s15pE3zBwZaq1A4RwfODHa0NDqu2Xjpc6OCmHP2+r9MueV/yEKoaJL6IEfqPXcfgwGw

NQahOsmTBx/s4YIyRoEcMHtC5sLwc/PG1MsBEygFEsmaci6ZMkXTBmRlmZRVZhvO+YlubrT5rkAWltUAi3qgdCWRiGLSxyP7IC69FZGRVqgdWJStb+O/DrPWisjbwBNgQM2+BeE+N6lAG2dsHajzEnMl2mZ3ZjLRp7b2vtSD+0DpqcgrtUHwUmlHApV4VpwHjonVCydU7pyznAHOecCDw1YZTEuf09mw0rtXNgtdfSFXiYpIe7dO7uW7r3fuedDE

jzHqhT+DVp6zyNAvPuzjoKr16QrTeXBnScCgByci4heBmnKLnXIzQDJsmNKgWYbxjxQEikQZQWZ0DBGZAMXMpB4oEE5Z8Hl0B9RMj0LkcyZz3RoEjJuaUyJPi+gILvE8+9ak2JPmfM+H5L63SCeBDpGFUoIWQnk7CuF8KEWIqRL+R1qK0R0Us4BoDAWkwgUJFJcDpK2PSfg/hGVLk0zYhgz14DUAXVwXgghjd8B5JIY5ZysbEE0LSnQhhaan4sIp

uFGqCi4o8IuYC1K6UciCIQDlJpBUJE0ykaU2Rha6oYtGaotqHVEaaKHq6nSeixqD0MVa3ipjFqcEsfpaxx8jL2IOmOJx1SY0js8SEwFr0E0rqCV4pN8EIlgSTVDedDdzLw0RsjZJgL0a/JHUUgmOTd3R3CPW2ms8c1fnKZFSpx1qkj0vDY2GDTBbrv/C0sWlLTXoS6bLQlG932oEGcMzWKiWr/l1mIKZxtNn4HmYs/tVsVm23to7bDuHXa7KsujL

24YjknKDuc9txlw7J1QDczGscHnguss8u8rzgiZ2zgPfOPyKbF19KXbGFdUBVxrscrjkKW6/xhRUrusAe590+UOyaKKrzjxIpPI6M9pHYsXnin8BL5Zwa3oCGKbBemUu4JCMCW5fQIAABIfC+KeVAf4eBjGKAAX22KUcolQJDAWZABOsABNAA0vMEUTJja9D3m8YY2Z4hxDmDwFIKRVg5Z4KsfYYwazSmZaMF42hEhTgrPMWYiR5jjFnKV809xiC

PDQPViYbxJKfG+CaU0gIE5KhpRCOU8JETIjRFiTESAOz4kJCGMkk3KToGpMdOkDIBXSlZOyBUSoZSIlVNGIUsIxTtYlGgKU5pZTCn21Sw7/I1TCA1FqH4eoDRGh+IN6UloRw2kHE6HbroEDyujRuH0fp0voFwPEYMXZiBhgjBD6MCBdxoDqzwTYhXqgpEFeY7gLx8eZiLBwEsOoXj/AmMV67oX6yNnRznNsCAOwI97FkOKgPhyjkvM2HzNWZxzni

GWPHy5xPrijOabcpc9zM7ZXvCQ3ZHAQRnU+PVb4DXoSvt9E11SLWQbctat+dr0XKJsL/OiIHmKjxAd49Js1rxQMoxJKSCDA3IJDYCtB4aDc6W4suyNM1uKkKNqmw9tCfJ+TyXm0KBb5GxW4QlRjLjg2VqytW4RtbxF3tfdI+DcjOEqrN9UztGieraKaSNQdQ9ffUM9mO8xk61o6tnf+WJR0zOcVcZdQN1kjX3TXbo/8N7A9IP/He7JxNR8/nyaJn

Pjb3Lwc/d+zvDFzU86PvUyEwGh/NNFm0yWV7QPdLXkS+DiGNbfuqeh/WCBpmIFI9slD+HlmrOIxs52CzyMpOTrc1A7zHleoN9MxNNhMElpJ/ZrB/ZAh9A2BGBoCKYKMmNYU54tIEVQDDFYw1BuNUBTdDMsV55TMR1+hyB5BUB6ACAwJKZSoB4rwRwF0EAAB+IyCzPpYlbeCgDVbzCAJXbA1XdXU+TXNCbXY1W+Mte+S1afZ+V+W1D+fTR1V2Z1PM

PtOSIBG3D1XvB3J3YyWBV3ANd3VPC+Kyb3TSWvAJbBHvd3YPOwUPCheDKySGCPehKPKQ/BGPNJNCDhItRPPDM6QwqtGtK3OtefEZGRdCAvBRIvXw0SVqNgdRbtcvO8FQ0Rf8KvAxSaEdOaUgBaRvWOKdOpN3baNvedDvJdNxVwvva+YJAFXfEfTQ8ffGSfXJCo2fQpfGXPJtJmWFKpcQm8DfAowefmHfJZMDA/Mw78aDHpSzfpcGC/EZZ/PwnuSZ

Q2LDT/aIl6AjN/dZJ2LZL/HZH/O8P/AArjego6UA75cA2MBGWWGAuAq435JAnScOL9VTNAjTITTA5XHAvAzFYzQg3FYgk8UgtACg3wZ9Gg5EOg/o5g8GVgs/azHbMlClCWSUUbFkMlBlbqfAZlVlI8foUVblKoPlbbc0dMYVfAAk8VRSOAKVMlWVJgMHRVPUeqfwdVBXdAXglXWiR8AQjXIw4Q/vKgiCPXB+EdGQ9+e1AzRQi3ZIjY63ViO3ONT2

R3X1PQ1fCGQw0NdBcYhif3cozQ6wzQWwyhA0hSLNFwzQ9w9UrwhPYVUtVRCtTKIRHUmmf8YI9ohfMItCCI1tYvXo0vBIrRJIyvfRLTcaTIhvJaPI5vVXdCOdDwUo3ogPeoqondK3Oogw+tJox9W5SmEIt9Mpbon9Xo68fogDIYppUY8WQ/Z3PfGDaY4ldCOY5DfklPJYjDFYmZR/PYhYlI1/IjbY7so6R4uSdGX/djY4+uU412c4guc9K4qA4KLI

O4hA0KEc0SZ4v4t4xFMMwYr4vTB1NtTgIzV4nFJeQE6kMg0EqghtWgoAhgmE9COEqzElaUXAWzezFEtAJzFnMXTUDzPrbzXzfzMoILYoELSAMLdAT4KAQiXAbsGktlGZFLTVNLb7aoZmY4anPLNYMsErWnSAcrfYP8Y4SYDCisRMY4G4aUNrDrVAa4FrcoXrLzbgFMIbYEKlNE27WEckKbCQdEWbbEebAkP7UkXi1baAcgDbekOKJkXbTkbkB7FU

OMN4bi0UcUSUVS07BAe7KoZS+HPwSQJHd7ZVT7WAb7NEv7a0W0QoIHc0F0HBRklHc0X0aSaHCAXAHgAy0MN7BVZy8oGMPnanYXRYFIHLYnTgb7CKjgUncnVACYTLWYMYeYHgTLWsBsYICcFsOXaUTsEkYgdnfsPIWy7nBgvneIGraoRrCqxIXLXUP8tcPyyXcoaXRnfcQ8c0dlKoAAcQ2U5Xhn4N5JfCEJghEJvmFJLNFJaKN1kMlIUJ/hdSaXdU

VPTX/G6u7G0J0l0PgX0Pt01PYRbS4TtOT0WMdICMzyCOzw9NCPz0OqiPtNQzUS7S6kSP6hDOr2HWmtHWyLMSjJWnyJbzsSLPVPQlSmaFHiyhr2kiiDZBBsAWP1aWrJdMmNPxfK6NRpmI4K4J6r6oLkGqGucBGugjGqFJdPNSmpTPFJN3kKPPN0Wqt2WpHXWs2tHJdx2rhr932vQhtKOpLROpiP8PT0CN33dOpg6MXyqjuqUXWLX2HziOep7Qrytz

SN3JdK9W+pyL+qsQGPVJ2heLZmqXBpWUkChtHlPQ5sYgRvA3aRSRlimLYKXyNQdoRPsqRIcyuzRLpSgExKZSeHlxPEpKJIQH5SZDJJ4QpK5SpMlTeGlRhrlT5yZOVRZLVXwGxokF6ogn6sBoJufCJp/BJt10mskMpptQlJ+IWuUKWvUJWuMTWo2tVPZpHUdLj04WLST17K70FudKz0KnzLz0lvj0UVppL3lrLyDLeuVtDJrwjJ+vHQsWjOnW5KBp

UwNt6KNshsMWhvNsrP3yRttpP1gxmPRsPvYJs0Uk/KpR/Jc3/M836x820D80C2C2lCgogAAEV8AeIeIxhrxJBuqktkKqRUtpRod4hnhqhtBZh4gXhVh/hEgzgJhZw3giKjhtBVgJhccEwxgkqLgJhcTWsNK0AJhEwetb7vMsH2KRstLxtxLptBK5tcqFtRLlsKRegpLaQZLGRnQ2QFLFQlKjsVKTtxtzs6KCKZRtLdLeQBGDLXtwwTLzR9R8QvsB

s3grKAcSrgdHKE7/LIKocAw0hnt8rjKmqlUbs0c+dCthcNhiGlhorCcVhorYqqUcKcH5gjh0qGc+d2rfzzQ8qew+xOcNHzRpzGcKrpwFhsseAZhr7GrwdmrIBWqvGcrOr2SIBHwM6AMGwYaDweSc687vwC6xDVF9cxTS7qbDyzclC/5kASAFBGDAhDREAABeKuhUpm+uo/VAP1QopU5uT3bmw6tumWs1LujPF0sRXu66gs5tQe+6/mzup6se3td6

9I8Mr6rIzWidBenW1vZpEcEaIVK4oCRp8IZAPXSaDOX0byAgVALJ82s50fKOIQfQGunSKOO52G6gVJZgL5jOZoSKZpKIKAGiL5jkDkPibjXOJpa8bqi29CKOZQV5uSKOFjKy1ALe2GlJACOEOFtCKOUQJF0SG5BswgTQNkbuDDPoQeDFnJr3f8DONiL0Dw66KOfQXFmCKOQUccMg4AMSAwHwJwMQVANAYAILVAEaIgbAGAEVgLVAALQsleno1Rde

k2zes22GmmUIK4zgJck5nexGiDfe+s522Yp2+E1880cgTg1J9JjZFjD52lnOwm1s4mwUwu4pimzMqmuQipp1GUmp4gOphpwgZp1p23dplm0Sba/1dlnGLmzwwZnwh6tss6oWi6kWq6sWz0262Z6W5NxYgMl68e0gWUhiAdVZweGezZ+e/6mMpeuM0DfZ5EJsOWE5+Qc58aS54GFaeGB19tx53qF5ohd59Vg8L535X5/5wF8cEF5pcFyFtgaF2F4d

gOQlziFFyaNFml2N9GbF2Nn8fF7ANdhiYltg0l8ltTSl8abd0NBl4gJl/d78Vlx9r8TloFnlvl/QAV6wIVmVr5iV8waV1AUVuVhV/WpVx6lV027J6grV/2HV4NxAfV62msnQ4/Y181pss1l8uSt2r86lUlelRlbEv2vEgOyOoOkOwVckwOqkaO6UWO+k0gJy+JiAFVVk1Om1jgDJ+10d3VAm/J38N1opx6kpr671ua2mqpiIANoN6tENhAFphm6u

iNhumNpulBA6weoZ/NgWvptPbuy6yZrNm6gewvPN+Z2WxZwM5Zyej6jI9ZyMrZ2txenpn8HaDkJtw5qYtth54pi5q5nt25vjvzp+J5odgdvt8dimSdgFrkGdn5udiF30KFq3GFl91jVdld1F4kD5G9zpvdldglldk+89ogS9uKa9vj29xl2PAdtlldrlvIEVz979snK8P98Vo7KVmV0D8GZfRdNeiG1VyabdzVsIeD1CRD+w+GvfA1m2zpu2jGxs

5WbDzGs+uzcId2tKUgZzP89zMhn4B+kCsAMCsoCCioPnCACgTAUgSKa8JoXaABnoIB1CkB7gCsVYSBrHcYHLanVKrYMrEYRYP8M4WYcsZMDC2cbrGiwh+K2Bh+xMeB1YRIHgKq8sVMaUZiu+iq2YNBhMerMKqqyihMShzi6h4UWh/imbISxhkSpbKntbdhzbWS7hvbRSvS6Rins7OHsRtSyR5ULn6UdUIy3ynzD7JR8ylR37XL9RtAIcTRt0bR1j

1y/0CQXARIbyxHMXxOsxxnTYDBxcGYai0kvMTMVEk38odMcxJxwnbBq4aBy3yC+nTKtq5J8oPxgqgJgcIJ8oEJ8qmreBjCmBhKmJiXUxlqtgHcJJg8Hx8oLqiQFjSKHiRofINUHeVJpPlPtPxE3IZEqlJILLGYKJhrFIT78sUXV2ojrEnE/2jlCjiQYk0OpgGjhvtbej80Rj1zZj5XiPyAdjlOtO9ALP1Ppkd88+rb/Dq+/bgClinUY7p+8Cl+q7

tgOECYXabsRIXaA2OAfSQMaLfAfQZwSKTQIQdoJCl73lVzJkaHNxiBxIMsXHo3vLa4ZBkYOYKYc4dYRB+YXHNHurN4LRUuzxV6s2gZYLVSKzVAMKUAtKlj0O5oBqoeDRBomEywlZ0GUAxYGT1BDc8JsrDanvQyZB4h6eCORnpJRpAs8uGO2HhgL0ezHYbs2lERsAL54SMOeUjJ7MLxeyi85GOoCXoaCl4+Yfs5oNRjZXl52VaUIOFjn3wqB6N1e8

wLXsY2A7gguggDXgOCDO4BVzGqJHgIkAqrxBYGlfK3mb0io6h5g9VU3hmELDFgqUJwCiplmFweNXeMfDqh7zZze9iqog0qrzknBTgg+ywI4KHwarh8twUfGXNlVj5vARovodwfaCUFFBOgZQUbAkKUFiCygcQ+IXg2SDgC8sKwaAdUFgHxCwAowbQEgJSqmC/MCDDAYkGSHggUhEABZIKARD6BuoMgWMDxDYBRCghJ2ekFAFViuVwUnQ80N0l6Gk

h+hJjLSt0MiikAoQFAP1AMPKDdJJh0w2YWMOlASsYAygYwUzlj6L9zuy/KoHZDsiJAAAUsoEEgAANZgJgBSCYAzhkgIwIkFIDQhqg3VACMMAv4PYGmUQYbAw3NDQ5nASwPHslUaxJVqcKVL/gYMIrA8qw2gPIdTnODXA8sKVGHgQwuyE5EqxQgHssEmDrAommPc0NjyApuNqolwLHOEzAYXB8G5QIEFQyEaU8VsdDGbIQKYYM96RVIZnpwxJK0pq

BrAwXuwPoHCNeeOAmgfpUMayM7QZg8oIoz4HMo9BllWXiIPtC1CHKSvOYbozcoBgJg8gsXhBWUGX8Ugag1SpoIxwpUwGpocYHYyuwNZHGVgn4A1nWDzhwejFZ3hlVHhu9whuVVwRzh94eDpQ/vbwSUMazo8IRdQ8XCsKlwhC3RYQHYSUD2ESBYsPAYCHAHmDMhoQ14Z7g9i6poU0AowZYDCJqx5YNgWOTYOgzf46gH+cQXLLVmqrJVVgYDQAXD0X

B4imK8AgQWiWpHk9aRPFVkegAEqMjhKi2Egd2LIHSUtsclbkXw0558iAqDAwUZ2J0o8jaBgjS1pwIUESj++ZlGUYIPKDCCucivUHL30hzqj1eswLUdwLiZSDAqkoRKhVSgHIjDBFgnlH8DEbW8ScNokwY6NR5/AlwLlF3q6KcFx9cQnooqruOCb9EA+4TcsCCMaxh8wxkfaPrLndEpNNUZ4bVPjSGoGosaqTA+P+jQm8kMJhHclNt0KwESfaJHTr

HX1o68pg6nIyAGHXcCUSJUiFBjnSW76SDmSqqfSJx2QnoBsJm+TaE63wlvkPyk/S+rtwAkhib6gFI7o/VArP0XKV3WqgBFxyYAKAkUdMShRokQBocZfCBnlm0HVZZwtYjYGuIgAyj/gJFF4L/0mAP99gxwesaiJMFNjIABIiylgLQBcVtKpA3sbNiZHED8qpA9bBw1HFs9eGB2EUXOMYGaU5xwooXsuMMqrjeByjVsao3lEgTxBWjVUdIKPEw5qg

p45HKx0vGdZJgKAmYCZJfGbC9BlI2iUYJipvifMyVLBpcCqoOC/xCE5wYBPyqFVAmPo0CWVX9EzhGstYxcDBPPHBD4JYQ9qdAFSacls6eTC+JhO4k8EviuEgQoJKr6ET8OxE3Pt7WI618yO9fMVJR00l0SRUbfRibSRlSsSDxSdDiWyUWkzSVp6uNaVSOEmsAp+Yk6+gdyknz8ZJp3OSaFiu7xBcggkbAMBHoBvCjwKgqaW91+E/A9Bf4XLG4ySp

JAywKVJ0aZJ+DmSYRtVWqtg2QHQMnJ13BsRcFIbfTkpb5b4dgLnFeSaePwj3syMHF4Cme5AjkWOPZ4Ti2BdA6cQKIcm8AhRC48KXFLFHyNJRG41yTLytBy9FRzoCQddJcoyCYcakwxj5TPG68NB+vc4CsDGCpUdBFogQXeOqkPjbeOoBYKlRSALAABL9X8VlS2GTTPeXU70dLN9FgT+pP/PLL8G/EtVQxo05cBGP/F18camdPGkvQEnzT0+1rRaR

kyzqPS9Uz0yAF7Xz6okSJu00jkhIOmElG+1E5vkKnDoMTqSF0uOgyTlmSjk6nEofhACjnBz+Jc0l2i9In5vTRJe3KXK5ln448F+skpfvJKqBuZ5g0WPiNCF7iaB1Jr3TSaA0J5gC0e3/EkcQyKylifM2Wb7hsDL61UX+KQEyUAMnCeznJLY1AW5NQAeSaGQ47ybT18YMz/JQ4wKRQM0nyUYpU4sbMKEilXZ+ZHM3kVzMgAi8EpplSXpuLlGSyFRC

veyrLMymq93KuAVWHlMymFTUAawOYCsHyxlSapcMtEuVNqntcfg5stHqlUWDoyiYLom2d41ZydS3BaUyAH6J1AC43ZQ0reRJNiZqyEmfstqeJIT7oAeOk0PtrkwE4Xxa578jPpHLtZsLquaudCVwota0o8OVKLaetNIl7S05DEpvtR1zlnT85MdFifHUykD8y5qTVheNHYVCK8JIisfq9O27T9m5kkufvfV+nqCLur9eIMQBSAFgEAfEKAABGHlr

ZgGsMjHCj2KHoMFg4wJKjjKqkYydQYDKYNAwuCFYqweDJ3kTN5nwy8ebjFHmaLWApVCZLk8hXj2OCbBaq//EnmI3bFUz+RdIpmRAGPl0zcQZ8sShfPZHBSqB7MsKbFO5kPzZxhS+UALIaXvyVxYvEyVKKSmyiUpf8khSyCAWwS1RavGHHCAgUjKZQRonzNoP2BlhyKesvxdaLQVljEwhWRMJbJ/F4LIx4k+2cQt96kKXZ5C6cJQo9kjS6FdQhhRN

KYWpNk+jQZbuEAWncF7ljynPutMTlrKqsxfGYI1nL7U5k5NfVOfH3xJnT5F0oE6RHUOl0cmJnfVRUXPUWly7pLylPm8sMX1zjFH0mfjvPbl/TO5AMqoBMENBHCM40IAsCLHmCxY2ACAVYPQHwAIgOQ14bsK4okllKtJrFanKkGeB1ZccGDbWalTnnQMdBVWerPOFqw6DTB9kuinoKnAP0qwyVM4NVWKykzzFiA3LOWPNmLA1gaPRBnvIPlFK+KPY

2mb5IHHnzill81mSFNvlvzxGPM0Rs/PqV3yIAH8rpYlP4F9KJZ/2f+UqOGU+z5Z2UjynWEmWKD4h0AFQTwANGo5GcOgs4H5inDBiUFhOM4CsriraDsRJwerC1PwXu8Op/jL0dEIAV+9jl/OU5USKoUXKdGVy8abbPEmRDHZaQ1IUoLACJDm1yQ24GAAbVlAZVUwTYFD0VVrzlVTaoodhQ1URNtV1QRBtUM6C1D6hUARoc0KbBtCOhUyrlkKmGF1x

lAmUoYX0ITiQKogQqRYaNGWF+r5hpIQ9TMKkiZS1hGwnlN42jHWKrugkVYO0KaCmgzh0ITALtAzh8Q36mcBABMHwASh3hVQT4fkqzGsYlgcQWBrAzmBrBrglUwVQuG+5ryFg+WJqfAylXACcKUwS4C8FhGmCYNKqnHuMCmDjArgE65rGA2DH5L3JOAmmQQP7HMMAp1S1nrUtCn8MnValR+XzOiltKnVLqs8d0rFnS8hBqUw5UMoylTKQFAYZlcrO

15nidRYavUZGr1585rg1wDYAsqWW6yIVNU42T5lnDgN8saJXBZ40YWEK81wE8TWQpLUBj3Zw0wIVMsSaML71sY9AMQEaBVxqgygOsKIt1EZj3FQwQnEVj/D1ZaxtVJAeD0JnlZS+eYtGfVhOCJLdVsPXmcCKI1AVxgbYymbRuplHzjVjGlkeapY2UD7K44x1Taq43NLGlrSl+YuJkZcDxRbqn+f0q9WDLlR+44BQrI8rNBg1lyqBTMHyzJhtBes6

BvlhTVUpf+GDMvhWECWmbHB5mj0UQvzWDKbNYTKJYjN/6k9HNJ6+hdWoIX7SqgLGP9H0AeZ8lHw3YUkHWqgBoB+IQkBGMbh9ZSkK6tiR8M0DZDPpFyi8cwDTAvDQhiAo0CxBwudaPgAAVNJhpqVNk4CIBpIiGCCkBtAdkXVOdswAHMIIy1U7TUkPgIBk4AACkCDARx24QAAJQPNHwCKcOvKVtwY7jt2Ou8Djt/hfMF0uAEnRxCjjt5dOT7aNDgjQ

CroaieLE9GyB53bpHoXzUEiQCMj4tMYaADkCAi9BxJCYgoL5swFl34B/Y4tL0jBFZ2oBeJdBLfEaC11/41i+bKOG/TAhgQgCoBIgaatlhRBmA0IPFFHCrIQZFyYQG4g2TBig77t+EO8GYDEARA+INNZgEdBaS+6K4iO3Jo+BFDIhYQg8dHTyXFZQg/dzAKHbUhx079cAioUFMQBZ2nQ2dJRV2KFw5aoALtzAEQLrvGg07FSTzH6HRjORnEAAZNxj

vAzkcd5O7AEuQHyxgidWuljM4AAB8T6DjIAXp28ZWEBse5E3pb6Zhu9uem8IJDhC4F38OxHDE/lQit6+432/HdUWz1a7qMPsP2AHHoxnF19qgdvVvq8RE7MuWcR4QjEpiCQ+IzgOsHxB72TQZMoKOTPXHiRRxr9wiUIKgHv2P7n9s+/WqgWLgaZfQze0ZAahPpXhZQQgemGXtO3DUL4wADiPeEu7oGEBGDGsGgYgA17MDkECAI0DrBwg4QHIRIE+

s2AFg6w6B6gLgbP7i7uA6B4g6QfIOUHZg1B2g7geXRMGIAsWBAFpEihFsuDp0dA4Ql4OBkAAApCE+CBxDmgocFLoAMAiGfw6B0hOQhchBDcDkrW/IJBmQEH0DKtFQ9+HQPXNA4XsdQAYbSY0BcDTXGiFYYLI2HRDdQ0IFAD320ZSyZVSKLkl4MbL5gzgFHs4DR5PrqgyAPLMgFNC1B0Dj4ALM8sO3aZakSB5A+dsu3tDcgN2yBF7rLoQ6/WS9V7e

9spifaN97ezQL9v+0UBAdQig1GDoD2+tFCUOslCFDh0I6kdHAbsCjubaU62I1O2pLjvx2E7mAOesncUZuZx6XAmOy8LjoZ3otxwzOwvZl3Z2WdMuMaIXZ3qRZRw4Yqx7faLvnQS7Uk0ulXfLpyRK7DjDaUzuDC1066a8xaWAAbvYxG6TqJus3VCUt0VL/Ytu+3Vrqd3tIXdJ+JJOa092l0fdesf3YHuD1MBQ9LRiPRwCj2PDxoYxqOHAET1agU9W

OtPXSEz2Iwc9B7PaPnqWh7GS9ZemvJXq10MHa9wcGco3ogNT6dWJ+zfcLq70v7I4A+v/PcmH2/wDYY+ifdSZzmcAZ9OJx3AvrWQkYHjtJtvR3u338nOd7hg/acgpMgE6TZ+hk9nqv30hf9d+h/U/qZODw39YKT/QmlVM36/9ABrU8Aa3JgGnkkBpqNAfd2oA4DCBwIEkeB0cBUDzhxg1gZOB0HnD+B3gywbIMUH8sHBmg04dUN4GfTaAZgyQf9Ps

HODIZkwxAB4MRm+DAhqKMIbjNfgxDm6CQ0W2kMsk5DfQBQwnCUP6BjDGZiAOobDxaHnDOhxWHoaQ68GjD6Zwg2YYQAWHJAVh0s4QbsNhQJD/dJs+gdnUynjknh3nN4asN+GAjiQII9UBCNhHVgER1YFEbSYcBYjBEz5QR22nSLgVXQUFdCqolUddNOc+iUoo77lAu+aiqZRouRXxGK9iRoHc6Yu3EArtGRu7WU0e3zVOA0nA1G9qLhFHT9P2kIH9

oB1Onqj4Ouo1RAaMw72QTAKE0+GR2o6ujxAHo1jr6MgQBjQxjgG3tGPV1kLkxundMaZ3YnOdixjuq+y53ZAtjaZeFgLvwCUWRd5BXY+DEl3BADjwgVXfekV12nTj6u+DJce1SYFuEtx2fYbt2K+EnjCAc3SOFeN+SbdoQT47Pu+NlRZYru6Au7tAtAmITIJhQLUbIhB7XYIevWLBdzowno98JnC/HqRNR8UTd4HXeiYz1IgsT8xvPQmQL0EnjoRJ

5FLUir29Qa9h+uvZScn28m194p8/Y9ClNkXe9LJ9jGya4wj6mAnJ9ONycVhBXwrmXQU4vsHKingrxRiUxft32HJZTR+lvSFeVOX7v9ap2/f/s1NAGBTr+kFHqYhQGnyrRpjU4Aa10gHu4i8QK6WhtMO07TTmB0wgBAsoHcD7p1ADpJwPemxrkZ1gwGaoPBmvToZsk1Yb9NsHAzsZxa/GcTOEH+DghtM5tbLPiGkzUhmQ8oHzPgRFDegEs/2fLMpo

7CBBpktoeWIIA6z/kBs6GU7OmGVo5h8cO2d4OfXxGCXBw32YOuEHBzBV4c2BLHO+H8s/hwI8EeOBzmFzS5mI+is24NzHMWK0xV9PMXAVXNXciQLtHiCRR1qu0NzLgFwDzA2AbmegIhGiyrACwRwngN2GcAsrQNlM8De7Pv5hU15tYqAVVRLFA80ADUh+iFqWDWT8s5szDaxWhGzhUqSQI3oVmqzpb7GKsXDRgxR7zhhc4VCmRxQKXVbcBhqkpflr

p7W6WGRti1TUtK11KONFWmcbzOYHjZrVS48oAJsa1fzpR4s0TQMvE3ta2JL9LrbgH/pyaFBim5LAgJU3qzyqcwZGSj0zWHmCcay8bU8D+DTbLg1Cuba1JuUWavey2zKatoFyzAjJ6wEmdtsuXOabl+NglRIDYDRZmgiQACAWEIB8QWVmY97ggMgmHAbxxDEoXkLnn/DZwVWPIToLL4oaZggSjecLeIYq2dQuIvVXRry0MbTbTGqpSzKttcibbk4u

23aqYEOrbbLtjpfFNdUe3elW4yADuN9u+rLl0m9Xm5l62Vr+teQqsOWGakJ3zeZY8HsnauybANgs4V+9srM3Z3Ftlm7qU7N6leCTleDMLVAz+CBKS4mUiuzWoDmJ9JoITJ0w+dSNRCbtjuLI+Uye2fmLc35go9cX9hfaSjZR4C0DtAs6WPzNgSC0KGgvw7w9cFtox0aFSIWMdITVCwTqXKDGnL2u50oQGZDXGFdeQA1FhfhgIn7yeFpgHFdICM7Z

jRFsiyRZdY4mVjqZQfPztYSsW5dHFqACcbYtq7s2RkHvWWQEteQhLOJkSyvp7IutxLklj5EJit3MM0oclh3XNxQ4kO7TfxtSxfDB0aXSAoe0E4eT0uZgDLYgIy861hMx6OHFl5E+GGTgjm+gdlzE9nv4cqP5jOJwk4EGJNeXtTrGKKzHBiv1w5HCV/jElatMcBUrKLefRlZFOiXirOV0K4ybNOvELT3VqAxfBgP9Xdug14a4+FdOhmxrE10G+gfD

OEHVrc1oMwDeWu+moza1+awDe2voHdrqZzRADaOuEGTreZlvhdaLNXWAbFZ+61WdDM1mwgr1qw42dGcVBvrrZ36x2ZuvdngbpSG6+DZox+wknsYaG0mYnPw2ZziN8I5EeiMrm4jKDgxP0XQcGpHzz53iDg7fOSdKmhDi+D+Y+2ywyHAF3AEBYqP9OOANRnI/UZsuNHYdMF5h8ZfaMIWpHXDunf0d4dKPMucIQR8I8wKiOTowx0/dhYVKcP+iUxu8

Ao6iB0vnLjiJY1HHUe871j+x5pKcaaIGPdHPFkx7Ps+fXHBLwxMi9Y7IyPHUApuiSy8acdvHXHdu9x4pa8cqXeHAJ3F1keBN+7tLYJ/S5pYiekuonpl2PeZfGOWWk9iT/oik4ctpOtdGTty6XpyeeWsd3lyK4PuKeFRSnXJ7ODyddjVO59QppfUOUaf/nmnO+1p3CjUxdWY3nTx8N0/tPAtHTVDka26fYiE5sD1z8ZzNejPrWFr9Byt0QfmdTONr

3BtxLwdWdCH1nN1zZ+ge2eyHdnhZ5QMWcOd3XNDYwp6x2Rev6H3rg6AGy2bbMPPrnTz3sy8+udvP99kNrwz4Z+ew3Jz052c4C8XPAvVz209c5IrEXV9fa5E/aXIqzkKLjze586SosukXmdtbHJFVxO4IsY0H95qF5g/SOwvBIuD981JyRf5HfzaLnK6UcAvlHKj58T3TQ6k70OmjJL1o+S86OUvuX1LtC7S/4cMuxmQjkRzklZeYWRjkjl1080w+

yOCLij9J3idIvLHudGjvnUXu0eSvDHejmV+xblcXGFXZjvXZY851qvV9WurVw4+ktm39X8lnE0a9+Nu6TWgJt+Ja/CDWuQn4JwJ4ZYdeR6nXsT11/E+T13hPnXrrPQK9xMuX8TTF4ve5cDcJHg3+T/vWG7jixWOTUbj5Nm75M97anwpj/A04VMlW1jcbjq5m/APJWerXT20/m8QNFuBno10tx6cmtLXprDb2azGdrfen63kzlL0s9bdJn23+13A9

248o5nTr51gd0O5utHPR3o08d7oandJmrno125/O/+uPOgW9h5dxLVeeuGhzxAT58QG+eEHfnU5hG6EYPco2QXG3C+pjableyzFbcyxf9MgpXd5g5w+oHAG6rXhYGjQaoLFmvA8Azh7ATOEBshmX9nV1aL4Xrc5sLBmYc4Nxr8A2Bjrot3AOYCrB0HGadZsokrAbJiV0VsGEDWsX92w3g8kGcAsmblgyUnBUqawTW590Jk0b95C94paUpNUr2ita

91jdbfY1b2D7tqppQ7b3tY/6tn8hRsJvJne3Wtl9yTa+5vsw5GgwasOyoP1GdB1BAgGZYsGwW/B1gI26Jm/csGrKfM/N1DXgxM3WzdlOdh2dEMuUF3wmiYRBugxMnwOnN1ymtVXcW9VASVjQACNFkkBHC5BwGkeTfye8EbUgZQyyZgr8yPfsx1WOIBhXN+NYcsCYSydLYxwOb8RO8v4Flr1s5aWlhtiSkj4K2MyLbxW6+WVv3sCh7b9q3jbVsFmu

3OlgmprV7e3Fiaep6UlUVJsDtHD77BUmZdA2QFlhAzSyjCl/fnknA15pgou1mtF/APc7VmqZVL6gcf815Aqsu5WsQf7a05gcoeP2zO1tG/3121AMzSA8Ivcjm0MD6i9IeQeKH2LqL+a4Q+Q7CXUF5o5p9YcUvyPh+qAMfDx3YekkGFiRzp4RajwN/1H/l/w9VhwdcCqEWKDXmZDDc0okkcVtEDVQts1WMHA3e7rQDHxUAAAal4gP/9IM5fLtJ670

zuspY+O8nhwB5uA1gW5DW0/oM7xmwzuW70GiXhl41uMzul6NumXjdbLOyZntadu1zgV69uZ1v25eQ+zsobleI7m9Zju1Zs9YXO07m3A3Wc7vc7Nei7q149mx1iDbaGXXhDY9eUNlu4DeO7n877u85kC7LmQWNF4luBBjpKpgiAbF4TOdYPEDRYqsLMCRQ8wJFAcg3VKrCoBSAXIEKBSgSoFqBGgZgHZehBixjxAGzlmZJmv8NIbgQf4Ac5kBZCJW

ZJmANmc6Tu9ZkmZco0gLO6NejAY4EteQNu16VQnXoKDdevXv17oGg3nu4AuQgYe4iBoLiwobIFuJC4Xw0LmkZ9+A/vC7l0BDnkYcAKLoUYQe/5lB6YuMHji54u4Fp+ZIexLkw6oebDmjqr+gcOv60Qm/jw7b+pOiR7suZHpy7x6dQYf68uMxsf5a6p/hNzn+t/uXqDw1/sbTDB9/qyRP+o3IIpWOb/jqhf+P/lMEgEAAZzoyeIAXJ5muEAb05QBO

LrAFlm8AZ6bSBK1ugEoBN1rM5JmyAYs6GBhkG24pmHbr6BmBp6A4ZFeOzvIbEBg7rYHXOFXhQFVeVARO40BdXh9b0BXgZYZMBthiwHPOHXqu6cB7zhu6jmvAWEH8BQ3v84jeUQWN6iBLpjF4SBfKhW5aB8gYoHKBqgeoGaBMgZGYEhugcSEGB1zlgEmBTwRZC8GlgQWZQANgaQHfB5AQu7VetZrV5g2gFJ4FrQTXj4HMBfgWwEruHAUEFcBIQYiE

QA4QcN5I2wgajZrmREp7QYkKcle6yKYKre6HmrfA+7KKzEs+4Iql5u+7lyGTAkE/uSQb35oAaQQ9pD+0pFkE5BXjui4FBWLrB7JGM/vi4QW8/gw6L+VQSv4dB4xl0ENBNLs0HiOpHnv5r+3QUwB8ucxv0Fn+OrJf6GIYwZDSX+dIFMFXEz/tvTCW8wR/7f+XsMsE6sqwWRbrB/sCa7/GL5Lm7hekAZF5VGxbkM4yBIzscFzOyXmcEVuaAc2HXBNI

UYErO9wXl7OG+Aa8F9u7wZdZshuBj8EPWEOFyHnOPIYYbAh1zgwFghQoRCEihWzuwHVmsIeu7cBm7uObIhEQWiHI2R7l6b7BzZvWG4hjYZcHaBhIXoEkh5wWgEUhRIfoFZetwUmZ0hXbuYGEGTIdYFle7IfYHHOi4f8E1ergbyFeY/IT9YLhhBgDZLuoodCHihbhpKE8B24YmC7ucoaN5HuaNpN7fkWNjN442c3idxWKbmhACCQIoIhDMg3VDACr

Au0CkAAQH9IRDAQPEIhBv0F2mcJs2Z3mBrt2qAEsBlgMIvlhzgWSvVjUKzKMlRcq8anVhnAa8trLJUTvghgI8dWDJENYYDLjipKLYo1jVQGPCcC/8D/DiLoycPvqpdiiPibanyMlubYSUltuj4b2mPpzLY+lWnj6R+5Wtj5u2IsuuLfyCfufZJ+YDin4daafgGq4AsWHT5KCSms4yR2LPtGpRa+FO4w8+PKMD7mCNvHVKLgT9rZKbAFfv7JV+4vv

lJSCdfo6JQMJWOWAVqrHK37M4KvpdxVAygBLAigwENFiCQiEK3aBakANDjLAUDFypF2vwNoJ/2YjDFqcqawH8Ax2RWA1h/KkkcQx48CVBDymgMwBRQz2vAHlge+NIt770afYsvaFagfmj4laZkc7Zh+O9lFLe+K0aKINaDkWxwk+HqmT7WUbWlfaVq1Ph5SVRIdjrwP22fiEqBixUiNr2+RfjeKmCEWpgJWyOyklG+MQEqA6S+xamtoC25IslRfe

Cvq+55RiEiCp8KEEBmGw0iQSkZPmKQVaEbUg/hkF00yhLJz1M8nKGwXwnnA8jNsRzBjGnMHEOagBc3bDcx9smTpzrPM4rlFzfMsXNOzAsiXGCzJcHAKlxDQ8EMuwWeiLHsY5cVoOiyzBExLdA4sexsVwWepXGSzlcWkFezUsfMV+Dhwd7A+x7GDXBZ5NcH7FdZtcv7MBxisAHD1waxfXKP65B4/vkGT+rocZbweHoWUFehyHpUHQmaHuw5SOgYUw

B9sjQehb8OjQEy53gNoKgCgwAsPf6ugzEL9Sf69INiT3ozRDiauxNeBwDtQnyPlRfMCeLDCBxWkNkjNEbLhTp2xB/neCOxR/jGGz6AwdqwX+d/tNxfM7gPDCX+kkPgCIAowSSBX4CrpNCaI1zPDDbs5MWRb2xpAF2x1xGcXeBxu3MSOD9s1Fs3HdUuXO3FMAcbqLEXsEsZVwsAjcZlx1BI8eLFwg48YPGkAcbnLGhQk8fv5QAy8fOLcscjjPprxy

cK3E9sZMfk5jccjoMZ3GRcHjoUwS8VOzxc9MaCzzsjQMzGLsAYWnFMA/cVaCHx1ceNBWUlMDjokAgxqgAXq1nuNBEwt+oICGgvOAAlqA7ZhwClOVlOPq4QO8Wv4zxRoHPG5ADsdLGZcyCRVxoJLAAAmSQQCYPAgJ3sFHxWALbPcCWGMCc54ksYsdiiSx8CXACIJdQRvE3x7jkBB9QEwei4ugBgGBb4OqEA/FUsnElFDZ8vVmfg9O8BrsEwB2IWW5

HBU1mSFJe1bh2F1uWge2HTONwb8HdhOAY8GvhzwdmaaIuZoOHMhw4ddbfhGhr8GPW/4dyGARM4TO4ghAod4HgRvgfTFQhAQTCEShcIZuEIhCEXDYohggfuHnBd4G/HdxBBgUDaAoSX/FOguBjeiFAoSdoDhJ1zmWHHQZXCgnjxrAfaAxJcSUuFiO3ALyyqxRAD+wdcGsf+zdcQHCBzysK5gaiPgLCWIl9Okic1DoGysQQZHh6BrkmCsvwUeFlmll

kSAGQzAFnD5Ey2CSDShqwJ2YdJUIF0lss5IAsidg5yLGAIgAyVYZDJTZmWYJJrADQleQ4QJFCLQ3KDMldg0oWMDDJhBsslJJaycwAXaTmIKCxgqsDACzJuQPMnAuP4JiGhmWsYLDZJskDOGSshAOsmREj0NcnShiQPslvJ5gB8nMAGyawBbJxAM0DIgkUA8hQgoJD8lWGFVP8kQAWsUCn9UUfLziCQq/Dsnwppgcub3JUEGUnHulrLwrcEpoTMEw

cMMT35wxWDv36Ix6QWbEox1TLUzoxJzEpyVJXnC2zHMCnD3HjIVyJ2yBcpMSFx7GlMXsbUxE7P/jXxLAXfFMxLMd+BMQ6XHsacxFnl3F5cGCbuyCxFnsLHUWWCWPE4JvMS/6nQssbVzMs0EM+x7GysS1wtJ+ScKyFJXXJKwlJsrGUl6xjoRP7QelDjWGmxpQXQ4WxFQZE7wW6HrUEvxpAI7HBhxOi7FuxRcp7FgQ3sZtgGQfsYtABxpAEHGJxWum

HGGIEcRBC5w0ccMGoQIQImkJxhMEnGtBKcQGn1B6CTBxyO0YSZ45xk3BMEFx2GMXF3+pceXE5wlccboAYtcUFwNxCqYGn7xBAAvGdxm7LlzcpRen3EDxfHNvGv+Z7Ksk6pVLMOkmpSCdQmjxqCVSz9pWuhvFzpOJkwlGpLCROnPxpaS3H8p+AB/G1WVXDByoAJ8cPGiYF8aFBXxcXJKlJcqAA/Gpcu8QEljp5Kfk7fx56X/GX6gCSMHWQhRuGAkJ

ECeQkm0sCblz0JjCaPDapy6WWmZhcwVOmjxYkCkl4JTAH+lEJgGeAlkJUCeelUJCGeLEGwdCfciQZ68dunvsr/uwmX+nCVCD6APCbQ6PpOCYIn3KxHtsHiJ1YXB5Yh4gdInxe8Zm2EKJqia2HKJfGc27OGWAbl64B+Xm+E9uA4YQFDhJAcYmjhHISc7xmzgYCGEG9Xm6aghf1n+GhmkESuFiha4W4kbhUoV4lIRqIfKHRBFbq+lWgqSSElhJtoBE

nOGUSfLzpJ9mfEmLps8SknBJLmcwAOZOmWRnZJrXHkntc1qaKxFJdqb1wEpFSRwBVJEXoW7upHGWWYNJLyc4aWp7XI0mvJSKaMmugPSdrT9JNybwYLJoNiMl9w2WRMm4AUybzhwpBWYimHJqySimbJmoHlm7JNWe5lqAQKacm7c5ycQCXJVWUmYLJHEA8nxmTyelnPQAKYaCfJdUNslzJvBn8mLJamd1z1ZoKY1kQpd3NClwEBAL1mEGCKXNljZK

KeyBopTYBimbZ6Br8B3J34GUlfgBKbhx58yoYCqXuLKBRKahB5uYI6hGcu3ywqZ5vCo98iKrdIfuHflDEHgFKckHUp1obNTIx0nAoBox03KynRZ7KXjG+chMbymDwvab2yCpFnsKkWeoqTFzipd6QlxSpC7BxByp7Mb3HiuyqXqlwZ/MYBDqp1Fpqn862qUhm6phYeiz0sRqeqSmpSse+wWp/LEFnqxoWbamAcEWUQ7geBsd9rOhRQdP4lBvCQoD

lBjDr6nL+/qf6EvpsGbDROxOHimnhpPfJGlMAWkDGk0ZmtAmlJpBaernhxkcZmmkgMcVwhxxeacHGhhbQeGHNxC8ZWkn+cYXnFXgdaUXETBTaeNDMgraRq4sYHaTcxdpHMT2mHpq6Z/GDw38avERhVmSOBh58GUShHJM6XmBR508a1mwAMGUGnjpv8EvGs5KeaPDMJ77LulK5B6STFHpaOSelSxZ6Relnxz6NenE6a6RKl45D6U+lPxxeYElwAx6

ZzrKpP8d+koZBCf+mgJQGVhnqAOGbxhwJRGZ0FQZaeVcnzxGCaewJ506QzmzpfeWhkAZYCaQlXEIGaPnxWtWaPEEZ48RBmT5JGfezBALCeRlhAHCTlZcJNGbP5nE/CUwCMZwiWF59WsWdAHxZTSRgZcZeIXIlXB/GUok/5pwYokiZXYdgFrOWiXgGSZhXnonFeRAUYnDuP4ZV7mJpztQHThSKbOENediWBHRGjiW15QRLiTBHBB8ETDaIRAgZEF+

Jlma/FDpXmXZk+Z8SfmjRJNBb5nxmu+R5k4JNmd5lMFiWf5lc5X7DzkFJfOU8mC55SSIoxZVYXFnsZsAfUncFwHLgapZYgCNma6mWSVndJvSWtBNZtyTtlKFYycwBlZFWU2DHZEAIVkZZLBW1nrJDWVNn5ZSZnsmaFJhcckdZxUBclXJWKdVm4p52dc7DZyWYoXIpE2YogWFvyYineFwKeYXgpkKWtmwpzhUmbbZRWfNnvJ6yftkJQsYEdkRFW2T

ikDZ+KYqFCSGKvhxt+WEa3JAUuKnhEE2PEmcIGwHmiEBjA0IDwAiA9EUcLVYuALUCNAsmsd4fCLERzZsRfgp/ywMU2psoZCc8tqpoMpoAgw/K0PJJFHA1UCFQwCA6vsABCrvmTJLAf4Db4xqFwFThLA1CtpEI+Rtn75zRAfsZFB+bMuZGvylkeH672NkaH5bRRPqLJORImon4+2yfvHLHRKvIHYZwvkaGrh241oFHTKjOBrZryqPPHZRR79vFTRK

KCvpq/A2DMLjT2b0YA5IOyUQcq1+v0YXZ8Rg0ejLAx5dkr53qHcrsJFFUgIhCNAeIGQhw4evm4owyQWggJ/8YAs8ArAQuLhTBi5WHCJoM//OAxQCVvtEqT2AgtgyQMZYBFqKRZMn5jVQ0xYmDayGwLWIXAWkdlrw+uWnpFL2BkWbbMai0cH6b2FkatG4+EfhtF8aNqvZE8CJ9u6pn2FoK5GFq9xZT7X2gdgWCZ+F4tdHaCFYFVTJa/xRVK/8Rfgs

CjaaPGFSJRC2p9FLaNfq+7pR4PBsAlYOYM365RaJTmrQy3BA6xXEu8JDQcgiANgD8cTrKzbhy5cqGX+w4ZSbSRl1aDGVOs12RtIF8tYqkDmyySk1FF2t3ndlkSD2de5PZx0lPr3ub2Y+76hhct9lGhv2QmWjsYZdWgplUZemU50aESJJTe4kiuDYR+RfN74qqvhICFY+AHZBwAb9GcKYABsM4DVAQgLFjVAzQNUBwgGcEIDRYXlISWne9MKxEeKv

AAsB48Ogpso/2UAklSBK5WM8BfchPM8Dg8CVFOCEyrJWFQzAhwLAwl+2sjViC2sxeYoy+zMChorALwMLjnKutlNEG2M0T5L++ZqgtEjipkfHIh+BPjgLcajtndhqldkbH7u2xPlcWk+NxeT53FEmqn5U+gdi3YXRCmn5FvFjPhiVR28YDAq+KLvveKJ21KNQrAlMUZiIYMKVLNoi+H0S4Lul30ZWp1+0AqlSbA5ov6VSCoMVGLkVD6r0D4AtQM0B

8QAELtCEVzRRpIG+GOMjwwiISoVh1YRWJcD92s4NVCbK1WN6WfcdpSlp0UUDOjJpK88n5jz2EpZsX6R9MoZGylUFUtEwVCpYcVKlPPNZGqlUfu0rOqqFTtE9K2pb/LYVbkQaV4VRpV5E8QppYaKhMxDMsAWlNFYbJ0VKAvaW4ia8l+IulQDm6UgOjsj9F9SkDlWDjACYIgyEyKJS36BlYMTuaLS1sHxCJucID54To5oY+BdBKyNVXbED8V2wxQ4Q

DjrZA+gKVCZ5MHNoBeQkeJqCX6AADxRwzQL7k6sPUMECkQUrAageYOToNXOEmoAPmCgkJKQ6EAsILf5EwaAJFDZA9UHgAKAvcMwAGwkUAnCZAiXN2AiA+oAgBHVnAP9oiKqsDRCuY4YOpj/mOOjNX4gMADnpuhPuWTgzkTVdgAtVjsG1W+gHVcwA46S1bUB+Ql+rAFCOOOgACEUNTDUyFp0IEAFuqEKsAAA3OkXNQ0qNTARxFAKgBNM0aAgBE1dY

JeA46ROjjX8kSgNrqcAZJBMFmwUwnFC9BV4FCiX++6nUGZo2QKgDZoUEPjUQQ24AQAvWdCMTWk1RNfwn4A2gBTV9ATkNkArZK0FAA46Yhi4DXgHILQao1P4MjWagoNrLqkAmWNnYcQ+tT87OAjgKoA+GoNpogdVptebVqAnZmEDSoSFgN5m1qqJbX4pROtoADwStTjqE11NUISC19oPrVfM1tX0BK61aPdUOg4tcLXBA8tRXC0g5XCrUoA6Bp7X3

IOOgWDPMvVf7W01CgP34bIl/kHq81S1TgSCmAtRvCQxKyG5jCAg8CTWE12gF0Hz6VdSIAQ12dWjDl1r+O1U8wtdaND11B/vPqd1nVa3V8sx0BXXYAIsE7Xi1ddQ3VkGEdaSAt1NNVBB01VxoYZhAQgP9roGtzP+h2AdkGd6UwTju1Am0LuloDUg9MM3A5wrlBMFeeIccPXUwXNaPCy1V4LXVk1m9ck7glAIPD7i8XTNXUh1YNWHXeOTtUPWB11sA

/WT1z9Q/U46b9V8zv1uoK/hN18jh3W/1LOK/jj191dnWL1udbVU2OW9Ym49QodR4SB1V9QPWUwJNZDX0gdQSA1PMKyA/WX6udRA2mgqwKgBg6RwEPXo1IgGihL6RDTTWOpF8HxBwAg1ZSj+wUKZZb4gkgEkYGof1d+gRhzVUvqg1noAWaQ1dCNDXDVWtTfUQQhNaA3k1lNWg1417dVABGAIDU/WaNyThxB11ikL3DuA84vVAJwSdTkDOA6tZrW8s

OtVeABYUpto1fgrDaQBr6ejRQ3Roo0DQ3npsogw1MNqwNnXcN3Cs6rEpVQFVU1VdVcbEmx51ev7SNrVRwBENXVVkC9VfbANXmkyjWNWoAE1f9UgE01QgCzVwGI+ALV7Ndk26YlMGtWPQYmFtXqAO1VFD7VPCHdUnVZ1YHChQXzFdVQgiAHdWkg1phwBPVrAEjhvV32h9XFNX1T9XxNEjQDUH+STSDUpNiDRDVONsNVBDw1SNYo0o1R4R42Y1NNZd

ll1I9b41E1hjS/UIAVNQvXNQS9QzUt8TNSIBbYbNWfWc1ZDfnVi1/NTo2HNMdaLW81JzVLUy1l4HHWK144DY1q1GtTQAqNX4E41611dYbVIOxtdXW21btZ2Z4NCLRbUO1s9c7VhBrtai1xmLjV7XIgPtX7UXNbdYc0FAwdX7FyNSDY7WR10dWimx1dCNoAJ1agEnVnMEAKnW4Q6dZnVDxRLS/C51GTAXU4IPNbpjPI8+gc3Uw1sHA2T1PddPVwN8

9aK2j1A9ZK0UAvdev791Szec1ytyDei2KtyrYKYoNc9eq0512utqgr1EluvUQApzbgSaAO9fTB71ecJ2Aj5R9ZoAn1EEFCisgNunf5X1GrXfVQABjRLWnNEDROrv10DV8xkteDeHUANezao2v4vrZqBGNZzZA0f1MDeK3f1CDRS1fM1sHq3Z63LUvUNOV4FfU4EeDcvAENHDUs3i1pDSRD31/6JQ3YA1DS/ABN9DYw1SRLDaPBsNdTvnlLNXDVFm

8N/DRLCCNa2SI1iNF8DM0gEgNcDWjwsjeDUKN2QEo0IAqze80E1o0Bo3+tbjVG3eN/6Cc3gNJjT3VmNtLZY3gowLXY2gtXzI42bNK1S41QQK7Ts3npa7TzAiufjXW10NpoI23MNnbQYpKhm0iqEXuJZYErsoN7s9n3ir2VHQfZkAOeaGhr7leZ/ZEgNE2DkmDeq71VeigaijtMjYs0UtENd1UZNfHFk1DVs7agC5N+TZI1FNJTfNWoZgrdmirVik

LU1EA9TZICNNe1aLCHVx1adXnVnTcXrXVvTb3D9Nj1c9UjN4puM0lNUzc6bDtOrEh3JNqTSs0qN6zeJ3bNLbZ437ykbRdlRt6jRu1aN2bRg3XN7Dpf7M19zQwSPNd/t62CtfNRaRRtnzXHVLtvzQ/UAt+LUC2q1h7Q40cQkLXC0iAMLQQqOdpACi321TZsi0u1dte7WnQVLaSDudvnZdme13tUC2EtAde3WktKbWG3/11LSTUmd9LYy3K1mBinXa

AadRnU9VXLUIR01fLXf6F1FTbzVJwIrfO2j1Erd3VKt0rdXWytJXam0dV2rdPWpNgDe3UZtWreV06t8+pm3Vd6DUa1Y6amavVmtFrdvW71lugfXGux9eQCn1rrRfWX+nrTV3etMbWA2U1CbcG1f1IgD/VptsXaSBNdhzcA3rtfreA3LdXzEm2V1KbdbAD16bSsiZtK7Tm1YNPMPm24NSzRq2ENpbSQ3zdVbdG2Xg/jY+1BNTbZG1Xtz3ah2vtBqN

23eQAjVFD9tWUIO2PgQnahAidCzWJ2ntOHbAGB1Snft0qdEXYc03tj9Wj3GNp0KY1sA5jSLVcgVjcoAHt9jWC0nt07X5Bysrjdy1XtkNfo3vdftQ+2BNz7SE1A9YcpkXo2mKtN4JMLcjiqDlmJdXboAIoLgCzA+ABnBwAQgEcK7QZNbUDYAmgDwA8QzdWcKa8m5ezYXebEUmDJAjvJsDqROWLPJC2EGmcCHALFbjgilFYFaLGVwAqKqHAMdisBHA

saoDyfld9BkKHAlwIZU6CMqlZXTRi9rNHSlKPpBVBS0FSyCwVipfBVVa98jVq2RhPsfboVnttcUuRtxcFW4VHkfhVeRLikRV2g9PsppM+UVeVQoyOgi/YMViCibLPiemnVLJU3Kj3YZ27Fa6WcVWVRL48V8JdODm9VFBsA5RwlWVWiVeKkL3Dlw/MyD4A9AN4DYAZwgWAwAFAGcKNAgkN2DYAtQLjjdgJ4ur2tFmvbuVJg1vsvLzgv/KaKtRT3pV

jkiUDBSKVS1wNQqslywFMC44KwD1F1RE6l97mV6ygkD/APxbWKXACtqKWe+4pb72Sl/vXZUylq9o5XylBxXVoR9HlQbabRHAkfZx+Wpc1qeqh0RT6hVJ0YHYcgLxZ1QM+HxVArC4j5YGLDa4Ud9gl9RsnVJgiOkixXpV0JZlXV+3FaxzpR5vZYwJRQlWNKhCyvmJX4RwEKaBsskUMBDYA0WIQAKBMABwA8AHIMwDOAHKLT7L925W0W7lfgn+BUam

WFVRvlY2kb1JAa8iKopUAPL8Dmy6Mmf2oMvwKJENYyPKaDUK5lWcDJAwVNeVaqWOAoPmg6xdZW++tleUr2V//cH1OVofS5XADEUpH04+0fWcUQDwspqXx9p9oFVwDOFX7bFyoyqApPcWfdkkkVaA3n1RqfOG4w19uWOX0PitooEqMVfPoVjzg9vvxWkDORbmoUD2Vc325VtmtALqRVYNQolVAZXtr5RzA1iWqw3YL8AwAb9JIBNFqAyd5t2u5aXx

48hZTlhayOlTSXxg1wIcB5CJwDHZ5C+ftb0y2wYvf1zgPvSBV+9YFdsUQVuxXKX7F4A974IV+PuH0+D20X4OXFCfZhVJ9QVfqWp9/tv6pjKHlIJCRVcQ7aITqSPFA56yX3ukOpqRWINoDapAyYoN9BQ031UDLfXyqBtKGnA7eyqJdUPlVwZVUBjOd4AoWcQphjIG2+0RYoVoBiEJFAFgu3gWAigIoGcJLmCI9BCGGWWSoW5ZBhUYVUIeI8oXjJsO

uVnIglWckXoGxI6JDoGthYtliofhVYbWFOI6GaMj4QPYVdZPWTSOGF+yV4ULZPhetUGFs2eyNDZQo0EVLZsYCtlCNMKRtl8jURclC7ZcRcLWHZmKdNmRFqReDCEprtpE0SAkI3QEisyo5/lXY6duKNlmSIyiNojGI1iMCjuI1oXZZqhYHDqFLhRaNqZ+I+SPsglI9MmujfWfaMcjaeUyNgpBhWyMmjnIyckNIDhd1lOFmo4QZ0jMI2gWxFwKV8ks

jM2QGMSjyYyCnMjIRatmWW4RXGMnZEwBmMdJko6ikJFxAEkWFjEAKdmuFMELqPxy4iknKbmaoaWUahD7uCovZiirqGnmIHV9lnDJco2U2seBlCOeFiY2NbwjJo1aOojPAOiOYj2IyaOdJTo4SN8jCYwxCkj2hboVUj+hauMljByUGNmF0o36OEGYYySOneeGaYWRjZyU2C8j1Y2uMYQKoymOTZx4+gZiji42WPBFso2EUKj1Y0qNnjgReWPopGo5

YUpFZ2fWNdlGNhhG89rKnkXSSuEQt6FREgJoDxAu8KQBv0IoAbCxY3VBwCRQqVNiyn8MAJFAGMClRIAa9IIOBp+C1UJrbgl2srjK79ZYouBVY2/dAyYKywETiTDwto1iQMPxcQzuytVPYIg+qqr/z48RdgDzaC93rD5ilOkT74MiiwwH3zRKwwANrDyFW5XqUoA1H3ziXlfxq+Vew45EHD+0VhVBDKfSEOdaXkWmKRDIrNEO595FUFF844DLd5NY

jw/gPRRfPj1FzADWHQMAO82lBN7KX0YUO/DxQ39En9F5VWCd9DA7sqBY4AHZQeUDyDfFUoIWNACSQ2QESRSS2wAwAfJFAKf4ODiPvbC5TzIIMAQAWnXFCNAfQPoDE9BqrYNSlIHXc3FTpU1lN/9qPkpNpTRU7kAlTWQD+ZAD0foVM1TrU6VPlT7lSqXVTLNb1NZA/U5pMx9zUz1NQAbU/oBpwkA2hVDTW2DNO8NGFQZPdTw09NOlTEKV+0yKi07V

PtTTYx7STTG0zNNcEf7ZyLrTS031P7qHKFMJHql6rBKXT+0/oCPm56seoeUd01QDHTV01kDnqr1iBoI4BU8wBcwsOkxHZi0PGAKmC2CuDy1iZomlPAzTRtFiE4JwMUKmCuWObIsTRWLTgQAvkAYBRDpJJQScUpvXlgTA0Yk9MjTs0wjgKCPlflQFTRICQCnuNKLqUMzfQDSTC2aU/TPEAd7AgAXa5WcEAcVzM0ZH4z5QKrCIgV3CRB4gOOqlTdYv

ABVRfM0s1A1gCROkyBAQygObASUEs7gBSzeWPLM6z40UG1KzEAGTMtT5KNpQg9G8HMKp9QEH6Ak9ws5AA5AfM4zifDIHUQBszO3NBMxoPZXqAxQfZT2VkzQ3Ta0cg+kHADczvM5oD8z9fR5R54GKYiB2z/kXpSZA36FKjFQBgP9OK+oI5NLX5IsMEBJzGVS1SuGIBi9ZxE+AOHyRTvfS4NPKMrCAABYQAA==
```
%%