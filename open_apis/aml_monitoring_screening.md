
**WIP**
# Description
`amlScreeningAndMonitoring` enables customers to screen and monitor a user's risk level. It provides two modes: 
- a single Anti-money Laundering (AML) screening, and 
- a single AML screening and add it to the monitoring list for periodical screening.

There are two concepts with this API
- *Monitoring List* A user list to be screened periodically. 

- *User Profile list* A profile, identified by `referenceId` in OSP represents a user, by a number of fields, such as name, age, address, id number.

## Screening Mode
OSP only conducts one screening operation. In this case, OSP also generates a case if there exists a matched profile. 

## Screening and Monitoring Mode
OSP first conducts one screening opreation, and adds the user to the monitor list depending on the sceening result. The effects of screening result are shown in the following table

| Matched Profiles| Case Generation   |  Case Decision   | Add to the monitoring list             |
|:----------------|:----------------|:---------------|:---------------------------------------|
| none            |no case, and set transaction to `auto approve`              |  |  y | 
| y               |y*                | Approve manually    |  y  | 
| y               |y*                | Reject  manually     |  n, and remove from monitoring list if necessary| 

Rules for case generation in this mode are: 
- Existence of any change in the fields (i.e., `primary_name`, `icon_hints`, `timestamp`, new matched profiles), when comparing screening result to the previous one for the current profile.
- Absence of user profile information.

Regardless of screening and monitoring modes, OSP would add the current user to our profile list if `referenceId` is provided.


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
