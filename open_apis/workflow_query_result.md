
# Description
Retrieve a transaction report by `transactionId`. 

# Request
- URI: https://{domain}/intl/openapi/journey/v1/query

- METHOD: GET

## Request Headers
| field name           | description                                     |
|:------------------|:------------------------------------------------|
| X-ADVAI-KEY        | access-key for authentication.  |
| Content-Type      | application/json;charset=UTF-8                       |

## Parameters
| Parameter Name    | description                                     |
|:------------------|:------------------------------------------------|
| transactionId        | Transaction Id returned when you submit a task.  |


## Response
| name        | type     | description                                   |
|:------------|:---------|:------------------------------|
| code        | String       | SUCCESS or Error  |
| message     | String       | Error message if code is not SUCCESS, otherwise it is empty. |
| data        | String          |   | 
| data.caseNodeList |  Json[]| Detail case information produced by workflow decision engine. The order of nodes (i.e., API, scorecard and list) is in according with the execution sequence. | 
| data.biometricCheck | Json | biometric check result if it is included in the digital onboarding journey | 
| data.documents| Json | OCR documents information if they are included in the digital onboarding journey. 
| data.appFormInfo | Json | The filled form information by a client if it is configured in the digital onboarding journey | 
| data.attachmentFiles | Json | Additional documents information if they are configured in the digital onboarding journey | 
| data.referenceId | String | ReferenceId if present | 
| data.result | String | Decision result: APPROVE, REJCT, or REVIEW, or null if errors during the workflow  |



## domain
| domain            | description                                     |
|:------------------|:------------------------------------------------|
| api.advai.net | Sandbox Environment                                |
| api.advance.ai | PROD Environment in Indonesia                               |



# Example

## Request:
```shell

curl --location --request GET 'https://api.advai.net/intl/openapi/journey/v1/query?transactionId=19d1fa4666fad51b' \
--header 'X-ADVAI-KEY: XXXXXXXXXXX

```
## Response
```json

{
  "code": "SUCCESS",
  "message": "The request was successful",
  "data": {
    "caseNodeList": [
      {
        "nodeId": 7721,
        "nodeName": "3:Email Detection",
        "nodeType": "SERVICE",
        "status": "APPROVE",
        "data": {
          "apiName": "EmailDetectionV1ID",
          "serviceType": "API_CHECK",
          "data": {
            "rules": [
              {
                "hit": true,
                "action": "Proceed"
              }
            ],
            "results": {
              "code": "SUCCESS",
              "data": {
                "deliverable": true,
                "account_details": {
                  "github": {
                    "registered": false
                  },
                  "lastfm": {
                    "registered": false
                  },
                  "spotify": {
                    "registered": false
                  },
                  "facebook": {
                    "name": null,
                    "photo": null,
                    "registered": false,
                    "url": null
                  },
                  "foursquare": {
                    "registered": false
                  },
                  "tumblr": {
                    "registered": false
                  },
                  "google": {
                    "photo": null,
                    "registered": false
                  },
                  "pinterest": {
                    "registered": false
                  },
                  "linkedin": {
                    "twitter": null,
                    "website": null,
                    "name": null,
                    "photo": null,
                    "registered": false,
                    "company": null,
                    "location": null,
                    "title": null,
                    "url": null
                  },
                  "instagram": {
                    "registered": false
                  },
                  "microsoft": {
                    "registered": false
                  },
                  "myspace": {
                    "registered": false
                  },
                  "apple": {
                    "registered": false
                  },
                  "skype": {
                    "country": null,
                    "gender": null,
                    "city": null,
                    "name": null,
                    "bio": null,
                    "photo": null,
                    "registered": false,
                    "handle": null,
                    "language": null,
                    "id": null,
                    "state": null,
                    "age": null
                  },
                  "twitter": {
                    "registered": false
                  },
                  "vimeo": {
                    "registered": false
                  },
                  "weibo": {
                    "registered": false
                  },
                  "yahoo": {
                    "registered": false
                  },
                  "flickr": {
                    "registered": false
                  },
                  "gravatar": {
                    "registered": false
                  },
                  "ebay": {
                    "registered": false
                  }
                },
                "domain_details": {
                  "expires": "2030-07-27 02:09:19",
                  "created": "1995-05-04 04:00:00",
                  "suspicious_tld": false,
                  "custom": false,
                  "valid_mx": true,
                  "accept_all": true,
                  "registrar_name": "MarkMonitor Inc.",
                  "registered": true,
                  "registered_to": "Shenzhen Tencent Computer Systems CO.,Ltd",
                  "tld": ".com",
                  "disposable": false,
                  "spf_strict": true,
                  "website_exists": true,
                  "domain": "qq.com",
                  "dmarc_enforced": true,
                  "free": true,
                  "updated": "2021-08-23 02:30:04"
                },
                "email": "81557999@qq.com",
                "breach_details": {
                  "first_breach": null,
                  "haveibeenpwned_listed": false,
                  "breaches": [],
                  "number_of_breaches": 0
                }
              },
              "extra": null,
              "pricingStrategy": "PAY",
              "message": "OK",
              "transactionId": "19fa551d89e3cb90"
            }
          }
        }
      },
      {
        "nodeId": 7722,
        "nodeName": "8:new list(10372)",
        "nodeType": "LIST",
        "status": "PENDING",
        "data": {
          "results": true,
          "action": "Pending"
        }
      },
      {
        "nodeId": 7723,
        "nodeName": "14:0092",
        "nodeType": "SCORECARD",
        "status": "PENDING",
        "data": {
          "scorecardName": "14:0092",
          "totalScore": "412",
          "filedAndScore": [
            {
              "filedName": "EMP_S",
              "score": 52.0
            },
            {
              "filedName": "TU_LEG",
              "score": 46.0
            }
          ],
          "messageMap": []
        }
      },
      {
        "nodeId": 7725,
        "nodeName": "end",
        "nodeType": "END",
        "status": "",
        "data": null
      }
    ],
    "biometricCheck": null,
    "documents": null,
    "appFormInfo": {
      "sc_adb_619_pre": 0,
      "sc_lir_619_pre": 0,
      "Email": "81557999@qq.com",
      "sc_oid_619_pre": 0,
      "sc_wky_619_pre": 0,
      "sc_tu_gai_619_pre": 0,
      "sc_tcl_619_pre": 0,
      "sc_emp_s_619_pre": 1,
      "sc_tu_dpd_619_pre": 0,
      "sc_tu_leg_619_pre": 0
    },
    "attachmentFiles": null,
    "referenceId": null,
    "transId": "19d1fa4666fad51b"
  },
 "transactionId":"19d1fa4666fad51b",
  "timestamp": "2022-05-19T04:14:31.378+00:00"
}


```
