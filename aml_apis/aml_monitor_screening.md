

# Description
`amlScreening` enables customers to screen a user's risk based on Dow Jones's AML services. It 
- collects a list of matched entities/persons who match query criteria;
- filters entity profiles according the score similarity threshold.

# Request

- URL: https://{domain}/api-wrapper/api/id/aml-watchlist-combine/callCombine
- Method: post

## Request Headers
| field name           | description                                     |
|:------------------|:------------------------------------------------|
| access-key        | access-key for authentication. Ask our supporter for it when setup your account                                |
| content-type      | application/json;charset=UTF-8                       |


## Request Body
| name        | type     | required | example       | description                                                                                                   |
|:------------|:-----------|:--------|:--------------|:--------------------------------------------------------------------------------------------------------------|
| name        | String        | true    | David         | Name of the person or the entity                                                                              |
| type        | String[]      | false   | ["Person"]    | List ["Person"] or ["Entity"]                                                                                 |
| dob         | String        | false   | 1968-01-01    | Optional Date of birth, yyyy-MM-dd, not available for type Entity                                             |
| score       | String        | false   | 10            | The similarity threshold between the name input with the name corresponding to the record, ranging from 0 to 1 (0-100%) |
| regionList  | String[]      | false   | ["Indonesia"] | List of countries or region names to be searched                                                       |
| contentList | String[]      | false   | ["SAN"]       | The array format can be any combination of SAN, SIP, PEP, OOL, or OEL                                         |

# Response


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



## Domain
| domain            | description                                     |
|:------------------|:------------------------------------------------|
| sandbox-oop.advai.net | Sandbox Environment                                |



# Example

## Request:
```shell
curl -X POST \
-H "X-ADVAI-KEY: XXXXX"\
-H "Content-Type: application/json"\
-d '{
   "dob": "1968-01-01 00:00:00",
   "name": "David",
   "type": [
      "Person"
    ],
    "regionList": [
       "Indonesia"
    ]
}' \
https://sandbox-oop.advai.net/api-wrapper/api/id/aml-watchlist-combine/callCombine
```
## Response
```json
{
    "code":"SUCCESS",
    "message":"OK",
    "data":{
        "meta":{
            "count":"30",
            "first":"0",
            "last":"0",
            "total_count":"30",
            "screening_context":"H4sI*******AAA=="
        },
        "data":[
            {
                "id":"1072591",
                "type":"RiskEntities",
                "attributes":{
                    "type":"Person",
                    "primary_name":"Wiranata, David Kurniawan",
                    "title":"",
                    "country_territory_code":"INDON",
                    "country_territory_name":"Indonesia",
                    "gender":"Male",
                    "is_subsidiary":"false",
                    "score":"0.95750004",
                    "icon_hints":[
                        "SI-PERSON"
                    ],
                    "countries_territories":[
                        {
                            "type":"Resident of",
                            "code":"INDON",
                            "iso_alpha2":"ID",
                            "iso_alpha3":"IDN"
                        },
                        {
                            "type":"Citizenship",
                            "code":"INDON",
                            "iso_alpha2":"ID",
                            "iso_alpha3":"IDN"
                        },
                        {
                            "type":"Reported Allegation",
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
                            "name":"David Kurniawan Wiranata"
                        },
                        "type":{
                            "type":"Precise",
                            "is_linguistic_variation":"false",
                            "is_non_linguistic_variation":"false",
                            "is_structural_variation":"true"
                        }
                    }
                }
            }
        ],
        "fullMarksNum":"0",
        "profiles":[

        ]
    },
    "extra":"",
    "transactionId":"e66542183ca218ba",
    "pricingStrategy":"PAY"
}

```
