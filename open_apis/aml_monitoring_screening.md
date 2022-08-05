

# Description
Global AML Watchlist Is A Database Of Different And Various Lists Around The World And Companies Or Financial Institutions Use The Global Watchlist To Operate Customer Identity Checks On A Regular Basis Against Risky Or Suspected Individuals Such As Money Launderers, Frauds, PEPs (Politically Exposed Persons) Or Terrorists.


# Request

- url: https://{domain}/api-wrapper/api/


- method: post
- Content-Type: application/json;charset=UTF-8

## domain
| domain            | description                                     |
|:------------------|:------------------------------------------------|
| sandbox-oop.advai.net | test environment                                |


## input parameter
| name        | type     | location | require | example       | description                                                                                                   |
|:------------|:---------|:---------|:--------|:--------------|:--------------------------------------------------------------------------------------------------------------|
| name        | string   | body     | true    | David         | Name of the person or the entity                                                                              |
| type        | string[] | body     | false   | ["Person"]    | List ["Person"] or ["Entity"]                                                                                 |
| dob         | string   | body     | false   | 1968-01-01    | optional Date of birth, yyyy-MM-dd, not available for type Entity                                             |
| score       | string   | body     | false   | 10            | The similarity between the name input with the name corresponding to the record, ranging from 0 to 1 (0-100%) |
| regionList  | string[] | body     | false   | ["Indonesia"] | optional List of country or region names to be searched                                                       |
| contentList | string[] | body     | false   | ["SAN"]       | The array format can be any combination of SAN, SIP, PEP, OOL, or OEL                                         |

# Response

- Content-Type: application/json;charset=UTF-8

## output parameter
| name              | type   | require | example | description                                            |
|:------------------|:-------|:--------|:--------|:-------------------------------------------------------|
| code              | string | true    | SUCCESS | SUCCESS:deal request success  ERROR: deal request fail |
| message           | string | true    | OK      | "OK" or error tip                                      |
| data              | string | true    |         | business data                                          |
| data.data         | json[] | true    | David   |                                                        |
| data.fullMarksNum | string | true    | David   |                                                        |
| data.profiles     | json[] | true    | David   | aml profiles                                           |

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

# Example

## request:
```shell
curl -X POST \
-H "X-ADVAI-KEY: 8cba0f54f2153sub"\
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
https://uat-oop.advai.net/api-wrapper/api/id/aml-watchlist-combine/callCombine
```
## response
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
