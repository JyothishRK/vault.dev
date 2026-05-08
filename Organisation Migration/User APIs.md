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
N4KAkARALgngDgUwgLgAQQQDwMYEMA2AlgCYBOuA7hADTgQBuCpAzoQPYB2KqATLZMzYBXUtiRoIACyhQ4zZAHoFAc0JRJQgEYA6bGwC2CgF7N6hbEcK4OCtptbErHALRY8RMpWdx8Q1TdIEfARcZgRmBShcZQUebQBGABZtAGYaOiCEfQQOKGZuAG1wMFAwMogSbggATgAtYmqASQAxeiF0sshYRCqoLCgO8sxuZxSAVgA2bQmADgn46oB2HgmU

xYmABkn+cpgRlMSZ1LGDjeqNi8TqxL5iyAoSdW5Fw53ISQRCZWluePixjZvCDWZTBbiAu4QZhQUhsADWCAAwmx8GxSFUAMTxBDY7GDSCaXDYOHKWFCDjEZGo9ESGHWZhwXCBXL4iAAM0I+HwAGVYGCJIIPKzobCEQB1R6Sbi3TpQmHwhC8mD89CCypAsnfDjhfJoeJAtiM7BqPZ6i5A0nCOCNYi61AFAC6QLZ5GyNu4HCEXKBhApWCquA2rLJFO1

zDtnu9kLCCGI3GqKxuMzGizGQMYLHYXDQKfTTFYnAAcpwxL9lolEuNFjMUj7mAARTJ9ONoNkEMJAzTCCkAUWC2VydsdQKEcGIuGbZerPB4iXiiw28R4MyBRA4cI9Xvwq7YxNj3Db+A7kL6mAGEkRgQnCFQAFUwiwADouZyvt/v9/P5/xbSoABKCAAI5COEUCoM+qCQVBkG4L+t5/gAMuBHDQahaGoIQv4KLgcCEAo9DxLYpDKNYhBGBOWYKEID4R

BB6E6Kg2TqGwxB0eh6GYagAAKADy3IACpsah2C/h8uDEPmQnsdBnFQAqHBSVBxC/l2xAwMh0kcb+HC4NkimaYQnFZLgnL6dJhmcQAVnY/FqMEZnsYQ9C/lAhDZEYnAIA56HOagsLBMwz5xKgACCQjqDkrl4K5nBmbBqByQiKHYB8xIafRv5osojHUWBmg3kSrmMM+KS/mFzGkGRFGxSh6HxbCbBQNhxD6L6qDUUwChmQxcBsBQTDtQ+6WaTJv5cU

wrXhlmaD6NY0Q3vekkcMkqAAGoECQ1UcIFtVofFOl6btI2QZxAHAYQgSsUd0EMcZpnXSNp1AUIF2xt5aGGXBHCEMBXkPQZn1rRtxCoHd+CoOJZA6mZIl+Si4TvahnH6LloOYIQ0KIzJp3w8wOXQqg+UQ9gRV/WMZUk1mO3sfF2BXn0d5DZFpAwN1v7ckwVhEEYN4w+zOQgwBDKcGEz5TKgABC2WFVmcW/gh5g5GEAD63Q3u42BeltbPtVA2DK2wb

JsmEYEa1rMUcNofPcaEzAUGiIPKDkTDa/9qDKYzA3OP+8OMThuEcNlbWLaQyt/vDysALI4agbJohDXJwwFqBrgiV3sQgv48RwXZMo4geg7NnIJWwg1MGZbIU0VJqoAhbDZXHpBl03dMIAzbVZaR5EW8rMtmLAyuosoO2LL+Qu9dtf3BpQ/H9FUl4hAzIc7R+q+fgpHA/v+z2gcNqHxfBSFY1BnHYbh+GEZ333d5RHUsF1bsMUxkgscfJ1jXxglu7

DYkSU+buORcvJMyHtVLqTfhhbSuk/rHWxr+MGECLK/mspoWyUB7IAI4r5Vy7lPIQN8v5BGHBgrlQ+LkcwrsaZAKSqgFKCA0o6yyvjPKBVKbFQ4KVUK4UX6VRvjVKhcNGrNVaihO+D92I9T6gNO+e9jqcXGqQSaBYOAzTmk7T2/8VrrQ8FtamdUoGHVgSfMez1XrpwyoXEy+BEEmPOpdRBnFbzfV+g4zi2iSCWOLpDQI4ZraEL0UY5GqMsAYygA4s

euNmGE1YaTZ85NQqUxFnLWh9MFpM1yCzHWHNKobR5rIqCsMOYUm3sLSeYtfxS2JhbZJCsxCT1VvAdWBBNb4EoRY8K+tDbGzbrQ5p5ssxW2/mNW29tSCO2duQapbsPYh1QN7cOwQ/ZwADkHFCIcw4R2jnAWO8cCDg38SnX0aczKZ1QNnXOYzfTZTBiXZuFcq5ORrnXBu8cZGt3bihK+VUe59xNIPeuI8TGlNFlwZ0nAoDcjIuIXgEJZRx1yM0XSnJ

TSoBXCefoIUiDKGzOgYIbIBh5lIFFAgmKvg4ugIaVkehcgmW1KQd0aBIzbkhGiL4voCCzzPPPVJGiV5rzXl+TetiQIE2SYffJBksI4TwgRIiJFr5bSojRcRFjn6v0wR9D+Alra/yWkY9+JckogJUixcBGqkYGJgfqwGCDzXYysjZOyVrAnYLcggDy2p8G/n8UFMq3DIoUKmQIxKORaGpThBKwmmViJRKJn3P6nDSGsr4RvARDUmriREc3FVaFJH9

SbjImx3EJoY2UaonS6jl7Pi0cDXRySDrOtgU9Oxb1H7wKLtYu1xjt4tvMYExxziQKuN/O4kGNzvHQyGUnIh+rIFRJCZjLtJ0cbJxRgTONbCyYPKSW7WmPLZnM1Zm21A2SuZQsjYUgWJSJ4gvFpUmW/D9G10VvUtWvT8AtLabm38HSDZGxNu+z9FtBnsVhlxEZDtUBOzpV+1CMyhrzN9rNZZVyMJrIfBs4IUcY6NwTvsyJqdW0ZyzjnNgedUM3Lkn

ct2lcEnV1gLXeuOyC1DXeTeDuxEu5bV7mwv5Q9AXXpFlPIEuBwpsCFlC7gMIQKrl9AgAAEp8b455UA/h4GMYoABfHYpRyiVAkIBNkf56wAE0ADSEwxSsjVr0OeQJhhoDnHEFMNZJjVBSMmZYaLZQotGCkKYGweAvBmOsRcswgQPGIE8NAqZqhAg+F8H4ZpzSQhBCqWF5QRQKipGiTEuIcRIE7ESEkIZKQoly7Scg21GTMgJZCDkXIlQqihCidU0Z

5TiklNKIEWWERNehS1oUGphBah1L8A0RoTS/BS7KS0o4bRDidPV10CAGWoCZT6P0Dn0C4HiMGbsxAwwRi3D1hA+40CzBnDMHgZxayQgzMo7gNxCXKOLBwUsaAUjzjGC51YdZGzBEnK2dsCBOwHb7FkSKi2RxjmvC2VTixpyznnIuZcsn1ybijLKVEe54eHmPLKU8KmIA9kcGBKtL5+UfkFVvM6IqwJisQpGwBqAz4ysvpxhVFslX5hzahJ+bcX59

qbWNW8X9QOiRCH/AJIvDU5GNYTU1zOtLregeE1AKDVZOuHQlN1HrG1yIIbjX1XCIrkOirLXd1DQ10IYcepha6WFVKcgmv1FVvmW7TWwIRma2piJ1r1fNzdleauLYo0t00/YVrSUtGtOiLYy+gvtNXS65107MTrH1qegkEwXWE7PK7wixpiS7uJ27tpith0vdJMIj0SP5jk7mvMp1FMFuEG9f073S0Samp9tSlYIAaYgQD/TH3ft1p0/9PSzatOA9

