

# Workflow Task Submission

## Description
This API allows customers to submit parameters to the published workflow in the OSP. 

As a general unified API, it accepts all inputs as a Java map structure, and transforms them into a concreted structure to the selected workflow for execution. This API works as asynchronously, and responses client back a transaction id immediately, while the task is still in progress.

To have an idea of parameters, customers can go check the published workflow summary, where list a full set of parameters for this workflow. 


## Request

- URL: https://{domain}/intl/openapi/journey/v1/submit
- Method: post


### Domain
| Domain                | description                   |
|:----------------------|:------------------------------|
| api.advai.net     | Sandbox Environment           |
| api.advance.ai    | PROD Environment in Indonesia |
*NOTICE*:  In case the country code associated with your account is not consistent with the OSP region (e.g., Open a Philippine account in ID/SG OSP), there would be a slight variation in domain and request header. Please consult our engineers and follow the "workflow access method" page in OSP portal for detail. 


### Request Headers
| field name           | description                                     |
|:------------------|:------------------------------------------------|
| X-Advai-Key       | access-key for authentication.  |
| content-type      | application/json;charset=UTF-8                       |


### Request Parameters
| Parameter           | description                                     |
|:------------------|:------------------------------------------------|
| journeyId        | workflow id |




### Request Body
| name        | type      | require | description   |
|:------------|:---------|:---------|:----------------------|
| input       | Map      | true     | A general parameter input map       |
                                    

### Response


| name              | type   |    description                                            |
|:------------------|:-------|:-------------------------------------------------------|
| code              | String |  Response body code |
| message           | String |  "OK" or error tip                                      |
| data              | String |  Detail response body                                          |
| data.transId      | String |  transaction id  | 
| data.result      | Json[]  | The detail list of entities to be matched   |
| data.journeyId   | Integer | Workflow id from parameter |  
| data.status      | String  | Workflow stage. You should get "PROCESSING" | 

### Http code vs Http ResponseBody Code
<table>
	<thead>
		<tr><th>HTTP Code</th> <th>Response Body Code</th></tr>
	</thead>
	<tbody>
		<tr>
            		<td rowspan=2>400</td>
            		<td>INPUT_ERROR: illegal parameters after validation</td>
        	</tr>
		<tr><td>BIZ_FAILURE: backend failure triggered by illegal parameters</td></tr>
		<tr><td>401</td><td>UNAUTHORIZED</td></tr>
		<tr><td>403</td><td>FORBIDDEN: You do not have permission for this operation. </td></tr>
		<tr><td rowspan=3>500</td><td>INTERNAL_BIZ_FAILURE: internal failure caused by data, please raise a ticket. </td></tr>
  		<tr><td>INTERNAL_ERROR: Internal failured due to code bug during execution, please raise a ticket </td></tr>
      		<tr><td>REMOTE_CALL_ERROR: internal failure caused by 3rd-party, or network, please raise a ticket</td></tr>

	</tbody>
</table>


# Example

## Request:


```shell

curl -X POST \
  'https://api.advai.net/intl/openapi/journey/v1/submit?journeyId=51592' \
  -H 'cache-control: no-cache' \
  -H 'content-type: application/json' \
  -H 'x-advai-key: XXXXXXXXXXXXXXXXX' \
  -d '{
	"mode":1,
    "matchPercentage":0.95,
    "nationality":"Indonesia",
    "gender":"Female",
    "dob":"",
    "name":"David",
    "idNumber":"",
    "referenceId":"amlkun001",
    "intervalTime":1
}'


```
## Response
```json

{
   "code":"SUCCESS",
   "message":"OK",
   "data":{
      "tenantId":null,
      "transId":"3640ff63fa15ff48",
      "journeyId":51592,
      "status":"PROCESSING",
      "serialId":"6oke11tcekg3k2j7",
      "subJourneyId":975,
      "ctrlEventId":null
   },
   "extra":null,
   "transactionId":"3640ff63fa15ff48",
   "pricingStrategy":"PAY"

```
