
# Workflow Event Callback 

## Description
This page describe how to setup a callback in OSP and the required callback method schema to make the communication work between OSP and the customer system.

A callback method is public API, **provided in the customers system**, and its purpose is to receive events from OSP for the awareness of their task's progress. These events can be: 
- A workflow node execution completion. 
- Workflow execution ends (e.g. success, fail), and 
- transaction/case status changes.

OSP will automatically launch a request to the callback, if the event is triggered.

## Recommended Callback Implementation
It is advised that the callback method should be non-block. In the implementation, it pushes the event to a queue or publishes it to a message broker, e.g., Kafka or Rabbit-MQ, and then responses the client with 200 information, immediately after it receives an event. The advantage of this is to speed up event consumption and to alleviate OSP resource pressure. 

The callback method should also be a **idempotent**, as OSP might retry to push the same event repeatedly. The retry happens in the following cases: 
- OSP receives 5XX HTTP response code from a remote endpoint. 
- OSP gets a network exceptions, such as `ReadTimeoutException` and `WriteTimeoutException`.


## How to setup Callback with us
To setup callback, contact our engineer and provide the form

|  Application Form   ||
|:------------|:----------------------|
| login name         |      |
| Notification Type      |        |
| Workflow ID    |        |
| Callback URL  |         |
| Acess Key    |        |


Currently, we only supports static access-key for authentication. If you have more strong authN security requirements, contact us.

## Callback Schema

### Request

- **URL**:   https://{domain}/path/to/your/API  (Up to customers)
- **Method**: post


### Request Headers
| field name           | description                                     |
|:------------------|:------------------------------------------------|
| access-key        | access-key for authentication.  |
| content-type      | application/json;charset=UTF-8                       |




### Request Body
| Name        | Type      | Required | Description   |
|:------------|:---------|:---------|:----------------------|
| tenantId         | String    | true    | Customer Id       |
| referenceId      | String    | false     | a unqiue id identifies a user       |
| transId    | String    | true     | transaction id       |
| flowId    | Long    | false     | workflow id if        |
| notificationType | Enum String  | true     | Optional values: NODE_EVENT, WORKFLOW_EVENT, TRANSACTION_EVENT, CASE_EVENT       |
| payLoad          | Map      | true     | A general parameter input map       |
| timestamp        | Long      | true     | A general parameter input map       |




#### `payLoad` attributes


<div align="center">
    <img src="css.svg" width="400" height="400" alt="css-in-readme">
</div>


| Notification Type     | Description                                                                                                                |
|:----------------------|:---------------------------------------------------------------------------------------------------------------------------|
| **NODE_EVENT**        | • **id** <span style="color:grey">Integer</span>: Node id number in the workflow (This is only unique in a single workflow).<br>• **name** <span style="color:grey">String</span> Node name.<br>•startTime<span style="color:grey">String</span> Time to start this node.<br>• **endTime**<span style="color:grey">String</span> End to start this node.<br>• opTime<span style="color:grey">String</span> The time when a variable associating with the node. It is only meanful for a data node and its value is the time when OSP receives responce from a remote data sources.<br>• **cacheHit** <span style="color:grey">String</span>: `true` if the the data is from cache. Otherwise `false`.<br>• **input** <span style="color:grey">Map</span> A generic key-value map for input parameters.<br>• **output** <span style="color:grey">Map</span> A raw response from remote datasource, or local cache if `cachehit` is true. |
| **WORKFLOW_EVENT**    | • **status** <span style="color:grey">Enum</span> Optional values: APPROVE, REJECT, REVIEW.<br>• **stage** <span style="color:grey">Enum</span> Workflow execution stage, and current optional values: `FINISH`, `ERROR`, and `TIMEOUT`                                                            |
| **CASE_EVENT** | • **status** <span style="color:grey">Enum</span> Optional values: APPROVE, REJECT, REVIEW.<br> operatorId: operator id.  |



### Response


| name              | type   |    description                                            |
|:------------------|:-------|:-------------------------------------------------------|
| code              | String |  SUCCESS:deal request success.               |
| message           | String |  leave empty                                 |
| data              | String |  Leave empty                                  |

OSP only relies on ** HTTP response code **(200, 5XX) instead of response body's code here, and: 
1. confirm event have delivered successfully and then next event, if it is 200;
2. retry delivery with maximal 3 times if it is 5XX.



# Example

## Sample Request Body for `NODE_EVENT`:


```shell

{
  "tenantId": 
  "transId": "6be712d68c64058b",
  "referenceId": "1000",
  "flowId": "104",
  "notificationType": "NODE_EVENT",
  "timestamp": "1660727267"
  "payLoad": {
    "id": 28907,
    "name": "8:Blacklist Check",
    "startTime": "2022-08-12T10:12:16.827Z",
    "endTime": "2022-08-12T10:12:16.828Z",
    "opTime": "2022-08-12T10:12:16.828Z",
    "cacheHit": true,
    "input": [
      {
        "name": "name",
        "value": "SITI CHURIAH"
      },
      {
        "name": "idNumber",
        "value": "xxxxxxxx"
      }
    ],
 
    "output": {
      "code": "SUCCESS",
      "message": "OK",
      "data": {
        "recommendation": "REJECT",
          {
            "eventTime": "10/2018",
            "productType": "CASH_LOAN",
            "hitReason": "MobileAndName",
            "reasonCode": "OVERDUE_DAYS_BETWEEN_30_TO_60"
          }
        ]
      },
      "extra": null,
      "transactionId": "84e32dcfecc9f4dc",
      "pricingStrategy": "PAY"
    }
  }
}


```
## Response
```json

{
   "code":"SUCCESS",
   "message":"OK",
   "data":{
   }
}


```