bEOCybxbJWe1KvWZmN4YhsQCSINrAg0CPoNgjB9+4xA+hD2IVeMMeeRvmRsY1BodQF8lNPH6MwH+cPIt/RyDyFQPQAgIq0SjcN4o444fQAA/M+KPIJmUqCpCOQBQJysTqTg/hTlTqvDTsKrvIzkfNnlKufLKs/oqnfLRMemqsLnIqLuLuhD/FLnqtatbr3mhKAkrkWg2urprq5Oggbo9IDDgu6ngtnkbgFCbqQgGhbmPvvIwWGvQhGowjGo7tEs7

uwomtwsmrBknt6t7hmi1H7g+HzjdL+IHtIkNEWgokopHrNNHrytWiOrWgnvWinrOs2i9PYselns4b+IoXnurovnjIoRurEhwPEpfg4VbneFXjHk3IelkpzLks3hLieleuPEJuUpLN3kGn3i+irG+jPhoVBAxL+l0gBnkXPlOgvr7MvqhqAVtBvnslvjvhDMUgfkfrGNOswGfswQ8v3OpDfrhnfmTo/kQT8lfu/vxl/meD/mgP/r4EXvlMAavmAQg

JARwNASkbAayPChCpJmgDdmCgikivgCit5uUETqStilUHinVrKBmMSvgOceSnJHAFSuCrSkwGthtiypVP4BynPBICgeTjRM+OgdThvLTjvKKuEeKkWmzhfHKlxtziQQYQUV4YLuqp4dxGLjqnQf/BiSGkwXBiampCHhaqroYtag6poFrtwTrvwfrogsIUQiQv6ubvkTBNIbbnIfbgoajIEaXhwm7jwh7pIZoYIjoVmv7sesYSxgNGYSWlNJwOWvN

DYctHYfHlTI4eSYEsKhnu4cbngfOujIus4YXv4bySXuwiET3onlBAfJERoqDBknXhYqevERevzMUusbehUhkZ7lkXUjkY0iPrPn6ePkUVPqbH0iGZwJ0cJHBA+IvqgFUQXDURbHUYnJDG0Xvn5FkK0SfgFLGdBBfqMYxi8jKdEYMRxvKsKRwK/o8gPOMQad/qENMQAXMQgAsamcsVAUCh3s+KyKJnJBJjzFJqQDJpCGuApkpklqptoOplpjppCPp

ugF8FAARLgD2M8UCDZrSHZpCNtvEGcOTDwPEOMPEBMLMAuFcBMECL5qcNoDMCFocCcFsDMAsDKOUJFtFqgACDeZCAlsptwCmGmKloHOlj1h1kiOVjSOgFiAVniEVsSHNhSDljBdAFVgyEyJFJsZyDyHyANmqHGBBaKAgBKFFlKLscRQqP1lUIRftn4JIEduNiypNrANNhlpAHNtaLaIUEtnCith8SdkuVtgGDwPRaGGNoykJbKDGPDosF9tUBMJW

DwHdjcfmFmL8H+WpZmEWCWNCkkACIuFsBsKpXpg2E2OdrHCDmDuSMQBDgOHkLxTDksfDvOEjnOAuEuCcZAKnJjsytjruGnAeNZeilyhIAAOI9KYrgxoEglviYE9r06RoHxM4wnSpwnDG3zKo6zkFFrhU9jUFoS0HiT0Haly4bzMlm7EqZF7QcnhqRoMQO7mnKGu6m5CkprJLprCJ6GdQB5SLlkkmjRh4WGKlR7KkU4rShEanhHNBtwpTB4SRRCcg

2mQQMSulN7ulJGent6pHBG9m7XTyIF/HoCRVgTRUqlxXxVglYGQkCLQkGmwmEGc41k873w5VokUGPS/j5WFVxmoC6q4kMHlWiEsnVWhlSHlUyF2715P48nroWmtVJq8JskQxaE+66GiL6F9VB6FoGnmER6jVWHjVAmql0bTUCKzV6ySALVtxWIrVRpJGN7nrWyt4wEgrxJenCb1bgqQojm7EcXsjgqIqtRHHcDeXQAYpYrkpXGsi3EUL3FS29CUp

AjUpLV0qCVY7lCso/H4BIFVCnWhSJyxWXUJV07YFQmpUPXpVPXVkpqvWkEw25UGk/XYklWA1lX4kg1VWBrg2in4lQ1ckw1NXw0tUlSCnqE1UQ1dW+6Y29VSn9XB5ynh4KkqJjWVok2TXWnJKU3zUyKLV02xFM15Is3JE7WwEc1l0goDlibDnQrSag4TlyaKaJYqZqYaZlDabFC6aQDLkQAACK+AXEXEYwt4kg4V1mjStmXK9mvwNY1Q0wIWj5Cw8

lHmWluw+wYw89IWh5128Qj5cwiw+okIX5FFqAqw4tAFM5CYIFsoaW0KAtvWUF1IeW8FhWkIhISFpWqFvQGFNW2FzouFNFAorWRF7WJFZF35H5AgkFQDqoID9Fo24YzFsohoRIU2yWFoZI3F0Oy20CGt/lemIlEguAaQw2tlTFUlmtAgZ28ON2Cw125YN95QD2Gln2FYL2WYb2H2qmKQBwh51Y/w/2FleOIVsoXYtl9lUOTlkIXZrliO12yOnlaOj

dGOlDBDPlgVll+ODdhOx1EAz4BtsyjYS1R4wJl1zgptEJDOFtuBGJj1HOttxByqyAJACg4BgQxoiAAAvO9cxJ9ZKqgC7VOgDfTSzp7cQoKeISjfFP7ZyQ1dGtlAEQjWHW1RHb7baWjeKT1aQMiatUYQnbjRifjSnUqenbHuzKOL1ESm0QBJ4+EMgGKkNJHL6G5AQKgMY3TQ0+at7EIPoP46hN7B08tdQO0SM5HM0CFCelEFANRCM9yNyDxGhnHDr

LeOFaE1BN7MoP09BN7LMlxe7LTctdbH+IiOs5BN7KINsxs2zYQJoJyDXHUn0E3PnUc9Mr+JHCxF6DOiNN7PoGc3MqgNCBOL/sALQgYD4E4GIKgGgMANpqgL1EQNgDADC5pqgJpmXmTTuhTXNdTXnYc0eITKEG0ZwDmXU4XWesXS3qXcCp3vtRsRqDPHowYz0kY/i3yibddYlebXdZbXY9bQ4wiVlbzi48QG4x44QN4740LnlQVa7dLkWuE5VWQmD

SKRk5DXE/IYk81fGik0jTWZ1dod1bHTk9jSYbKXjfKWWmnVETtCtNyJU2iM2NvHU/II0wNM099LNODEM0eF08dD0304jIM2yyM/4mMxM1MxOLMyegs0s2wCs2s0G1Blc+cw6fsy86Y1Oic/8xc9gCmwC16bc/cwxo8wNBm/TR7B88QF8zm4xLW0C3kDC2C/oBC9YFCyiyMwi+YMi6gLC2ixi1NVi0+jnbi0NOW4S2ECDCS+K4gOS26SXdtTS2kZz

f2fsdsXzTCmu8LcimLduZLWSpcR2dccw0wHcQ8UrVuZCKrW8fSvDp8Sg98eyrrUyxwIY0ND6+y3FZY79LdU+vdXywQQK1zkK/fCK2K/QhKwgD42QR9TK79dBMVfKwaYq5E6yZHX7fJAHfE7DVqyHTqwKak8jeh2q9HRjdmqawNUnSNanUTWU5ohU8so6zUxB4gK61CU0y016+02y36z8+1IG909xyY8wKG7jOG5M7yFGyJzG4s76Ms8eqs7W1s0m

