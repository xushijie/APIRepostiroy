

# AML Screening and Monitoring

## Description
Similar to the [AML Screening](https://github.com/Onestop-advanceAI/APIRepostiroy/blob/master/aml_apis/aml_screening.md), this API conducting one screening, and may produce a transaction for monitoring operation. 

In the implementation, it encapsulates [AML Screening](https://github.com/Onestop-advanceAI/APIRepostiroy/blob/master/aml_apis/aml_screening.md), but accepts more parameters, such as `mode` and `referenceId`. These new added parameters would be appended to the transaction, and indicate OSP to generates new cases and monitoring tasks. 

The effect of calling this API in different scenarios are list the table 

| Scenario           | Description     |  Transaction Generation| Case Generation       |  Monitoring Task Generation | 
|:------------------|:----------------|:--------------------------|:------|: ---------------------- |
| API Call          |                 | No transaction            | No      | No                      |
| Use in Workflow   |                 | Y                         | Maybe      | Maybe  | 
| Use in API Router |                 | Y                         | Maybe            | Maybe           |

Refer [amlScreeningAndMonitoring](https://github.com/Onestop-advanceAI/APIRepostiroy/blob/master/open_apis/aml_monitoring_screening.md) for detail rules for case and monitoring task generation.


## Request

- URL: https://{domain}/api-wrapper/api/id/aml-screening-and-monitoring/callCombine
- Method: post


### Domain
| domain                | description                   |
|:----------------------|:------------------------------|
| sandbox-oop.advai.net | Sandbox Environment           |
| oop-id.advai.net      | PROD Environment in Indonesia |


### Request Headers
| field name           | description                                     |
|:------------------|:------------------------------------------------|
| access-key        | access-key for authentication.  |
| content-type      | application/json;charset=UTF-8                       |



### Request Body
| name        | type      | require | example       | description                                                                                                   |
|:------------|:---------|:---------|:--------|:----------------------------------------------------------------------------------------------------|
| mode        | String   | true     | 1         | Single check or Monitoring.  1 Single check only ；2 Single check  first and do monitoring;                        |
| referenceId | String   | false    | "123"         | Unique identification in your system. `referenceId` is  required when `mode` is 2                             |
| name        | String   | true     | David         | Name of the person or the entity                                                                              |
| type        | String[]  | false   | ["Person"]    | List ["Person"] or ["Entity"]                                                                                 |
| gender      | String    | false   | Male    | Male or Female                                                                                                      |
| idNumber    | String    | false   | "13234234"    |  Identify card number                                                                                         |
| dob         | String    | false   | 1968-01-01    | Optional Date of birth, yyyy-MM-dd, not available for type Entity                                             |
| score       | String    | false   | 10            | The similarity threshold between the name input with the name corresponding to the record, ranging from 0 to 1 (0-100%) |
| regionList  | String[]  | false   | ["Indonesia"] | List of country or region names to be searched                                                            |
| contentList | String[]  | false   | ["SAN"]       | The array format can be any combination of SAN, SIP, PEP, OOL, or OEL                                     |
| intervalTime| int       | false   | 1             |  Monitor interval time, default is 1 day.                                                                   |

### Response


| name              | type   |    description                                            |
|:------------------|:-------|:-------------------------------------------------------|
| code              | String |  SUCCESS:deal request success  ERROR: deal request fail |
| message           | String |  "OK" or error tip                                      |
| data              | String |  Detail response body                                          |
| data.meta         | String |  Meta info for the matched entities or persons, e.g. total matched entities.   | 
| data.data         | Json[] | The detail list of entities to be matched                                                           |
| data.fullMarksNum | int |  The total number of matched entities with match score greater than the given `score` in the parameter                  |
| data.profiles     | json[] |  Detail profile list for the matched entities, the similarities of which are greater than `score`.               |

Main members for each item in the `data.data` list:

| name              | type   |    description                                            |
|:------------------|:-------|:-------------------------------------------------------|
| id              |  String |  Profile id |
| type           | String  |                                       |
| attributes     | Json[] |  Description for matched entities                                          |
| attributes[].type         | String |  `person` or `entity`   | 
| attributes[].primary_name         |  | Name                                                           |
| attributes[].score | String |  match score                 |
| attributes[].matched_criteria     | Json |  Match information for this match               |


# Example

## Request:

```shell

curl -X POST \
-H "X-ADVAI-KEY: XXXXXXXXXXXXXX" \
-H "Content-Type: application/json"\
-d '{
"dob": "1968-01-01 00:00:00",
"name": "David",
"type": [
"Person"
],
"regionList": [
"Indonesia"
], 
"mode":  "1"
}' \
https://sandbox-oop.advai.net/api-wrapper/api/id/aml-screening-and-monitoring/callCombine
```
## Response
```json

{
   "code":"SUCCESS",
   "message":"OK",
   "data":{
      "meta":{
         "count":"1",
         "first":"0",
         "last":"0",
         "total_count":"1",
         "screening_context":""
      },
      "data":[
         {
            "id":"11762490",
            "type":"RiskEntities",
            "attributes":{
               "type":"Person",
               "primary_name":"David, Fahmi",
               "title":"",
               "country_territory_code":"INDON",
               "country_territory_name":"Indonesia",
               "gender":"Male",
               "is_subsidiary":"false",
               "score":"0.986",
               "icon_hints":[
                  "RCA"
               ],
               "countries_territories":[
                  {
                     "type":"Citizenship",
                     "code":"INDON",
                     "iso_alpha2":"ID",
                     "iso_alpha3":"IDN"
                  },
                  {
                     "type":"Resident of",
                     "code":"INDON",
                     "iso_alpha2":"ID",
                     "iso_alpha3":"IDN"
                  },
                  {
                     "type":"Jurisdiction",
                     "code":"INDON",
                     "iso_alpha2":"ID",
                     "iso_alpha3":"IDN"
                  }
               ],
               "matched_criteria":{
                  "date_of_birth":{
                     "day":"",
                     "month":"",
                     "year":""
                  },
                  "name":{
                     "name":"Fahmi David"
                  },
                  "type":{
                     "type":"Precise",
                     "is_linguistic_variation":"false",
                     "is_non_linguistic_variation":"false",
                     "is_structural_variation":"false"
                  }
               }
            }
         },
      ],
      "fullMarksNum":"0",
      "profiles":[
         
      ]
   },
   "extra":"",
   "transactionId":"a2fe37821e69fdd5",
   "pricingStrategy":"PAY"
}

```
