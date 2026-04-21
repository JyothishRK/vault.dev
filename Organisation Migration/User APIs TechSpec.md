---

excalidraw-plugin: parsed
tags: [excalidraw]

---
==⚠  Switch to EXCALIDRAW VIEW in the MORE OPTIONS menu of this document. ⚠== You can decompress Drawing data with the command palette: 'Decompress current Excalidraw file'. For more info check in plugin settings under 'Saving'


# Excalidraw Data

## Text Elements
Create User
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
                        ii. description
                        iii. parentId
        2. Authentication
                a. token check 
                b. org must be active
        3. Authorization
                a. root/admin user/
                b. power user 
                        i. Permission: manage RC
        4. Validations
                a. title
                        i. Required
                b. parent RC 
                        i. Exist
                        ii. User has edit access for RC
        5. Actions
                a. create RC entry
                b. Serialize 
                c. Send Response
        6. Bg action
                a. Create RC permissions
                        i. Owner visibility for user created as direct editor. 
                        ii. Ancestor's collaborators viewers visibility record added?
        7. Response
 ^9Zd9IFvu

%%
## Drawing
```compressed-json
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
```
%%