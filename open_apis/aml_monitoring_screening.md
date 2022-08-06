

# Description
`amlScreeningAndMonitoring` enables customers to screen and monitor a user's risk level. It provides two modes: 
- a single Anti-money Laundering (AML) check, and 
- a single AML check and then adding it to a monitoring list for periodicity aml check.



# Request

- URI: https://{domain}/intl/openapi/monitoring/AMLScreeningAndMonitoring

- HTTP METHOD: post

## Request Headers
| field name           | description                                     |
|:------------------|:------------------------------------------------|
| access-key        | access-key for authentication. Ask our supporter for it when setup your account                                |
| content-type      | application/json;charset=UTF-8                       |


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
| sandbox-oop.advai.net | Sandbox Environment                                |



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

curl --location --request POST 'https://uat-oop.advai.net/intl/openapi/monitoring/AMLScreeningAndMonitoring' \
--header 'Content-type: application/json' \
--header 'x-organization-id: 9' \
--header 'x-account-id: 10' \
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
