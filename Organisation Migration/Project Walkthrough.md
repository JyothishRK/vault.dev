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

%%
## Drawing
```compressed-json
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
```
%%