3s1gwc8J8c6c0m5c0m4W3c0QCW5FGW2ywrlWzW0m380mw2yC8262+9jeB2/C61kiyi/23tZixXjNTizTcJxO8SyhDOzwXk4zRSwkTQR6W3kuxwOLCu3AbfTXeEDsQlGOTo+UJOc3YBXqHOe3WAJ3WUN3RUPDhABQJgKQCFLeE0KtBPT0LudPfuc8BcNoIcDWHvecCmGsKZZAL5kpT+IkBsI+V5pvZMIjhFl1jFlsHOYmH1zcIpSFkw+8NOa3dWNo

GcMuMpYpSsKtyJmBffVRQiN/RIHBflqyB/SVgdgd+gHSNVlhSyAA41vhbRfA3t6RWN7wM97A4Nm1rKJqIxZJaphNmg2xRg5CFxQttI/xXg3e9JYQxJNtsCIkOJYdn9/e5ljQ9KCFtUOeS8HFvdupZwNKIeRw7pfZ0BWMP8DMEZZ1xUOZYDlo6I+UOI72P2FI2gMODI5EXI+5Sjl5ejhuGozuLjsFUeKl10Ho7MiFFxI0PkAy0dWFegOL5L9L9zbk

LzfpTcE1y8EFokKsAuP1/JVu4cccXu2eOexIDLYSme4rbSMrVe68XJre35QaI+zpM+3LxAAr1L9XUOYlxu/XejlOS3b8NlwuV3UuUV2wIiIsKtD2IkKtMrHADpIGCZvgPoM4CFJoEIO0NuZPWb3JqyNthefPeeXOBWBMNUNUNdtr7eSMIcKPBsJsCFoXykDwLN6N+Rc8DMMkCcJWCZeX5vZj1A1IIt9wKVNWOsCsEuGMH14sOX0pdt6CLt2A9ltB

S/cd4hWd7ZRd+hfSH/bd/VoAw98A0Nkv51u35RSf4qIf3A8f99yNr90g3qAD8aED6pjNuUKDzxaz3xeUC6JD478JbDwDATBEeFDXtncC6A59eAdwPLqj0so3ZFGh5E4ETxxTnlB+LDYntwyx5LgzgCwI+rKAxgA424dPYXjZSZ6Q5Bw4PcoLIynAKMPKqOcWr5X54TlNGIjEgZCF6i+hHKrPcAUUE6BlAOK/A8Ad/zKC8C+B1YLvgcBSC99qg/fJ

cG8DKCjBtAY/CYBP3UzT9Z+iQIQXcGEEQBWk0IZEPoFagyBYwXENgJwP/4yUogRKCWL6HzjKALB5QAWDYIpBXIHB0DJkFABCikBYQFAX+G4IwAUgvBPgvwUwNlAIsYAygfHsDmF4h98uYfKoJZEsiJAAAUsoH4gAANZgJgBSCYB0hkgIwIkFIBwhqg4VP8MMGz41d0AHjKIDt3z418xgRwc4BsDnAJgTKl5Knt1xmDz1Me8levpvSkEqDFgbfb8l

9mXBKCTyyYKsP1yCzzch+gfPUBeVKg/YZw8jN8nMAaHz9wKF/LfkdwKwndisyFMrM/Uqw78bux7SAA1jwrKgCKT3C/hA1PqD9H6H3OimQ0QZ2g8BWtViiikPIC0P+ODCHm6Ch5UMKgRDHbIsBAF/du6EAyoSkGgGnZLKF5CYRcHGDIDpQiQIYbjx0ocAuGavJYKmG15fYhGtPVgQTgZ7g5meFAr/s5Thw0CZwdAnnioz57rZoeGjQXtELCCxCSg8

QiQGZh4CAQ4AEwNkHCFvDVcBsROOoWgFGBT9UgfwdYEmCWC3ZEg1fPUFcDiC8MFwiw/4JsD3rDDT655WYZfVbpv9IAd9cEM9x2H5YEK79A4V/RX4nDrutWHCvd2uGPcb+mWSCvcO6wX9nhtw2/gxVAEfDIAqDZ/t8ONEQA/hlAi4QJSBHqMQRgA4hjMAhEP9mRwI2SgT2XDzhy+GI7So9j1BIDMRr2PSr8BTAVhzgmPHHvgJp5ECSRIvCAIzzsoU

iuB9oHQdQL1DyM6R3PFvrz38E44gq7I2seKIvD7oSa5jCxhvEOp60hxi8G1mYw5artle67aFHsQXHbtRaaAcWmcSt64oj2stU9vLVN6Xcbesoa9vb3wZO82ULvScegAXjXgLq5jQVF73Ew+866KXf3hlxnJt1ORBXXupWD/DuZMAFAEKKKKnrnCIA22NYI0NmAl94BuvTvsqNUwAgfwbQsviWPcx9cKxn5V7vqPizD9get9WoWgAfqQULRr9fYZ/

XO52jLuv9M4U6KuHNYXhdw17o8JgZX9PuoDP0W8OQafDAeoY34VgzB5UjcGgI/wbYP9DENqgSY47KmLR7riUwE/F4IGIYB48UBZwZATiKezN9TyawEykSOrFC9SRBIckeQKbFs9ZQrYhHFzyUYYSfKRyHsSwP0kDi9GAJO8XOLi7lAECV4knIMWNpftxxa7VXtKAFpbFVxRvUKp4K3G6CdxFvfcRFKeIvEaUp4mMeeJ1qeTnJPkkEg+JEwJdWAvv

V8SowD6ZdZy85DuouXwFFd4guQfiNgEAj0ByhJ4SARLTq6ygDyh5JCYFgmBk9LsHmOQZCG+GITtA1wSQQ0PH6nlZhJ9TSgaNwmv8BapowieaMokQBdhVosRjaIonHCqJpwx0Xdzok3C3R0DcBkxPe6sSGJHE+/u8Kf7oNppmDK0AJObHOhoxIk0EcCCAlkMJKyYlHtQzgH18UgMgk8nOFRFmhxa6A7EUWL1CDSuhvDMnrpKBxWU2BYjIyQ5X+FUC

OetIxRqjmsm6DbJoQtLvZP7HG9icBtc6ulPQKZT4CjLN3oTKNojj7xfkhcQFP5oG8RaoU3Ribwinm9MRlvA9tb0vbHi7e6tJKV8QvG/EKZUVKmUtFHGkz4u3vHKS+PHLY4m6U0z8SVND5lSqg8mCYCZh4hwg64mgYCbV1AkHlO+pUBYACCn79cxgKlDGd8MOBxBrs12NYJWCkEnBFJ40tsWvQW7zDVM6mTYYv0sEkUSJa/a0eRM34LSrumFLafv2

dH0TfR7og6Wfze7ejjpMcyAD9wDEXSX+Pw66fNk/53ShJq2AWfgKem4AJYkk/wWmLQDnBDgP0wLIpOBm/A/gak0GaplPImya5swggcIwcmkCGxxkpGZAHMluVaBnYjGYwJTGxjexxAgyY1IJkst32JnSnL5MlnuTyZM8wEgNA/azjF5tMuFDzSS7Lid5BxJmbuzCkHjIp+KXcUShilczDxPM8oCeP5n+DtaT7TyW+3XnzyJZG8ecVLKfEyzRycst

LgrM9lKzcupUvTOVOIApBCwCAHiFAD/B6zLue5ZqdwE2DJAOuNYRcJmJOCzDvhe9UeKeQ6k3Yuh1YDGa7Nf571pgKC5EecBUGTSgFy3VbkbJb4qDNgCYH2WaO2ELSlpb9FacHJQqhzqJEcuFAfxdFH8vuschUJ6PP5+zqKScvaRAFTl/dFJwYy6ZnJB78Sc5pkn/g9Oxk90i5iIUuToqhAySvZlYK4F0NzAFjWGqAcYBjOBnqS9QfwWcDcBn6KSO

