
**WIP**
# Description
**amlScreeningAndMonitoring** enables customers to screen and monitor a user's Anti-Money Laundering (AML) risk level. It provides two modes for a user: 
- a single AML screening, and 
- a single AML screening, plus set up a configuration for later periodical check.

Inherently, two concepts are related with this API:
- A *Monitoring List* consists of users, each of which will be screened periodically. 

- A *User Profile list* is a list of users, and each user is identified by `referenceId`. OSP portrays a user with a number of features, such as name, age, address, and id number.

## Screening Mode
OSP conducts a single screening and produces a matched profile list. A matched profile is a profile with a greater similarity than the predefined threshold. 

The execution of the API with this mode will generate a case, so that operation team can review it, if there exists a matched profile. Notice you will only get a transaction id when you execute the API in the console(or other HTTP client). To check the detail result, you have login OSP portal, and retrieve the transaction and cases using the transaction id. 



## Screening and Monitoring Mode
OSP first conducts one screening, and adds the user to the monitor list depending on the screening result. For the screening, the effect is the same as that is described in screening mode, while the effect of check in the later monitoring tasks is described in the following table

| Matched Profiles| Case Generation   |  Case Decision   | Add to the monitoring list             |
|:----------------|:----------------|:---------------|:---------------------------------------|
| none            |no case, and set transaction to `approve`              |  |  y | 
| y               |`y*`                | Approve manually     |  y  | 
| y               |`y*`                | Reject  manually     |  n, and remove from monitoring list if necessary| 

Rules for case generation with `y*` are: 
- Existence of any change in the fields (i.e., `primary_name`, `icon_hints`, `timestamp`, new matched profiles), when comparing check result to the last check result for the user.
- Absence of user profile information.

Regardless of screening and monitoring, OSP would add the current user to our profile list if `referenceId` is provided.


# Request

- *URI* https://{domain}/intl/openapi/monitoring/amlScreeningAndMonitoring

- *METHOD* post

## Request Headers
| field name           | description                                     |
|:------------------|:------------------------------------------------|
| X-ADVAI-KEY        | access-key for authentication. Ask our operation team when setuping your account                                |
| Content-Type      | application/json;charset=UTF-8                       |


## Request Body
| name        | type      | required | example       | description                                                                                                   |
|:------------|:---------|:---------|:--------|:----------------------------------------------------------------------------------------------------|
| mode        | String   | true     | 1         | Single check or Monitoring.  1 for single check only ；2 for monitoring;                        |
| referenceId | String   | false    | "123"         | Unique identification in your system. `referenceId` is  required when `mode` is 2                             |
| name        | String   | true     | David         | Name of the person or the entity                                                                              |
| type        | String[]  | false   | ["Person"]    | List ["Person"] or ["Entity"]                                                                                 |
| gender      | String    | false   | Male    | Male or Female                                                                                                      |
| idNumber    | String    | false   | "13234234"    |  Identify card number                                                                                         |
| dob         | String    | false   | 1968-01-01    | Optional Date of birth, yyyy-MM-dd, not available for type Entity                                             |
| score       | String    | false   | 10            | The similarity between the name input with the name corresponding to the record, ranging from 0 to 1 (0-100%) |
| regionList  | String[]  | false   | ["Indonesia"] | List of country or region names to be searched                                                            |
| contentList | String[]  | false   | ["SAN"]       | The array format can be any combination of SAN, SIP, PEP, OOL, or OEL                                     |
| intervalTime| int       | false   | 1             |  Monitor interval time, default is 1 day.                                                    |
| phoneNumber| String       | false   |              |  Customer cellphone number must be inputted numbers. (country code + area code + number)   |
| email | string            |false  |    |   General email format with @ and . eg. example@example.com    | 
| address    |   String     | false |    |  Address must be inputted as detail as possible (RT,RW, City, District, Sub-district, Province, Postal Code) to get the most accurate result.  | 
| postalCode |  String      | false |     | Postal Code should be a series of letters or digits or both, included in a postal address for the purpose of sorting mail | 
| issuedCountry | String | false    |     |  Issued Country should be document issued country. Example: United States. | 
| expiredTime  | String  | false    |     |     Expired Time must be a date which is a kind of card that is no longer valid to use because its expiration date has passed,yyyy-MM-dd.| 




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
| api.advance.ai | PROD Environment in Indonesia                               |


# Example

## Request:
```shell

curl --location --request POST 'https://api.advai.net/intl/openapi/monitoring/amlScreeningAndMonitoring' \
--header 'Content-type: application/json' \
--header 'X-ADVAI-KEY: XXXXXXXXXX' \
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
