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

User create:
-----

Endpoint:
Files and logic breakdown:
Response structure:
 ^dIOn9gDn

%%
## Drawing
```compressed-json
N4KAkARALgngDgUwgLgAQQQDwMYEMA2AlgCYBOuA7hADTgQBuCpAzoQPYB2KqATLZMzYBXUtiRoIACyhQ4zZAHoFAc0JRJQgEYA6bGwC2CgF7N6hbEcK4OCtptbErHALRY8RMpWdx8Q1TdIEfARcZgRmBShcZQUebQBGABZtAGYaOiCEfQQOKGZuAG1wMFAwMogSbggATgAtYmqASQAxeiF0sshYRCqoLCgO8sxuZxSAVgAObR4ABmr4ngB2asT4

sbG1/nKYEZTEqYA2OcSxxJmD9fXqrcgKEnVuMZWbqQRCZWlueJmeMZfrZTBbgzF7MKCkNgAawQAGE2Pg2KQqgBieIINFowaQTS4bCQ5QQoQcYhwhFIiTg6zMOC4QK5LEQABmhHw+AAyrAgRJBB4GWCIdCAOr3STcPjFATgqEIDkwLnoHmVF6Ej4ccL5NDxF5sGnYNQ7TUzEESiAE4RwRrEDWoAoAXRejPI2Ut3A4QlZL0IxKwVVwMwZhOJauY1rd

HpNYQQxG41R4B0SPAmY0WfxNjBY7C4aESiRe6dYnAAcpwxNx4zNFvEJhNqilPcwACKZPrRtCMghhF6aYTEgCiwWyuWtdpeQjgxFwLa+ixnOZWTwmB2uJqIHEhrvd+BeCLxUe47fwnZNfUwAwkMMCk4QqAAqmEWAAdFzOF+vt9vp9P1Dfn/f+LaVAACUEAARyEcIoFQL9fxg2DcAAm9AIAGSgjhYPQjCf0IACFFwOBCAUeh4lsUhlGsQgjEnTMFCE

e8ImgzCYJ0VBsnUNhiAYxiuOw1AAAUAHk2QAFU4rjUGwADJBCYgmGYUSxPQnioGlDh5MY4gAO7YgYFQhTuIAjhcGyNS9KwnislwFkTNM1BCDsgCACs7AAfSgNRgms0zCHoAC3OyIxOAQTy9J81AIWCOS0PQuJUAAQSEdQcjcvA3M4YKYPg1BlOhNDsCkvFdNM5jEWUFjaMgzRr1xNzGBMlIAPitjSAoqi0qivTMohNgoFw4h9C9VBaKYBR0t/Zi4

DYCgmEG+9Cps2CeN4ph+pDTM0H0axomvO9ZJM5JUAANQIEhWo4SKbMywzjPa+bfx44CwMIQIOJusTmIsqzXtu2yAIeoQnqjUbuJ4m8OEIMCgq+267J4o6PFQD78FQXBiDIdUTLGBrsFSs6ge/TLsEvPpb1mpLSBgPHUGYtkmCsIgjGvSmJNQGniSA8IJrOyGMIOACACFSuqzNKcypDzByMJXPga93Gwd1Tsp5iEuwZy2EZRkwkg2X5Zx7QmYA3jQ

mYChEWIVBlByJgFahzCNNQfiOG7WlHA4UrEaytgZqYEzFl+jnODCJ8A0oIT+iqC8QmJnbH2fd845fT8bf/dmIbBOaFMyxCUMpjCeNw/DCOIkryMonGaLokabYw5jWMkdic8Ug3BJEqv0OZqSUd21u9KUlTKbtrSdIbhaDKM7nvru8yNs+ifJ54pzNFc9zx9n2zQr8hAArVYeYNC8LwhMmLGqk3JzGti7fJU8T8shdO3oAkqyrTyrkex7yV5g+q4o

SuvmtL4Xu4ZQAl1HqKN+poSGqQSuNlxqTWmpAu+31FrLUIKtTg61NoWxJl3DC+04YnRxudUyl0x47zun7R6z1FYAURmQrCFD/pUMAWJeyt4wYQzod+GGAF8Fm3dijNGIYMZY0ISLAChNI7bVJrkcm1CWa02OgzRBjFmaszNsBakAcP6/l5qgAWr8cZiNQGLMQXMpaIHEgQOW+Bz5FQAsrVW6tNaWPwNY06etmE/mZobEMJtSBmwtmqcghjPHfjtg

7J2/ivRu2nkjZSXtSA+z9pormQcHScCgGyCi4heDGk6EyDJzQjIsgNKgCYLwTxQFikQZQWZ0DBEZAMPMTBkoEGqe8Op0AdQMj0LkSyQSXRoDDFuE0iJ3hegIKHU84ciZSK7vHBZicMLJz+hBZRmFM7IXWTZPOeECJERImRMG/9ODl1ktAuxLEEBsReqvLhTdhL61QB3GSMc7k/Q9jlfuml2JD1CZhHiV1tFIKnpZfAnDbKsIXkvKAHl/m53XoQfy

gUIV73hAfG2R8f5JTPiEi+nycjXwQAVORj99DlSplVN+tUbZf2PmMk5ql/mdTYN1Xq4CEkXL0rAqapAEnbK8gbFBaCOAYMMlg6ORDYJ4OOhOURzLR7XXefdUCjDAb/PerEiFKrKHquVSDdh4FtWsN4QjWJyNUaBCETbTGcU34ByMRIq82C+VkwphqgCNNmqKMZv81ROR1H+1STbXR+ihZtXxSYiWCBzEyysTrABMD7FQBVmrDW1yXFuN1k8nxxtT

bm0tsExNplwmOzYM7aJZqwUewSUk9mKTA5cH+AlNgGjsncHBOBbcXoEAAAk3gfDPKgf8vxigAF8tilHKJUCQIFGSAQbAATQANIHEFAyboOToBhxeMMNAKRFj1R4HsMYexFgHBTFWU4LxSmjDONoRYMwD3VAOIuWs8QDhJBeHcYgDw0BPFzCaKS7xPhoCTHk8oAJ5QQclAKWE8JEQogxOiJAXZcT4kDCSBD5J0CUjOjSOkTSTTMlZLKeUEBFTRlBF

KIUIoxTUbg2RrdlGAzCFVOqL42pdT6i+EaF4ZoxyWmHPaYjToECDNQMMz03o93oFwPEVjRJiDBlDJuUECA9xoFfTwRMsxazNIzJwR4BmCwcGLBwUsWmjiftODMVM+TUFNmCFONsHYEBdh7MQfsWQkrCdHOOK8rZh0zkWHORIRxxgHG7WuDc4Z8k7mhEFg8R58mVKqL2RwkFJVPgWfHJZ6EVmqrWUYrOAqe44T2YXQ5JdTpnJYFyhSNdrl11uXcxa

N4W42XbtJHBbXL5fP+QPX5ZWFKAtIfCxS88XJuVhcC6GrCN5bzm/NbywD0VSpglixKp8UrFo6v1wleViW31JaRJ+FUqU1WW3Sn+DLbH7bCqy0BfUBqQIa/fVAE1eX8u1UK0gK1TNiq2i6jbv4ZUeFOqDriJClV9ZTmq1rsFbWxXtbjBVt4AtR2keCd1Sb5Hevpr6rrnqA31s5o2nm/NBao6MVGsxm7M0JojZchxabnHaxsdmm2vsydaLSSacgFAp

lDogBltQIOcu5ffPl2ChXU6QRK1s37qB877KLqRGrZdIH0Q9Vcm5yveIdaeS83rq9e4DZskN7SI2WGKuWzsqbi8Ztwv1UpJFm8UUTYWmiiKh8GrYp2/djOB3co3xtz+YqZ3yXP0u+/Oq/umotTxcQtbbKwGvfvO9riPL4GzQN8KwHLFMFzLeehcHBDMxQ8YjD+3gr4cA0RzBZHqOq8bIQpjkvCMZG48uV6um2Tw/fn9WzDR5Pluhup8nh7dPJYM4

50Hj7rOnEZvn1zjCPPR986bcRjJWSGZihgwU3IRT+r4FKeU48/R2m1KqA0oj+T0ytPwNfzpyk4A9Iyf0pgEmpOjOav4SZMOCQMXLLOiSXKXV8GXGCOXcCNORXbOL3GCXZAuA5YuY5WrbXLPRiJrfXRA8hPiI3P1SSHrUvM3EPb5KmYbZXIFY1RyabZeY1N3ZFbePArCH3DFDCLbE+VpKfYPAlUPY7QfKmB+KPClF+IWGlDCG7RPRlIxEBdlDPYaO

RL7XPaafPf7VBQvDacVEvVvcvOVSvIxGg1g+5evJhDCZveVfFG8DvF1LvHHORPvH1IQ4fQNBtcfKnAxPbPgmfGNOfeNTnbwxfFNRxdNLWAI9xOtTfYNBkXAFtNtffNATtdzFcHtftEDIdEdMYcdSdE0GddAd4KAIiXAXsd/CpaWXoHdE0WTb4aoTGCsCsM4PTcLRcG9XYe9R9Z9V9JcFINYaoRYb9Ojf9Z4IDAdUDVAZMezSDV2aDBjaUUkRDCQV

EFDTENDPEATYkBYnDaAcgfDWkJKBkEjdkTkZjeEJUCMGjBAYUX9UUNAcUfJfkaUJjKoFjZUNjSQFTTjUZbjWAXjQ/ATC0K0QoETfJR0MeH/NTPImTX0HgRTIMDjIZSEh4jTILQ9eIF9RIFIY9EzTMbgQDB/WSTMczSzYdMYHgPo+YCsesJza5TTVAZLFI/JbsJTbzQcPIYE/zOVOk+IELMLA4FICYA9aLdcREuLcoBLOkhkipIA9AAAcQzWqSRmy

1jggOcCgN/BgOK3R1K2V1VyqzQKT2okwLkVrnrhMI+VlN7E61Mm607lINnnNxyD92/m2x4KCOhxDyJRJV1zJTENj0kPQmkN/kNOZwe3kPTwgUz2ULgT5QQXUIBzWiLx0Ilxtn2hRysJT1QGaGuTyn5RkiiBZFb2rhJwJwHyeTUV52DQsOSTH353yUF2FyqHlMgkVJTNVMgKZWWQYS1OsKV3NOQLV2q3QK1wrhNOazNOVQAktOtL0ltNeSLLr2yid

MxQT24NxXdOr09KO29Lx19Jjy8IDNgiDLu14LEhZTTxe0jKUN1xUNjLz37L+wTPQSTOB2VNwREUMPR2zJTUkDzOuTBQXNgmpgUUJxcJJxHyDQp3QltWiMbUON33bTuMP0ZEKWKTP24Av1SyvxqU6TvwZEfzPmfxwt6G6ReF6QLIGSC1/3yTGQAPwEbIkGbLilZDbPbLVM7IK27LgO1L7MnJV0q1QI12HKNNHN11NMbwnh4mnONxIMAtG3IJXJdLX

N21DL4KXIEJ3MuT3IuwPOu1XJPI3Lb0ewvI5Te2jO+zjIfL4gL0TO0NfLANTI/IdS/JzN/IQXzIAscJArLKIPkQgvcOEUrLgubWUgSJyWSOiz7TGMyOmGyLKAnWKCnUgHyIgAAEV8BeJeIxgbxJBZSN0KiKQqj8kaiJh70z0Jg5h0Skxei2i0BRgjhphxgeAX0fgn1Zh8Tygf0/1UBtMXhgNB1uBFhfh/gZiclD9HjoQtikMVjUMTQcR1jMMpqKR

djqR9j6QHQWRji5RTjeQ5jaMbj6MLjGMTiXiziqMBd3jPjNQuNcQeNDR/jCRAS/NRNwSqKkTp1oSJBcA0g3ilMrrJN3qBAUSD8qqlgeATgcSjNsxIazMSwclT1fhqh5geTqTmxJS3MPMWSBxfMOSTQxwuSgseTZxEgVhKxvgOrIBVwRSAaxTKa2BdwksMbL9plGKM1o5UAmwCzDxwDVT1SfxNTuLeyEC+K9TBKjkQybBMCAB+QIPURAAAXjHNwL4

ukt8pN3tMkoUs4NXJxRUo4oe3Uq9JOx9NEP3IkL0qUoMtUrPNT2e1MqjJvJjJ+ysqWg0JFSBwlQcvfJZjHAmlIBbHZjlvCGQCMU5s8t10AnWyeRvFlLkt/DtkAhhDjp/AQAAgAFl2J3QODoKnK0d8Vvzcz3L/zCyqZQgoxUBOAwpiVCBEAvLSylFyzSdYKPCgrIZg4hcZSIAmL2aw7CyeaIC+a/wuKFceLha4dRb1dxbGU6sIgZbq6FalaWtldVb

idnlZLld1LnTj5daF8PT+CjahDI9Spo8dLzb49La/5d7NzjK7bFCoFzLVC+V4zNDbLi8Uzva2RfbEQA7gIg75BQ7i7uaI6o7fKY7k6wlfok7KZU7UAM7iAs7W9LDPz87XK/yubmBS6wgzZK7Zaa7a8gKSz+8G7fKKzm6TJdEyHt9QSELEjcl0lj80Lz9pTTwX9b8EBGl8KWlCLWGKRSKTRyKv9SAITaaIBaKJl6LO7u7Zpe6gG2L2KTIBaR6hahD

c4KsUDJ7NcRLzk56g7FaxLxyJKkEpyrSZK7TwGAUtboodbA9Ty97DbtzjbdzTbT7qULb6VL7bHr7wzLzOUH67y1CXabLny7LPbTcYJ9pP64A/af757g6AH0G5FI7fdQHY6KDE7zGYG4GEHAr0zkHMyC63LZoPKS6cQsGK60JcHa7dcnDQLG7/LayQ0ayt9Yj4jwhEKspSAu1Ui1R0iBrNRYqcjEq8igsIAKBMBSBYobwmgDp8qehCrpld1BqjRtA

JgdMmqD0sSqwosTRb14x/wzhqwhqkw6iL0BiTQurbjUBH1MYdN4wzgEwX0JgUw+rorpwpg5hExMTmqDg4wPmRrAQxq9r4MyRprkMGR5qMNPMlrcMVqCMDiNrSMTruQzq+RLjrjur7jygJqZQkWFQUXfr2MQwviaKfjSlvgHrzQhNcbQSxNhGRkHNPq5NEg4TlMESab6WsXga7inn5gL0SaYaD9lwCTDNYaLMclxgkx1h4gUghXp1Gw0bGbDxGTyh

mS+xsahxqXyh8bAtpxia6iUgjhKxhTYsOW6aGb9wmasKWb0B2bYpeJGh8hlQQ5O7bX7XHWd9cg98ckkg4hEhQslhwsD0jQnm6wPWoAT8SkMLmGqliKJA8KDMn8eHcM+H8kBGe0hG3qRGxHDIJHrWIBXWHWWnQq2naGIrumoqMivgBn4rciHMRm2AYRFgDpexEgDpnI4BDI/RF18B9BnBYpNAhB2hyi5n6ke0GRZMDglxtASaJgeTZ2L0Ug7NZXIB

b19hfZzgZgnnJ3axvnMLOqhirnSrtBT1MSn0kanh5hMXIB+rxj6pFgt24wFhbNlgMT/nZijr5jsNQWUNwX0MNisMQXlqqQ4X1riNNrnjkXdqP39qMWgWIO8WoP6zLq2WtRvjbrfj7r+NHqqW0ARwXrnRM3TWKhGWIBcADgWX/qkqugCreAJQErkS6TZhwaeTvhT0BXNRP0YbiSclSafgFh0TUbnN0alXMa1WfMNXcOQStWO9CbeSSbT1DXUP4svR

qbqLxT6bEsLWROTQJovR2TcOJQwAihOgygYNTPDOpOyhjOTP73kgT3F2ZW6ikaFgbgyhRgH0H3P1fgzgX34wLOJRLOIAbEwQ4R9B+oZAoxeI2A9OTXqNaQoA+YvQXZlBYuTQA1EviRolUuHioh/bYpSAIQKAXlsvygA18vCvivRSiOJoz9lAob6S3NBmygkqKgRmHIHJEgAApZQISAADWYEwBSEwF68kCMESFIEhGqFlMAmGGHa3VlqiFGvHZGBO

F9jfRrDsyNGPT2FqtQGcFfWqG0HmEPXOCeEXZ+bOfyQue4EXf6OPYU+O8/TswpteErc1HGF9nGEuH6L5arFDfySg0Beg+BcWPQGWLBbWMhaU2hZ2OA7Wvv3KCOPg4o3xeB/RcuavYo0uOR9eIur8A+JQ5ur1Aw+HT4xNABJw5tEC7BII5K+SpI9wEWAo7Zao+gBo5SDo/UzpMnfJIqqfSmMgHzFxLuI/S47ht40RvBsRsE9pMVZSxVc81ZJxsk85

J1c1Dk7qOapJsu/FJU7p6C40+E7CCa5KGGaqGXR4BAjgAOEZEhBvFma3TS0Wbqr2HqljEXY/WlZJoPS/R2a+HRPqJfQ2eqCTFmEncGIOo44F9e76dJ8P0B+BCBZh/B5/ch//Zh7w1WsI0OPA9xZR8Q6xbRYPcx+xZx9R6Q/x/+qU/KB1HQ7JbJ/yQp6BJV/w/E0I+kxklk1I4mGZ6Jaq656C3JMTB5KRp18F8JPq4WEx6F6LHF/e6WAqruej8cwV

a0/l+xEV/Vf06p9V5c2Cz1YA3PUPypv14lLl+Va6E7ojmdWVLkcTnboYvQGv6x3mTYvv/ocyXadmA/4jfQrA2jaTZBd2GCPcfv7W4axtk2ZRfhp/nTZ0ttQ/+cRo/wgDP9dC/dKXO/xNBxFi2rAUtp0wv5Bc0irzfpqOhrZDM62VQTEoBFrCYAKAsUB3pUQWbVEbuTzB9EuHJL3tVmfrA1i9zJazB6oW3J9PECrBJBwsmPa7lHxeZvc4+b7IHjlz

gzJ9kMqxOan+0WpfsgOexbPgiy2rkZcecg6UOj0Op6DoQZfAvpABVAE8++w6InndWkHk9sOzfHfq3zgFQlO+voaoL31UwiNIwQWJMENU/ShZq+oA0zBhWn4T9RWJJE7kkHWBGs8i8rITuf1E5eYt+z1fJNqz35E1Qs8nJGuMBe4n9++K4Q3gkOZoi4QCrFdshgPrLOs82pQ2/m/yZTwVPWX/ZCqhVPxMNihgA+NmmC4buBABb+D/H0lgHt8/84yH

NkgJqEOU7+9QkKq2hLbhU8BkVXpuMSyIm8WuKVeILkCEjYAQI9AWbseBo7bpGBxVf3nMBWZPoTgCncktej96ag+BR3TdjOGlabtei1YCPt1Q/TR8b2mRBvtMQBaJ9geCgmar+wWpQs1BMLOHpoLA6Ittqp1UwVjzgwGC7icHPProPKDmCq+1gknuSyw6UsHBeHGlq9X15JcfQX1Ogb9XhKWC1OQNRjucBlZkkkggQhgGEIwoMiZ+4Q71jzyGrjB+

iMvPflKTmqb9xO2/PEdJwJq6tMhLVA1nMGNb5D4shQtfvgLSys0WyLFWoeUKmEC4qhIuJiq2VVG811R1DRobQ2/5htf+bQq1jGw6RsMOGCbcAZaN4ZQDU2MAyivr2zaAE822olURMLqF1lIMrTHAXMK6bKcemRA4dNWzAD0dTe5AiQL2gOCLp+IkIJCHYHoHzMQBEAEqhsCO51FfB56MYDMCEF7sV2/vI9ssA+6JgLgawJMK8MuY8ltm+ST4V8GG

qYCluaAcapcQBEQ9lBwI6HqCNh4aD4WkI7QTtXOJGCrixfJEdCMg7DjURyHSwQyNr7E96+FLQTLiOp60shhDLVwV9T5geD9e3gmMDMH2Ays+O7HMpC91ZHccMKIfDYBVQE6xCaSvIy1gryxqCiUhIotXvv3FGfpZ2TzaUey23ByjXM2nc0U2TZrSNAGkUSYRUNRGaiQJoBaaDIwgnej9RiPGhjkmNEGjw2jDKNu0IgFADrRXQsAT0Nwl9CyKTo7/

OuJr4IDRhkjUCfBPAloDcsicH0ZACwEzD/RHaeYeW0WExUSB4Y2ttOhGbxBiAKQQsAgH4hQBAIyY3DEVSGBlhpWCQULPyWfQzg4w/3bYBhRrAJA8xSYfks1XPYvdxBpPKsNoEnYHijQR4n5h8JDFzttAHzUqliRfS/NYwMgv4SOPbGp9OxUPTYj2Mz4gdUxSPZEeX0L7wixxwPEwVOLMEzjrQc40ln8WxHLjXxkAGnm30JEM8YQO4mUZy25Lg09g

IfOomP0ZEisbuWJMXmKzFAfpz0KYFIGpOSpxDZe8oxIUrwk6ODUhMnMUXOGTDHdMeeQv8QUPNaAT1+BwkXHa0aCt13WlQjunmxGljSGhn/Whj62nb+tFJQbe4TVKPyYTWh2E80R0OAGcNCJbSYiSm3KBptnRmUyAK6NzbDT7WM06YWFQ4mBjdewYqQcsNIHNczeEgRYHqE65p1IQhYGmAcGXRsAEAMwegPgDhBsgbwvYKSQQLVDLd/0s7FZrmJOB

OdhBQpa4aT0xLTsZ2ZJEmj80nYGSD23wGcNMBrBjByw/JL3lZKkF3tqpJNY9IuBJo/AfuLklsUnx7Ep8lBTJFQSCMA5gi+xoHUErnwnEIcIpcI/QaFJHHhTzqFfQltFIxGLj4pT1TVklLXGpTNxcmBsBlNQCs9N0YoTnhGC5aoBVgTRZMIehPG9EzxYQi8XcVxkUyUad41fgNPwGqskhL45WRADSHckNenUnkt1L15nSDe/UhrkBPKC6dmp1nKzo

ZzABmdo5FnVzhHLKBEzfYiYE5mcApknpXOYAdzoelyn0z4w1QJmeen86dBAuwXKAKF3C4tgouMXAOWCHi4Zdku+vdLklyy61zcuVSArpNEq69T8kZXTuUV2kj68auMAOrnUgZIrD3p6AISDMGi5NAjQvXSEJgAOhp1+IqVZyGnQQCLB8AooOblUAW4J9nee3HMPVEmAHp72PRT9LGGj7n5T0HnUPlEPPSj8qxN3OYL7HWAGtHurHF7vWPe7JhUgl

wJ4JVNWCCkWZqAVsfIPZmKDZqXMrsd5N5m9is+/YwWVCJ0FBTYM4syPrwHHGoLYRaIwnmhwXFxS7BOIxKUyFVkByiRXfEolrJ1ns99ZDHILE8AXDNUaw5szYARNMzWzgsQghMGTOlY8ijezsgUWyVIWezZOB/ULAeIZE9TKRgczTk7InlRj0AxARoA7GqDKAGwVDcoLrJTFwzeAlJadnZh0xzA9mWzXbvtwrArMA+x6Mko5OxLnNi+rRUYs9OeZN

jfhrM/4ZAsBFp9VB8C3yfDxz4oKhx0s4KRgtg5hTApuCqKcSxr6xTMOxChKe7OSnOCNxxIuTM0C1myK9xaASRTplD6WyipmoIQaVJJJVgcwx6PMfGAEVFCmSwi5Xj3LfHpCQsTzbzvsG+GU1/ZDSs1vIuDmDTFRNrWaE6j6Ah0VS8jDgL2GJBhyoAIy5oCyHCDIw2YCIVQNgCpiXhIQxASaKKifDN1UA/IIQNjBEAIARlD/F1oMtmQjLpcTKCZcQ

CmUzK5lGDawGbCWXmBVlIQdZZspGU7K9lBywIMco/5esD8P/LCf/xwl2j6ku0m0URLBVdIHRx0siRmxdFUS3RIudmkMqOX91E41y25U+FmURQFlTytgMsteW4B3lFALZRwC+Wdofl6KrRSxL9HtMy2QYitrHxenhjwAIJUjlEw5BXhuAU6aAFJGyC35K2WwBgIQAQAUA+Y3M7sfAuRDqw5VjIQYBADlgFckojQPoPoA5AQKZVUCxVcqsIxqqsgkq

2BQB1B4IK/JuqkQPqvVW4rBxMIqcUqstWqr1VmqsJRjxFV6qnVWQF1cYMiX2qPVuQA1foEjqV98Fx0x1QGvVX8Q4ltgsNSqojVZBmgLQyNiCtjVWqE1qEwFcUAdVxqoAga4XDtPwmprPVGq9ueVy7mDzMp2atNfoGuVlqB5KMX0P3ItU5rA1daoSPsMwyKrmAhMeEPgF64jB6ZyQQNoKXRK5SqwIq7teFHwCLoYw56RqrmKYWHokaPwEVQFAMC8q

0wBAcCMCBMknBzgEwE3lWuLXBryR1oCAJ2pFUEgSAAKpCpeuajEAOQCAd/P+jvUkA4GCACZbgE0DBAalkAK9Sapwwtc+Y8IEZqRBxAAAKdgdQF4Ao0YNAxMBcewACUDIYCMoHljbEwNuASDYu2g0VLcNOGhDWMGQ2Hr/VmSS4lGsIQlcyFY8YCN6GaiuwN1vcwyN+rpKMrjpRAZ9R0wemQAgU90/AcIFhRjskieAw9XYAcjEo8gbIQyHAHfWfqWN

v60jq4yEisp8AjG7RfsLCDBBUcPScqAYHbUjtZFZ/BqaJgMA0wtNOMYzfFlCBVIlNKmk1uOnAD0cmQ9y3lWOhABjogAA
```
%%