5xIrue/QRks9c5ZklGW2MsnozuxhiieTWPxlVAJejQNmuEAnF6MIlUSpXgfMXG/B1e6I5xQSNgn68VxhvY+SzPCnXyz5oEuWu4FPlxSVafM94gXK1rO9hZxOOJZzQSXlBByP8pLn73ynvjW6wfZWXENVkSBFgxoZIZHDhCFgOYEwMzGwAQAbB6A+AZENyFvA9h4FmM7UBKJ/LyUVu15H7AP2aGqTepvwPhk1075k8NRc4eYLqOmyI45y5ipSt9NP

KJBaFhU0frw1VGr0rggWJYO7OBAETUARE/2ZwstHcKGeq0kOetO34Oj/6kcnaa6LEX7SJFh0xOSIuv5Qr5Fd/NOSxR4nsUs52DSMeyG0VjzNs8YnbPWAMVgC+B0ASATwDhHRhjFJfQLCmEyU5irFfDRuSTzNBXAbgmo6GZPNrH1jJGlInxcjJcqoz6RXYxkXZLZGwyp5HA3laIJEHgCwAAguVUIPkHSqyg/DUeMuBkFXLVgNy+bgoIeUHAEwzy84

C33WBaDOgOgvQVAAMFGDmwpg8wYYqBbWDbBrgwxU4KdWBwy5Vgzwd4L6ghDcVkIAWEEJ9VS5/B4QyITim0ZfjuR6AfiBsDMFNALg6QuEJgFWiRweIfdKOAgEWD4ApQFQgbNUNNEz1JRtytUQcvtkr1Wh2C+uWTwfIqU96mwdTJvXYbH1XuUgpYNoB+z+Yeh8wLYEqP/KKyqwxwH7JvXWAKSawbCuaRwuBVcKyJG/PhcCrDm79QJlwn0XIsfqSKE5

0ivrLIsRUKLkxSir4eirUU3SNFOg3/sJMMWiS4eG5IlVCNJUwiKVMlYxUNwr4t8uhAM6xc9ksUYClxSQRSvX0EZLkqxMM7Rt3J5UmSWxfiiyUPKslBK/VAVMVRGq6Vciel6AYgI0GzjVBlA9YNydCLFGIKhgI/Eyj+F/J/ATKN2abvBOcAEiBp4/Q4Kyq2XrBTlF2DzDhKAXjAZpny75cvynV/KZ1hwrfguponbSV1iKtdbCs3WX94VbEhBmdK4l

BiD1eE9/uor7nYq/+F6ouc0CJUfSjFllF4IFmXAWze19KqITwx0lfqQZzK1AEsCn5k8fpGMtxXpLxmeKJGjYlTQPPbFoylwI8rGXBpxkIb6eovN3rMlbjXgumoJZ8D2ApCSqoAaAXiAJAhiAd4SwHTgPbUFTNBOQRebMkPHMCEwrwcIYgH1BUSbzBUAAKjOTPU7aIcZEBkhRDBBSA2gSyHygi2YAqmYEcgmFpSTTiQ4AACkCCARQ24QAAJRdNnwz

yeWoxA+odaQtfQHrXfBGZgFcAw2oSN7FHQo1vYDaNAOnjcLsRvYYMTbaYkugjMZiJAMyBc3hhoBuQL8L0GOiNJQARmzAK7fgBBh8l2E7EZbagBvEMw785CWAO9sTKlFwa3sPuiBBAir4N8p3Q4QlFCBwgZc3sdauemzJhBmildP6M+DK3pUHwZgMQBEB4jPVmAW0bJFjsziNazGz4MUGiARBNx2twJeFrCGx3MAqtqSbrfH1wDKgyMxAJbUdBW32

Fpop21AJFuYAiAQCrGVJMLh6YvRxkMGNMgADI0MD4NMt1rG3YAcyvaQbe9tmTOAAAfD7EWTIYV83Wu+MrEITKxkMcuvcZwDV1c67w/EREE/gjLBlaiiu+uDlr62uFYwlunZjbHDCjJJdLsBXUrpV1u6OdALaOEUIhh4x+IPEZwPWB4jq6ho5yMjJcgLg3JvYoeiNKEFQCR7o9seq3VNR6IpwmMvoeXZbkFSc1AW0mEmELrC1XVnwwAISI+EK4N7P

sM/JUfXogAS6m94ECAI0HrCIhEQ3IRIDGuXCFh6wDe6gG3sz4nbuADenvX3oH1D6ZgI+sfW3vYLT6IAZmBAOpBCgY1l9R0BvQgjX0x0AAAjCC+BOxqm0IK5LoAMC77oIDelBGgnsiO829iLAfPxEaSd6G90pW/VBAb2tMnY4GdQJ/v0Y0A29DbaiMAZ1agG99ug0IFAHAze6HYt4DniFDCRr74BEwZwM0OcAt8Y11QZAFIOQAXBagDe58JphiVBa

Rd046vTXo4CRbiA0W2LZ/AS3s4ktL1Egmloy14wstzu5XZoDy0FaKARWheaVvK2ONucVW8FP5Dq0NamtdBlrY6wm1+MptqSHrX1oG3MBOdo23g202p0uBOt14WbQ+Hm0ThFtvHVNqtuI6psNtiVXUjts8T4B9tvaI7cDD52EILtj2m7aEnu2eGlC+HN7Vbs+3C6BoAaX7Vbv+1RlYMQOkHSAW2S4YIdpWKHcwBh3vb4deSRHVeh8R9kN46O8+Jjs

Vg468dBOpgETtkOk6OA5OooQND0Pew4AdOnUIzunHM7GQbO8SJzs92WHedbsb2ALqF3B5ptRGa5pPt92TJ18suovebpQhO7VAyu13WYg93XMNd2uxMnrtQwG6MMxu03RMcvkW71dNuu3d0kjIfpR8UxgPXMcugLHU2CBu2JBmgx+7180xl3Qdvd0h6mQ6eiPVHpj1x6BoCe8jMno7SvGw9GerPV8dz0lkh4ZunY5/I3hl6RQQgSvYEBoNjja9beq

fc3uuDj6YDHetfbPv72D7Asi+0fdAbv3t7sTaAGfb3rxML6l9xJ3/RAFX3kn19m+0KDvtpOQR99HaSAxjRP3fFz9fQS/YHGv36Af97JiAA/u1zP6YDr+yeO/tnZr7v9bJrvf/oQCAHJAwBkU13vAOBRD9m6DUw3otXXGfdyBlyqgeAMYGsDiQHA9UDwMEGNgRBjYCQf0YcByD/kveUFKFrZL1x+M0+ezO0qcyLi3M+KWrQqWPzqlrvYnMFtSRInk

TdBqLWYNyBMH4t/LNg3bQ4Mbx0tycHgzMdy0hB8thWpE6Idx3iHKIkhmrVyCYBlGXwzW1rUoaFwqGutD4XrUBA0NaGOASu3Q5Npp0DGjDTAEw1EHaPXNOjqraw9AicNB6+de22w4dr/yuHuj06Dw8ICe1oxvDgLXwy9qC57xvYQR4PKEedKpsIjxx6MgSWiMIBQdo4cHYCuKRRBkjsOkLm6QyPI6ouaOlgwoHyPY6FARZ0iPjotiE7FYlZ18GTop

3VHOz+huo7uAaMPggjzR1naiDaPmGAWQ54Q/Yd6OBB+jou97cMagwTJai4xyeFCdQCPHZjzxjnd8abha6ddN4VYwXHWNMAjdEcLY3hYtiXGAWYuW3cUWnyRH/dvBwPfMfe2Gnbj2Fri9mfOMvHU9bx8PZns+M57PdsyX40nuuQAmxLQJj49nve156a4EJ7Y1MlL0o7y9Y5BEwgALMbw69MBtE9Ypb2YmSTZJrvbifn0EmaTlluk8MeAO2X8Tw+ok

