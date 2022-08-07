

# Description
`amlScreeningAndMonitoring` enables customers to screen and monitor a user's risk level. It provides two modes: 
- a single Anti-money Laundering (AML) screening, and 
- a single AML screening and then adding it to a monitoring list for periodicity AML check.

In the *screening* mode, OSP will do one screening operation, and add the user (identified by `referenceId`) into profile list (or update if the user already exists) when the user (i.e., `referenceId`) is provided. OSP also generates a case if the screening result shows that the user matches AML list.

In the *ScreeningMonitoring* mode, OSP adds the user to the monitoring list if
- none of matched profiles.
- the transaction produces a `auto-approved` decision result.
Once a user(i.e., `referenceId`) is added to the monitoring list, OSP schedules tasks to screening the user, and updated its risk level correspondingly. 


# Request

- URI: https://{domain}/intl/openapi/monitoring/AMLScreeningAndMonitoring

- HTTP METHOD: post

## Request Headers
| field name           | description                                     |
|:------------------|:------------------------------------------------|
| X-ADVAI-KEY        | access-key for authentication. Ask our supporter for it when setup your account                                |
| Content-Type      | application/json;charset=UTF-8                       |


## Request Body
| name        | type      | require | example       | description                                                                                                   |
|:------------|:---------|:---------|:--------|:----------------------------------------------------------------------------------------------------|
| mode        | String   | true     | 1         | Single check or Monitoring.  1 Single check only ；2 Single check  first and do monitoring;                        |
| referenceId | String   | false    | "123"         | Unique identification in your system. `referenceId` is  required when `mode` is 2                             |
| name        | String   | true     | David         | Name of the person or the entity                                                                              |
| type        | String[]  | false   | ["Person"]    | List ["Person"] or ["Entity"]                                                                                 |
| gender      | String    | false   | Male    | Male or Female                                                                                                      |
| idNumber    | String    | false   | "13234234"    |  Identify card number                                                                                         |
| dob         | String    | false   | 1968-01-01    | Optional Date of birth, yyyy-MM-dd, not available for type Entity                                             |
| score       | String    | false   | 10            | The similarity between the name input with the name corresponding to the record, ranging from 0 to 1 (0-100%) |
| regionList  | String[]  | false   | ["Indonesia"] | List of country or region names to be searched                                                            |
| contentList | String[]  | false   | ["SAN"]       | The array format can be any combination of SAN, SIP, PEP, OOL, or OEL                                     |
| intervalTime| int       | false   | 1             |  Monitor interval time, default is 1 day.                                                                   |

## Response
| name        | type     | description                                   |
|:------------|:---------|:------------------------------|
| code        | String       | SUCCESS or Error  |
| message     | String       | Error message if code is not SUCCESS, otherwise it is empty. |
| data        | String          |  Empty(placeholder) 
| transactionId  | String       | Customers can query transaction detail by `transaction`                                                           |


## domain
| domain            | description                                     |
|:------------------|:------------------------------------------------|
| api.advai.net | Sandbox Environment                                |



```json
{
    "code":"SUCCESS",
    "message":"OK",
    "data":null,
    "extra":null,
    "transactionId":"e66542183ca218ba",
    "pricingStrategy":"PAY"
}
```

# Example

## Request:
```shell

curl --location --request POST 'https://api.advai.net/intl/openapi/monitoring/AMLScreeningAndMonitoring' \
--header 'Content-type: application/json' \
--header 'X-ADVAI-KEY: XXXXXXXXXX' \
'
--data-raw '{                                 
  "name": "David",                    
  "type": [                           
    "Person"                          
  ], 
  "referenceId": "kun0991412224124", 
  "idNumber": "", 
  "dob": "", 
  "regionList": [ 
    "Indonesia" 
  ], 
  "gender": "Female", 
  "mode": 1, 
  "score": 0.95, 
  "contentList": ["OOL"], 
  "intervalTime": 1 
}
'

```
## Response
```json
{
    "code": "SUCCESS",
    "message": "SUCCESS",
    "data": null,
    "extra": null,
    "transactionId": "c90f4af530ec30b9",
    "pricingStrategy": "PAY"
}

```
