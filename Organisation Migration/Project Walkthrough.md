---

excalidraw-plugin: parsed
tags: [excalidraw]

---
==⚠  Switch to EXCALIDRAW VIEW in the MORE OPTIONS menu of this document. ⚠== You can decompress Drawing data with the command palette: 'Decompress current Excalidraw file'. For more info check in plugin settings under 'Saving'


# Excalidraw Data

## Text Elements
- Sails Architecture:
---
* MVC
* Models
* App Init (Run modes & Datastores.js)
* Routes
* Middleware & Usage in the Controller file
* Route -> Controller -> Action
* Permission Revamp (Action)
* Error Handling (Refer Policy APIs)
* 3 Run modes:
    - API
    - Worker
    - Cron
* Structure:
    - Payload extraction
    - Validation
    - Action Steps
    - Trigger worker (If additional business logic present)
        - Create event payload
        - Use queue service
        - Send request
            - Worker picks based on event and logic (Background logic)
* Crons:
    - Cron.js - Cron init and scheduler.
    - Based on logic - trigger the respective worker logic.
* Cache service
* Eslintrc config
* SQL file
    - Setting up DB
* Development Setup:
    - Documentation will be shared.
* Deprecation of legacy Organization data fetch through Mongo (Action)

- Documentation:
---
* Models:
    - OneNote : Backend > Organization > Organization Models

- Action Items:
---
* Kickstart Tech Spec for Organization APIs
* Documentation:
    - OneNote: Backend > Organization > Organization APIs
* For Pagination & Filter:
    - Policy Controller
 ^deFoUiec

Organization V2.0
----------------- ^f40KFouM

React.js
-----
* Functional based components
* React lifecycle hooks
* File based routing
* File structure:
    - Components
        - Atoms (No logic involved)
        - Molecules (Group of atoms)
    - Constants
        - E.g. api.constants.ts
    - Contexts
        - Passing props from parent to child component.
        - Using contexts - No need for manually passing props from parent to child.
    - Layout
        - app-layout
        - desktop-layout
    - Services
        - app, api.... service files.
    - Lifecycle Hooks:
        - UseEffect()
        - UseState()
    - Middleware
    - Models
    - Store (Zustand)
        - Larger level of context
        - Managed at application level
        - Primarily used by components to fetch the required data (Props).

    - Hooks:
        - Used to fetch data from store.
    - Stubs
    - Styles
        - Global level styles
        - scss used.
    - Modules
        - All components of page is written here
     ^ekYP44PU

Action Items:
-----

Users:
---
* Create user
* Update user
* Get all users
* Get user details

User groups:
---
* Deactivate user group
* Activate user group

Reports:
---
* Export Subscription Details

Misc:
---
* Launch API


 ^9KKsqeO0

%%
## Drawing
```compressed-json
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
```
%%