45dFMMmu9G+rfayc8td6D9jJ4/afuUB8nQIV+vQMKcVP37HUNJSUySelNhBZTXkeU/1T1MVBZoABicGqbX0ZWtTkB3UzFdgPQh+LYyY03DlNPoH2pFpq0zacIPEHSDzpx8bXT/m1j0uiszpSApVlgKqgq0eICFHyqrR5MuAXABMDYDyZ6ACEEzBsELDJCeAPYZwAsvzW1DC11ixcPPTOAMMJg8lWcDP0rU5gy+c5IjbcpLGBYWNza+OQ0Pno7WTy

SQLHuRuzHlBDRI/fzO2pOAz9mhZPd8lT1mlfL5pPG0iev3438LNpYKoRVHN2miaPR4m8RVuqk0nT3JyKxRenN4kYrbpmiqMWpt826L8VwIceq9KR7Jjb1O5axQ+tgGuVmuZPZoXBPM3JKqedipuXdeUpJB0RHK0Jc5rIGIysV7mgJV5tg3aaQlDkyNShogBsATMzQRIH+ELCEAeICy8UWtY8zLdlwP01W/AMUm+Zteo8K4PdY664LXgl178nJNY2

FSlwqYcdf9cnUVZYKvG4G7aPnUCLwbP/YRdHNXUw345zEkiiJvYlI3/RKN1FSGMPWzZlNWKs9fnMel43cA8mLTSyJ01yVMeXQjHu8rrkqi315m+xapjWHjADgM/dmx4vhkube5PNyDYPI7EwaRVwS3GeKscmUHpEkRaM4KnoOMHMS/EF8zbUFYpa0zz4DM5luKTZa+DAh/M8VpyNiGO7NgUs/KHLP1aSdVZ+QzWZqOLFDDjZ9QzmU0PwWtz9VQgG

yB3O3ado2hmYx2eUM06uyPZ0gH2bMPvbELfOmw1tsGNoQztwQBc9duXPQgfDi557ckzdjq77S32rgnuYLa+wAdkhE82ebiPxwEjB2JIykat1pGCoxSJHfvhR2iGMdJRgox+aKM/nUHYgf82OMqOU7azLEDrWBfp0hwKrfQaC60Y53r2gY6pRUnzpQvBGW46Fq3UsYotLJ9dhuzYzHC0tZhmLuzfY+xaONAYHjZx4i3w7Jr57NLjFkvTCd0twmDLR

llE6ZdYgj8LLE+syxSbn1uXCTGV5yzicpN2X3LGV7yw3t8ssmREGVoK13pCu8nT2EVwU1FYyvin4r/PF/dkQQApXgDCpgK3/qysqmcr6p4qwVZ1OxJirBpiDOVZQNoHGT5p7A7gbOC2n7TjpsgxQYjNDQuyDdjeE3fjMxaW7bdoDuwZJrd2uDTREGP3ZzO4A8zQhxRxwDK2fnkt49yC1Idq0VmZ7AFue4oYXsn3l7zZ1ewOdTaIhN729u/LvcFTt

nwYXTyIqffPv9OELPO4cwCxvvEW3D52k9L4bzxv3n765syN/aWI7mft/9xDIsiAfHnUAwO087EYvO8KQY156B57tgelPAWmRpByPZQekAidhR6st+azC/nsHbT3B0Bap0gXaj9R8MKQ8iIUPYLVDy+/M6QvoQej20Po28mYcyWEMyxpDP7DWOcP6L3D6R7sZYcCP7dJzgi6I9V1qXwThevF9CefCwmK9MzRE8PaUckmzLawDE+o5UeMnXL1JjyxP

usuaOqT9lnlzAZMdMm/LFj4q1Y4b02Oz9djgU8oCFNOO4rT+1x1KfceeO0r+aDK8qdVOBOfHcoKToVdCd6vwniByJyaeidd7Ynlp+J/gYasOmmrLpumW6cZk7svTJ8tmVFI5lXyAzN8oMzezPGCyUpYvdJ/Xc3kxnsnnA2LWLnycpmnGS0Yp5mb7vcX+DuZwQ8IfXjPn6nL1Ce9IdadyGewCholAQ+IAdbunTAJs/1r6fUPBnshDCMM6ZijON44z

4t6W6meNm5t7sUw7M+520OCSnupZ72hWeP21n79l+3dtXOjvtnX9lhz/Zrwmg/tgDzi4DrOcxGwd8Ry8zc+h23mHnD5xB0+dqcvm3z4QdB18+KPvO/z/zwC1UaBdH3QLoLhnQ+DIcIBIX7O7tzQ82gWx4Lnuhh2henFi6HS5FlY5i+ovYusMDF4vfi5kuEvDjDuoS08bJdgm38Be1ZBB+pccBaX+l+l4ZcZccATLzLjl+ZbZdYmNH3egx9o4cu8u

SPXLwV8Y7Vxr6zH2+8V3q8lfAhuToV8K3K4VfFXnHyr3FW44DIeOP9Gr8uMVe1cBO8rQT6ZhAZCel4wncBsq8QCffEAqrMTmq3E+tMJO7XyT5q1lOlktK8p8s7UO0qD7FTur3S3qxIAmAZD6gcAcKreC2CNBqgZmW8DwHSHsAo4Oa+qZUPkX0IahC/ZZavXJg3B1M1YfzH8EuzwTDgUwOcIFhrkxee1T1+4K92usrclwHXM4NWAPom2ZyvDI4GcB

uALBLgKYC4LML+tcb9uvyoG0HNnVHDrbIK8OU7YuEu2obPt6Faf0gZHSEbycpFX7b3Wo2g7Sm49SprDsBvC5kdxoDevAF3roUsIzoDAM+nw4lKSlOcDvXfXfYmVmA+SojhHWVg87Tmgu1ze8WY329JdjzUKu82qMcbugqu4hrM/IaLP6AAZY0D/AmZJAyQ4AbmpAnLLteh5FbkFhuxfZ5gLwIzevSLXLABpFsn7KsEtkqULyTGs+ueWy+t1/gHG/

zxOok0By9hdttaXV8E2CLnbkNyFa17lBxyOvcK12zuuRt9eA7KisMRGMEkAjw76myO8kJjvSTLKp5cfhXwJPvqfpyd5SRndPLXYdrl5VxUBs5WgbXNxdgVf4ug2BKK7V3oW/t9OJ6MDaJBTJxFrjORvAmBVGN5lU7tFOOAPd7g0m+zMpvKnabmp3U4q2Koc3LT6e/m8LdtbgXWFqAMvArctmRtbZnQxM5d9Ow3fNEGi2fc7f9nqHEsIllOxQgRRg

8bIbztH8ZA/EnWeLDTuEZR1oBl4qAAANQ2xE/6+ctqkbiIbVd3q97IzS7kd0uq9OHvD3SZZdqPiPBH/l4Y50fFW9HnLsj9y9o96R6PzJxj76EsecnD9bH2xxftcgOOb93HpV6lZVeJW1XQnxk949RN+OdXEnvV8E+CtFXjX8niJ4p6idmm1P1rjT7a7tONWnT2mJlzX4I+su0w7Lly/WHiAmYJYMwEKBMBCjchwqEsXR3y9I/3/H/z/1/+/878p/

LvVmR4gfvysRgDO+BP1QIH8EccJ/VBAlNGTDKyStBPOU0ZMsUaQC1cl/cT0QDJPA1xk9ioOT1Ktt/JTxU9LXffzqtNPY/3tdT/VJ31oekdXzDdG7LXwTMdfVu2TN9fGwC7sjfEpyzMctc3yqd03Wg2t9izFLTt8p7HB2rNOnP3zbh3fFex8RWzZtwXt/fd3w7cFtN93D9J2J/Cj8PgGPzj8dAhP3ZQk/MdnnlPdTmnT8aILPxz9DAvPxMDrmHd3g

