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
----- ^f40KFouM

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
    - Hooks:
        - UseEffect()
        - UseState()
    
     ^ekYP44PU

%%
## Drawing
```compressed-json
N4KAkARALgngDgUwgLgAQQQDwMYEMA2AlgCYBOuA7hADTgQBuCpAzoQPYB2KqATLZMzYBXUtiRoIACyhQ4zZAHoFAc0JRJQgEYA6bGwC2CgF7N6hbEcK4OCtptbErHALRY8RMpWdx8Q1TdIEfARcZgRmBShcZQUebQBGABZtAGYaOiCEfQQOKGZuAG1wMFAwMogSbghiBAAxNgBVQgRsdLLIWEQqqCwoNvLMbmcUgE4U7USABgA2AA5p+PiRgFYA

dlX5/nKYIZ5Z+O0R6dWeRNHJkcSeY9WtyAoSdW51g5GeN+nR4+WVu6kEQjKaTcRarZZ/azKYLcSZ/ZhQUhsADWCAAwmx8GxSFUAMTxBD4/H9SCaXDYJHKRFCDjEdGY7ESBHWZhwXCBXLEiAAM0I+HwAGVYNCJIIPJz4YiUQB1R6Sbh8YoCBHIhCCmDC9Ciyp/KlAjjhfJoeJ/Nis7BqHZGyawxUQSnCOAASWIhtQBQAun8ueRss7uBwhHy/oQaVg

qrhJpyqTT9cxXQGg7awghiCDlvNZislrM/owWOwuEbZpNjba86xOAA5ThiEFjOaraYXHO2wjMAAimR6qbQXIIYT+mmENIAosFsrl44H8H8hHBiLhuyD1tNjmcrvFVik/kQOEj/dOd2xySnuH38APbT1MH0JM5UPzcLzmKgAIKiSRqFpQEQIZAAHRcZxnEAgAqVAAFkADVUTAyC2BqC84NfOA4FQR0ODUVAAAoACVqVQfQEPCVAADJUHbRdQigLFw

m0AArZgAEo4Nw4QemYOCIJIYhggoNkEDI1AGmYaJBJDVB1EE9FckRPkmFQHlglY9jBOcAA+VAZOVeTSFQDS32wKACzggAFJh9DbCsOFQXCEHoXB9DQ7DXyMgsWI4cCR1IRE9IACWsXiQ2UHC7K5BTTIxcwYDfUzHWYuCUlsgiiJqeRANQTL9Nix0Mqy+8pSxFFSDyzL71RREODgwVSCEIzfwAmz8tQUzcHVNhcGIVBenINzOFK7KoIIEhFxMpqys

M4zOAfHo5AG+8ABVSEBZQFIoIqFOwx0uVQTrHCmjgCFQTQhFYWMX0xVRsFQOBAjCXIPKyp7soqkIem6xhchutrMU6ganvvETBIARyEBAwdQMJSDMMR/ua/kci6wJQfCKA4ee7LCtIYqbvMJEX1JMIuum+ycigXaaVQS7zBwgAhMkKWjLrqewDzwIqzh0vGl7KoYl9ysq1AQywwLIewSQU0DJhtHm1B6aJ1BppZ7KERWhSpNQO7EDcxhUHW7GFJZm

XPK0skJchpgYYQOCR2YXcEWuvQOB5ZRqoARQAGUU3lre5+8EZkYLUDnCjabgztGExOAJ3JgO50agGKOPIQY9G6aHj5Y7BOYSQBOIY3wM7W6WjTmy2B24JlDJGKAHlSCrzCjFL1AFyiRSECgcXJMkXVJHgjhlDYHDXIOjzAPvdtk9Tg7GuAkCTYg4iLwT5qa/1at3rQemT0pzS64bwgm4O1A9/r6xD+bxfEM4qqXEmgt0J6fQubnuCAGk8fhNlyYW

lo+/5bWiksSoH3ufI+D9XxxRvoXKeZNS4rwmmvBAG8/xywZojE+ICz6N2bqfA+4DpqQPinBeoelWr+GbuRWovIeikAQdlSKRBsAxW0nJYIJUuA6koL/G8VR/ZPgvG+D8X56qBFnsBLiMEuJL2gW+VC6FMLkzwilYiL5yKUSiPCWizA+Zs1sqpWR3FiC8QQPxQIQkRJiSFjZDWrCMTsO9spE2bEhDvQMnY3S+lNIjzGuBcypBLJxgfnZByTlh59Q4

Ho7yvlUABRpLuEKeEEDhTIVFZhOUEomySvhGyqVwj0PvEQ2WWNiqyw5rfcCNU6o/jEbLVq7VOrdRvL1A6sshoeFLrLHx01BQIDmn7VAS01Z6X1jjLaO09pqALEdE6Z0DRUzYFdG6d0yaPQxjzN6glSZfVZA04g6NspA1QCjCGUMrYHP9hg5GYN4QHMTiUhScBP7HVCCmRWNltnk1FsrbC29GbDmZos8wejylc0TuUvmPNprCy+ZTZg4tJbsONone

WbylZAuuveVWyhVp6Q1lrb8hBdajMNhigupsEUW2huYX2Xk7YhgdqgJ2Lt3ZeyUr7ROAdjID2DmhdsYcTYRyCKaGOD4O7x1lpPbAKc4HHwzvgLOkNc6BHzuHPpgQ8DH3LlTBAVd0mgJwcfVuuB26dz7uoXu/dB7hNHoBceSdpXTwLOI+e4Er5BDBavdebBN5oJ3l1PBYDcFYPwZfGRdq77dJso6J+L8JEmw/uSL+pAf5/wfIArkwCDUX2PkQ2RUq

ZW5HgbLJBKCt7oN3iGoNx9A2GogVAkhwDyEhkoagah+BaEFJamklhnAdLsMApyTNuR+SH3ELwG07RuR9tqI5XklpUAtindeKAr4iDKELOgYIXI+i5iYMZdwa7ASbugKaTkTsoghiYH6NACYZy2ixICFt+AeG3nQPw58QjxYiJqX+ce8a3VSIXuGk2KE0IYSwso3JqihIaOoto3RKlXHhC4jxPiAkLGiVWtY7u0k+1sIUuypDbjNIeIcQZKNZkLJW

WCfZRyzko1RJ8sAuJQUeVJJSd2phMU816OySotKXaikDIeRw8FlVqoImqQ1OpP0OpdR6mSVpAz2kjWU4nKNM0+k30TkMnFa0Np6XGbtYxUzOAzNOleuMCylnF3ulANZGMBabI+mTb6eyLnCTCMcsGpzLY0s8wjSm1zUZ3OaqJ3GSaXkKxJp9WFgKlm/IZvaakCXgVwVBV2iFjEoU2RhRTLq8KJbEClqQZFzVUXExssrLFy19N4vNgSnWgkSV6SNh

ls22d/OwxNrbe2ogmWcBZSbfkntHEcvhh3blIUQ78rVZHEVbm45wC7QWp16deQKs0NnZVKZyVFw1c3bVldq5VrrdNY1pqu4WuEECK1Q8XIRLHnfNbsrnX/tdfBa+XbS0+sEuW/1mDs0EJsrWnND93VIVvoUiJj8shxs+4m/GUQU2DLTQAloQC9LA+bnm8OsCi0zxLd6noAOUSVpxzWs74PCENpNqQlq0QW3HyoTQpgXbGHRS0vh+xTBB0QlcWwOy

rAjDjvPJeKdu4EB+QBECN9BweDLGKAAXy2KUcolQJBcimG/eoQgIKck6OO6AvROSDDQCkRIsxDijCtykFIsx0wpGmAqKdC7nBNmmKkRIyxljvBGJMHgkwHeJD+A8YgTwLdrG0MsM4lxlgpHiIrv4EtATAjQCsbQTZFizH2JMWYiR4ibFtJCDUk7ygShVHSLEuJCQEiQIOZLTNq8MnQEyDgLIBIci9JttUGoIBalTHCZU0pZTymH5KVUQpjeD6jLd

yQ50QQmjNBaEE1o/gpadC6QonpbTekcggG9qA73BlDOb9AuB4hz9S4v29h4kwIFPEaBYhfPg8Fd+UcsBZuBfD3fmKsNY46m4iQVufuKwS6GuHYXYT+ik/YCAg4AKY4WQZMU4iYU6c4rcMBm4jYxwSQywFw8Q4Itou4+4d+aB5QmIJ4PYsBF48BV4puEglOD8UEcQkwH2wEUY3CDB6ATB00LB2gbBQEc8Q6fao6ouP+1upwkwiQYIpw9ua4oee+M6

c6+AC6EBHQvQR6G6VQ26u6ZY+65gBAWhJ6NEcA56faT4+opAR+J+D6dWz6r6VQvBNk/Bghc8HBAuNEwuY63AUmdBkuV6Muae8u2giuKuaurY1BGASIAAmqZCAaZA0IbvAMbiumbtwH7ssAISMCMEXmCAQasGcH8O7u8HECkCWA7quLMGCAsL7mHmPhnvENuLaKnnLtwE2OXpAKXuOp0QPiPmiBiDXhIHiPXkSI3uSCljSC3t0OQB3qyOyHoVOkpA

KNPlULPhPiqDKBHnKGgB/kqJPn3jPlFEPraL3LfqgKWFOqaGSKvlaL0Zvs6K6B6F6D6IftQbYVOiGDUOfhALgDwNfjGAaAeOQQII/tQQng7qsG8Jbn/tZM8CkEQVOl/gARwLWEaNcLMO/okMcL0W2J2MEEuL2HAQgalkgTHKgfeugfOIuFgSuGuDkeUT7juCGKQcfvfpLseCiNQeLv4eUGkRIHZEpnzOwZ9rUNSBEjMq8l1HoE5JwCgaxCEEZFTI

QOFMwtgMEKgJIGwMiLIu2oJITG8lSNNiQj7JDFJqIn+gMuiLKfqJOJ5q+DRM/DhNWDZjTCGPQBiIwMQI5s9PeIvMENKsEC+NhAAOJUhoTaqLgGCZLgqcxRB2ncyJwjjaDKDaC7RPK6BxnWB5DaB5BlJ9q9A6brL3itRBI8q3SmgvjegGDfQLGSRDzfr4DSkGBwBym5DlZOZeZBwXqFnZQun6hvKZp6T6DWBCAECqHfRlkhQVlyCKSIj6C1luY0RM

qfhNkdnZQextTsSea4CoTOD4BbmuKeZpRIimH7mHlowDIIzUpiBFmdm7lwDUDpmEDaCvlplnI0rjY6Kyx+Tan4z0KJxAwjhciqlQDYQ+mAVhCCg0ngUDQDScEUCOECmKlQDClCEAZtrikHSSkKwymtm2l5nOIoXKmqkwDqmCRak6kmkakGlIzsTBTUXZzmm/pZYtltmEXrLZQOnRnOlDzKzumekpgQXNT+ktBSzBlhkOiKwTKOkxnNQyRfwJmcX3

jJmpnPmZkd7xk5kcXyUFk3h3m+mM5TnLKVlzk1nzFLkNmrnNk2lkzrmQXdl6V5B9lDwDldRDmESjnjkxSsjGUzlVnzmLlfTLmNmqoDKbkwDbmJnNQPnnmRVHnRUTQnlnkHnxWXmcrdYoaJXZQPlPm7kvlvlUpWxfnrn3i/k6kAXNRAUgXfiwXZWAxQVRA9B1VPTwVeiiE+G7G9HDpQCzqWSqHcDqEm43jGE6HJKLGf4GGHrromFnp/AXqWHXrvHs

nlCPoUIvrcEQCClGRoXuGilYXTJbZSmDa2UJngTbXkxECkXkWal/m6mmm0Waz0UDyMVmm1QWmsWnU6WdncVOnYQun8UcAen4BenCUTSiWBkkShnhnSW7SyU+nlRZlKXFmoCqVpn5UaWKXaUGWI25CFmeallnTTmIiznVkLkWXBVWW8g2X4V2WeYiSOV436UuXH5glY6eUBjeWTlE0mWk2BUU3kwhXWWlWoARVRXKXplwBxXi0o3JWmjS0JUZU3lZ

US25XqWFUfliAlU/l3WVUTTVWgUtUo1AzQXNU+ltUl6C7eHiFoB+HMn6hBFtFGihFK5lDK7gC75dGoSm3jrq7QASzZA6HBH9AMDNAUB/KTG0iDGt4QA4ggXx1cgh3So+RkwxpZA1RV7R216jEN7FAQDJ0LFp36AR3N5Z2MizGd4LFJ0iCF1PztorHqhHFihbD5012p1PwZ2j7bHj550F3t3p39GHFrHHHV0p25BF0uJ6hAlGgt193j1Pw1wr6wBr

7l6t1j1QBF31C5B9XzqDWz1t3z1ZBb1QBiHjpB773r1F2vqjUSC6Gj210D3fzviIgUASydTAmUmQBz0b1Pwjg0jP1sCv0hBRFsgv333936AAOIUpFVBMwh3wpsIAAaP+7+Ahiwge7wMwDuyw0wLdCD9iMR3AVwIw2gJwGwhRYw1w1w0hLdRg2p+g3A6ukADkvgPRWe5RJYPA4RvdB9P9WQLigJcYsDAKIdlIJAp98oq9YjxAvSZhaAiJkA0j7qf9

uAmgwQ3JxJed0j0xaATDEAtMGIUR9cpI2EPAm4T5ZjtwvA5jqAkwMeTEnIdkyggYbIsDygJjPA5RFjXjvAPjdjywDj3D5Q39ndCAi9B0FJLd++2QdkoYy0A8jDtoOQqj6jvhtUvJX9RAcjkk6Tfwh0gdttuTD6riUuaTYMQTkAdg9E34zA/Ih0cAyjh0ajMBPJLdSmRKCAC02p+AiTy6MDIomQES56p0jpC0/TbJIJEAlBXJZ4mjSx85CMAZB0sz

tBO41EPijAXTGIH9Ku4AqudA7KrowA7tyuQAA===
```
%%