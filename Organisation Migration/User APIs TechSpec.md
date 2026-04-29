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
                        iii. job_title
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
        5. Actions
                a. create User entry
                b. Serialize 
                c. Send Response
        6. Bg action
                a. License_type calculation
                b. utc_offset calculation.
                c. Password generation
                d. Onboarding email to user
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
        5. Actions
                a. Update User entry
                b. Serialize 
                c. Send Response
        6. Bg action
                a. License_type calculation
                b. utc_offset calculation.
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

%%
## Drawing
```compressed-json
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
```
%%