dnnfdww94TLDxqdq/UU1r8iPKyyo92/Gjxb8v/ajyMdirEVwY9/LNvRY9pXMK1ldR/eV1gC9XHjyACNsfjzf05/LvQX9TLLAKAMV/MAyk9tTdfyNcX9Lf1Ncd/c1z38VgWqxtdEnE/zINMTdwKVNL/faz1cAgu/wf8n/F/zf8P/fwJ8Cf/NoP/9OgvVxFcQAsANMg19SAP5MoAGAPH94gyf11dkgmU1SD9TFukwD5oZfxwDV/XIMNdZPTfyIDigk

gItcG9K1woCj/JJwddNiXeQ3Z95H/g9Mj5N11yUfTT1z9NvXR4iPE75cpQd5DFJ+UvFVfegJogNfWMwYMcnNAB+o9fG30RJDfY30edynfgMt8cPYQLHsFAMQJkNL3DpyLclAmQMD85AobS99FA6QID98wIPxmcw/CPy0CEoHQJkRY/KmlJCbwAwJ0gjA4zhT9TAtP15RLA8DFz8SWfPxgdC/BHQcDHzUv3Q9y/TD0r8RDYy1RMGgrwKcsfArRw78

ughv1I8pQvwIGC6PRk1CCmPcIIH9grIfxlcR/SKymC29BIM70kg1VwE91Xef3StRPTINys1gnILwD8grYMKCdgm4zNdKrfYIgBDgyoK08HXWoNFDO9K/yaDug1oL/8Ogz/39Df/doIADggpUOAChoUAIld1QrvTGDoArj2mD4Alxy70kA2f1QCu9dALQM9XMTyyCrQmAzX9rHDf3tD4DYgN39qrcoPU96rKgO09HXb+Vas0AVpUM8CpD8S6sYBb8

SK5+IMUAQg2QcKhgANgVaBSA/wAegIhAILiAQg+6SLXSFlrXzwLV6uRzE3ojgSYG3p3MCniWArZICimAlbeSigkiNSH3eVSFTYHJhZgU8J+8IZO5RnJVgUqDm5rgMvheVK+C23K8n6Or2nVsfIFVx9HbPfghsIVURWJ8xND2068KfYn13VzpGnwzk6fEOwZ8tFbG201L1AMDMxJvElVJtZvJDQW8nsR8h+w2VAWhTsz6IGQF8m5cYCXCAQKngc1g

NALTrEvFKVQg0ZfKDTLt5fQzyZFBbG7xBwRbB73DEeYMUEAgTMfiAQh5bfDUgBtsE4DfIVuE4DxFEcdzAbkdlMH1Khzgf4Ga4TKbXh154fLzCUFcFG7A2AXgBcEH4XrXYikFUfLYQx9KvQOR4UavATS/Cl1ZryJ9hQd2zJ8JNb2xk0UVFBgU0rpI9WzlhvHFTgii5PiMJtQBbTXLlm5Rvh+ljbOmz1BZwHCIIjLNPekmEV6MdUA1CBciLhkyRQu2

5toI/uVO8+bYVUYjRVPsWrswlCKh+D6QzpkYCsnZgNydgQ9gNBCQOCIDA53GFjig5BUe1kY5qmNvBdZ4LA+A45PWNpg/Yv3IYwE57DD9lE4AocTkjYZmaTnmZZODgHk5DCO8ETY5zZTjnNVOK0HU4C6I6Fhhs2NwzzY+dPTmLZ1IUtmeZbAyCErZPmEQjnNLOOc2s4m2KKzs522XtjhYu2Fzjui3OBN17synZN0HtqnWENHsGnBEKacyzJEMd957

XEJDgP2D3yrd3tRoHrcKlVAF+gWYeFiZBdIRiAjxk9JkCOJlzfPE91IY4PA4BGoHZFsoRmM3FBhUY9SBCR88fe3G00QvENIBQY1QK7ciQzQJJZo/QLhGZ3AcGGj8PgfAEQAm4NkHJAe8UiyRiuo8GHLZeo1NmUCHwD1laZ8AGmIfBxHJaNHA2OewzFimAcKiwZpYpgHEcdogzj2ijOFgBFiAWf301iTQREB1i1Y0gHEczOAKD1jNmNuAtjL+YFiD

9Lda2KpiJYr1h6j+Y8diD9NDBd2ThetXGHNiI2STjGi5mWNkaApo+Nn0MlY0gBVirQN2JYchoLijxhutEgE0NUAXwSYBGHDCG4NwwXcCsAnWB4CAMOAIP2VguKE3RwhHY130NjYAY2NyAmADeVT9gUIti1jaEHWLxg041CxkQMYcPUEBjQOHFTi1ANU0LjOHSukbi/kfaNLi4AcuP99bYwONvMAIDqCpDkPHLRdADAb6JrJUAUOKeYXeUKEV4dLG

lj0sXAoUIzdcPb0NUdxQ0U2aD5QoIKaCL4gVyviV9SMNMce/MIJgMIgzUKiDtQsf2itkwx/USCTsOYOSsFgiAHSDmXC0NmDCwjYPwDUrbYLLDdgisNU8qwg/xrCTglvwfBo4+WM70CgbQCwTk4p0Db0fUQoCwTtAHBL1csjEWBHiq4luIwTCE4hOtC8gTvVBZroogDbYHOO6M7ZnOHtj7Z0WZ00FRnwGeP3iFHKvzogG9aznoS29RhMhYgAuoNFM

wLUkF0hmAaOGjwysckBdCNgDU2kTYQWRL+YqQVpC7BJkWMGRBlE4A1UTFTUU1ITtochMIBwgEKCmhsUfRO7AXQsYDUSu9MxNYB9ONQEsTmASLWkxoQWMAlgYAAxNyAjEpq2ggz/GAwejWYbgCkS0g5zg8SkaOxMMS19RICcSv9GJKsSbE7UGIBmgNEBChlkWEBmIAkl0PnBkkoBNSTmAaKl3A4cfiAj57E4Az+BgkqCC4TIILhPrDl5WXlXkfOYq

IXlw3MqKBDdfSqJEDOA5xlcY6oupmg5eEh1hajnWSDgVi1WWZBdjuonjj51emfpmDZhOIaPCARowOOjYJouNiEgGIRTj50Fo+wzljtkDkIQ4x4LTjnMdOOc0rjtYmuIOiGQpSHeYTo75nvs62PnUuje2WziYT7OaFlYSnORFg4TUWLhJeiTfN6LN8PowQPadM3KqNEC/oyewBjyjAtyBjb3J2JBi2WMGPkDqHLGLvgbQGGJAg4YmrERiLCFGNIA0

Y0mIhioYpuBxiwIOOHxiqQlCBCAyUkmPRgyY73wPtffVFNd90U4TgJCQ/C+yt0NA/zgXjmY99DZidAjmK5jY4XmMjpdmIaBERJYlaOWorY7lPFjOOAgFNjZY+OKwYZkt5Mji0EuAE1S/tYeLcTDOe5N1SBmCuJNTdo6uKeYjUq3VtiLUz3SniXkmeIdiadSOPmSpYxZLjiio5agIsSCDWMiRfYgKH9iJOXIODjFmUOPk40U1BNVifU1FwGgE4gi2

TjBtVOI+B24oaE7jbYHON7j846miLiS45DEni24W5NtTa4w6ILZrUpuP2jW4jNIzjs07OJ7i84/uIDSNjatNHidY8eJLSoAaeOmZZ408zCAF48p2Xj9AVeJTR14+5K3iIlPe35C94+R1cDBE5Rx9C6/bwNlDAg5v2vjJQ2+M3T74rv2VCn41UJfi4wqVzfiOPGIKTC9QmYISs6TZAJNDokzV3NCVg7ALTDcAsaM2CCA6BIU89gsoMwNqwygOQSmg

uNKtA8g+0GoTbQXBJgN8E1nnAzmASDJJMXE8hP8TKEghOwSIM9YOBZ6E75IkS/k2FjYTAU1zmaSeEjgD4SF0w+NoNq/YRP7TREmA3ETmEmjLQgv9DRNdB5EtRHiTAktfWMSArdRPrgWM7RNwBdEuHAKSgkkxOcSO01yDSTWAWxKUSOMxk0cTRMhvUQzTUjxK8SxyHxOIA/E4TM4z6kppL1dwkhjNQgUkxFliS1CS6C0zGTJJIUySk4zMkyyUWMCy

SyuXJKPwCAczK70ikqzIejYkrkAqTmwKpNcyG9OpKdMQkuiGaSzglXmdcslG4NRRvTD13PlopYpVikXgyAHvkQzD4LDMX5QqIeTOk8LX+Dm7CqMS0OA+2gUBaowLjGSSMiZKdZamaZPaj4yd1nVTvWBNPhd+OFZKE46adZOk5xmcNKk5I03ZKOh9kuaMViWsk5KVTM2c5P/BLk+w2uT7DW5Obj7kkbJlxjo6tlOj7Dc6PsNPkhhPBYfk26LwyAU7

tkIzODRN3BS+AyFKt9x0233hTc3B3yRSnfFtw9T0QitN5TMQte0pTm4PFNhj1IIlLHSSU65GJj0Yl7JkQaUvGIpACYshCJjmUv7KbcffW7Ijj7s6mIxTaY0P3e0hUyPxFT6olmPqJ2YoIClSeY97FlSHSBVK45hYw5NhyvU+1MTSm4BOJVT9U+NN5S74DWPEy7kp5idTrmA2IZzy0uHNpyZY97UdSqcm2NdT+091JhznY+rNjjyc+bLbT8wINJ9j

/EMNNGjtkkOLDi7sqmINTRcxY21SQMlNNtA00tuIbSs47uNzi2ifNIlzQ4ItLLilcstJNjK0+ZAZzZspnPTT044PEbT9cvNNbSi4pTN2jlYMeOLSlcvtMwzU/eeOj8R02EDHSs3CdI3imAadJ3jZHedIr8GXYUPP8PAsUNb16/W/0vjd0rExvim/Cj2FcH40V3Mc+/WMPADB/ERB5MtQ8YJ1Cv4q9JTDePQ0Jn9jQwBOASa/UBOyDwEm0OLCCgqU

yKDHQkoOdDf0ioMP8qg6gKAzlYnVKoS0MuDJITjcVDKIT0MvBLZyUMmDPHz4MukxETIk7DOYTcM+6PYT9s7hOhNSM2POw9484+NqgqMzDMiSxEzbIkSDMkkxkSWMhRPmgZMlROKTb8uRP4zBM5sH8yIALjMUhFM8TNiT0k9jIcTik93IM4VMjJFyhfE/xJqTtMoLIaS9M9hOvy6TTzKsTTMwAuANLM7jOiSbMspIALMk7JKcz8k6AsZN3MzAqMzz

ALzJxxKk6pISTiCmMKEhGktzhaSTRbKSS4QNNpU6tTPDsKjUIAREHSFlYNDRCAxgOEB4ARAScOSF0RXAFqBGgeZU+8JAFa3881rE4AvJpgQ+liwZ+dzDM0fMeMECwVuS4E0jNeEdUS9iuV7k2BjZRSnL4W5R5UPokfJ7GrVfpEvg6kAQF4G2BQKNH0tsjIwGxMiAVa53Miwbb8IJ9fwhFX/DbIh4SAiWvRyP9tnItFUU1OKKCL5Usbc9Su94I4hk

jgkIwnEgFUIu73QiYsc4C+wkwEH0gBcIing29oUWLBijy+PbzyjObHuVSj4ik71ojS7TzSyiAFS72Yj/NGISQ1OwqoEkAEIRoEJBUEPbFkKEFJqQI1PsdzDiBZwdzCuVOpHqS0LJRXoT+9ZuOzWuBlgEhSwlXyB8gr5Kwc2z7VPZdTFKgc7FYAtlrsMLCn4nwgG1fDbbarxBsHbPwssjCfP8JsjSfUIvJ9wi14Vk1H+cCLRs3IzFTSjVNRIq8jI7

QsDZ9YxAKMcV0RM4BF81vBcBKLpQBMCXAVBADUrEEoiX2qKwNNzQyi5ffmwV82i3KLYLclKoB9Y2iWeHmpuQRAGwBP2UcVoCJAIkpBgSS6mjJL6ESkvMYwspJTNAfwXL2oVHFCngvJFJYKU9Nos913yVfTE9kvkEs/JVKVbeBKQfk0soWXDNCS/FmJL6EBkvJLmSy6hatnxNqzfEOCnLi4LRbG7HwBLIOAD7p0hTAGVhnAaoCEAzMaoGaBqgREEj

ghAEzDEohinzxJh5wpBV2I5gPL2rAVBa4BnAAQW5Uo10FB8krAawAbjcoxpFtS15pgLYEGkLZbb00ibC3Yh2tyYL7DnowvTHkR9XCwyLhsXwtCjfDri+20/C7i2iQcjnuddU9sZFLrzkVQIuTTFsXI1RWDshvUO08jY7ZIp2w5bXyMhEpvFCPJtsiqzUrk5gQH3fUbsIwsZtLNCnk0iVbTekqL8S5KMO9qI6kRhlGi87wFtY7JX2rs2InuiK50EW

oGaAeIP8FWhOyrzzw0RiwSOQUKwDYHbVO+S8hPJlwF4Eo0drUqBWAlKTLxCwLgMvnh83yAormFTbG4FK9ONC4vzKri0yJuLiy0FX8KmvB4qCKnimFUAjXi6yPeKnI7iUDsYi8MTiLjvEb0qVcbMSR2wuIEEvhE5GfYr2ssy4zRxQVgNAUijMBAzR+EVKWcoojuVKXz+LebLEuaKbJVovXKWIpKMC1icDpB4h7dRECXc6HLpMFRlAvWH4rDjUOI9Z

wocIG61sgfQHygOcumm0A6STyDTSAAHm9hmgGVPXw2oYIBIgkWQVEUxULVSu1BM48vTRA2iIgARBSQjGDQAQobIEqg8ABQDrhmAZWBChA4TIGk4ewEQENAEAFys4ACtaEwlhqIOTHDBSyHLW619KokBgBOdWgxxye8blPEqIyKSt9AZK5gG61+CWoDUre2OiC3tutAAEIsqnKrqDAgelxQgNgAAG4QsuiGpQCYHGIoBUALxnWwEARqvrBrwbrUG1

qq3vCUAPtTgFuIF4zWG8FIoEPxvAqMaPysF/fXXGyBUAeklqg6qsCEoLggWyBmrmq7UEaqN4/AG0B2qvoBWqEABzNmgoAbrX30XAW8G5Ax9XKqOhiq7UACsrtUgCXA8ZISDuqYnZwEcBVAHMKEgREGSpeq3qtQA1MwgalBLdLXV6rZQPq2qE0xBtbQEbhDq7rQaquqwVEggFq+0DuqRmL6r6B7tehECqHQJqoL13ADxzdRtABkAM5jqlAAb1Ia5D

G61CwXpkUr4anqoUBAmHpGj98dGav4JH8Vi1qqRYMCA6R5MYQCbg1qvqG0BlAm3R5qRADKtprEazmonxpKhmH5qKAQWpkCbdaWtkrxasFm2guavWA5hAanGoar5at3xt1NawKrFruquiF6rtzL/TCAhAArQb12mW8TsBLIXzzxh4jRqGppEdLQDpASYW5A5ArzHQMEcOatWqh1iINuB2qbwNataqba8h2XpAQf63+5/qXmtRq0q9GqedAalWqRqO

kYOu1qw64Ou61I6kZijr9QCfBFrg/DpCVqRmDpANqKQWmpNr6awSsPNba+3Tag0a75iRrBHJWrxhmqzKqZB/fdOp6Y9YYOrTT6a7OouANgVADK1NgFWrKqRAT5AjJW67qpBSN4HiDgAcEKFBBgcksCyJBJAGg0FQEqtMjErsACSpNhUqz0H5NMqt1GyrtQNNOr8kahqozq2qjqsrr5qyWqgAjAdOtDrb68hyEgdauSDrg8a3kEqhA4EmpyBnAM6o

urQWa6pvAIauiHvrIICetIApjJ+u7r1sPqH7qCLH4WHrR6jYFpq56r+VaTPJPioEqhKqFOhTPKt32SrJKjgFbq5KrIEUqP2FSr1wcqzStQBtK3HN0qUIaKsMqN4YyrGq6Gsys7joQSyrKdCAGyvUA7K0KEcqKEAKrcqPKp2GGj+dXysQAAqikBqhnwEKtYAjsCKuV0oqhAAMrYqzeo3ht69fF3r96tuEPr0qk+uyAz6hAAvq8qtkEKqwGqxqOgYG

iqu6qmkv2vqq+oG+vDrn3KBtZxq6/qtPZBqkQFqxRq25AmrO6xmrdRZqwQglr/apavxrVqlqo2rcgLauDq9qg6onAAG06vOqaAS6uggwG26t5qHqvKKerean6tBqNTRutKb3q/6sxqKQSpr+raTCGqhq0QGGrhrjah+v9qCgFGoFiZKjGsBrsa5qtia9qwmohYjqpvTJrtACmqpqFK9WLabIIXqoNoma6BGmr2MNZBt1XG9WuwBC67WoFqhaxEEL

qja9ZqlqE6kOsQa5a3ZooaU6yWrLqamkGFlrda1i3LrbQTqtmafGj7R5Rza08ytqIATxqfxNAe2pJhHa8B2drHnQXU0B3asCCowvam5x9qIyQ5smqg628VfrPG7OoVFc6kZnzqumxut6bAqy5v9q06xFoSbkWnOujr867mrjqjmo+tBwJ8R5u8bTazixvBBHR/Ebq9EZuunrjmtuoIt4WqAAQb8WvoGQbB6i4BHqz6DBucacycqoOMTYGerINiMh

eqXqeYFeqcz163RufB9GklkMaUq8hvZazGhAAsb7G6JrcbGqpFqzrvGpGvgaCW9auRaP6gWq/qKk4IF/qrkDJqAasmkZlAbT6zyDRYFjbxscbOW5+tvF1tJBtZwUGoeqFax62euIzWS+mU3ZIs110FK7g2LMKU9xcUp9cKUW+WSy3g0byqU5S3BtIaAMGuuEdhKnLPVayGihvkrqGtllobcEc+tQAGGphsSq9KrRpiqjKh3NMr2MPGD4bLoQ5CEb

JAERocrOYZytcr3KzypkafK2EHka64RRuCrQqtRoD1NG7RriqiG1VpQgi2g+s1bKWjKrsacmut1sa3WqttKq24Seq+VRWhgqvr3Go1rvqXm02r8ai3aPyGqgmvZ3GqdArluWbImz1HaaCYQZoiakWzau2rrwVJuab0mk6qdaQGoSDybimkQEKaQNMDtIA6msGuggKm4Gt+rYOqCABrAqmDp/1Gm6GvSbWmhGtVqCYTpvJasWpOqxqcaj9uyBhm4m

rGaIAcmpwhKa6mpmacO+ZsZqdA5mq4aZq4ODWa32jZq2a7m3Zv2bnmw5uLrjm7ZrOaFa+0q1bcWgmGuatanjtE7Hmg5rpq3m6cTSCLar5p+a7ah2vB1gW12rBbyAD2shbbBBeN9rOOgOq7rzWzOo6riWvOpGZMW45uxaK60VtTre6szrfrn3SzvRbS6vWELqPO7ABLrqWm5tpbq6+lolaekBuvZbDmluvZacajusDruWv1onw+6wNoFa0G4VvHr9

22BuC7e09lrDboTWVrchl60KEVaUoZVqmidKtVpkCc24xtXbTGjdsvrJa6+rPbyHE1sfrfWmWsJas6q1rlqbWn+tP1/6wDuAbsm11vMb3WiBtqgvWtLrgaWum8H9aKAfltQaQ2kVulboTDUt/kmwgzxaLWwjpU4LQFbcqqAxQXABmB8ASODgAhAZIVWhWq2oGwBNAHgC4hRa9IQR4XS+QrBA1rJShUoHyLYH2LUlBRko1LylQvML/MKfivJDw17g

OVpgZrj58tlZcHeVdI1AHEFpgH7F14eufhnOKrbYCqq9QKosrQo8fRr3ZArIx4vLLYbNr0k1gIiIup8oi1Ctcimy9yJbLYItsqLk4FLsuJseyjIr7K47euVZVcFRSjW84osisF8sxU8K1V6K7isoiUoo7xoiaRWX3ojsS7KMrt2ijkU6LuC28DZB8AegG8BsAdIULAYACgHSFGgfiB7BsAWoHcwewRMQe65w1awXCz6YLxjK+GWgU0lIvN6zrU3y

DqTfJzZTnsNtT6D6wGleGO8I6kPMPESTLeAFQQSASIpIEXAfse6x/KyvICtX4sfQspx9MeiyNLLt1YIueKvReyKT6SesCLJ7afPiWbK/irCojtcK4EG5A0i04mZ65vQit+Bxi9YB+lZwNb0TL07JuT3pzyPIpIjBeqeUYqi7ZisxLJetisWUmIzitl6EALcsK4qgQCAuA/mEKEAhsAEzEIAH/GAGIRuQZgGcBPBCbxN63Ss3o9KfyfzCvLO+MsXm

BkwGcHgkkgL7H2UVBCYT+APMH8tIVJgK8r+A+uA4GW8h6qHqmlpuJQTTLDVGcECxkejwsuK0e7wrMjQbCCvuLAi6TXx74KtPurLKfXr0z6UK7PvRsT1e6Rp7gRdsuBAquBnrtASbMvrQjWei7Futq+5vjW8rgWEpVFUwfa38wjCsiNRKDvGotF6lyjnzO9h5NcuBENy2700xwAPimBBlkQOOhRdMaAA+BsgS4kD4dgBgEsSKAcPx8LOFI2CkG2QQ

YAgBb2yKEaA+gfQF/ruNX/q8K5BwJoUGlB8QYAHbioAdkH5B3IEUGsgDMxAHEbDQeGqjBpQZUH2vF4rvlNBqwayAbBonreLigCwdqxjB/QHDhoBriXcGtBrIAXroiinuSyHBqAE8Gskw+RjaTiPwccH9ACIbZKo2+wcsGwhpQaQJ7guLLcHDBlIacHPVQNTTjxIBwRiHsh/QHoM8h31WBBvVKgGEGshzwbyGUrKoFKxZB5gDphatGcMlEblK8ovJ

8vRYU75lGTLBaGuQEzDFpN6BemcK/1cQQWBhBjyAMBIk+7DbJwQVIE14kgTkSKHPB7wbek7QHrwpBZB0kBIBI2y4PQq9hvoGeIcwYQd2HiAKtgQBItATOCAObJTRIALuArglgUQIrmIhCQbrRPIhhXgHnARmT4dzr21QbVZAAIZQC1g0KN4dwAPhqQV+GoR3gBhGrysYEBGVhrIecHcukWEKGw7ACD9A/6+wRhZ/VHSE0BbhrUqvYiAE4eS5/5SA

AbQiRlBnChJyNqxWH1OgFu5AdIOAEuHrhgkaoGTRTdCqSUQWYfSLvPMIGCAe8KlFygDAeoZl68SiiNHSOYQUYth87NLjgM89Dx29x8APyi0xwAPLhx6AoSJLYHNMIAA=
```
%%