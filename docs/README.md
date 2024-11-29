## Overview

The On demand Deep resolution API service allows users to run deep-resolution computations for a predefined area of interest, during a certain time period.

## Base URL

The URL for the On demand API is [https://api.digifarm.io/development/deep-resolution](https://api/digifarm.io/development/deep-resolution)

## Authentication

The On demand API uses client tokens for authentication. Include your client token in each request to the API.

This API provides deep-resolution data for a specific field over a specified date range. The endpoint accepts various query parameters for customization.

## **Endpoints**

### POST /deep-resolution/available_dates

This endpoint provides available dates for deep resolution imagery within a specified bounding box and time range.

#### Request URL `POST https://api.digifarm.io/deep-resolution/available_dates`

**Method**: POST

#### **Request Headers**

```
Content-Type: application/json
```

#### **Request Body**

*   `bbox`: \[object, optional\] The bounding box coordinates.

    *   `x_min`: \[float, required\] The minimum longitude.
    *   `y_min`: \[float, required\] The minimum latitude.
    *   `x_max`: \[float, required\] The maximum longitude.
    *   `y_max`: \[float, required\] The maximum latitude.

*   `aoi`: \[object, optional\] The GeoJSON format. We have only enabled Polygon and MultiPolygon single geometry GeoJson functionality for our API. Check the valid GeoJSON instructions [here](#instructions-for-valid-geojson-format).

**_NOTE:_** At least need to provide either bbox or aoi. If provide both aoi and bbox, aoi has more precedence. 

*   `cloud_cover`: \[integer, required\] The maximum allowed cloud cover percentage.

*   `start_date`: \[string, optional\] The start date in ISO 8601 format

*   `end_date`: \[string, optional\] The end date in ISO 8601 format 

*   `client_token`: \[string, optional\] The client token for authorization.

*   `callback_url`: \[string, required\] The URL to call with the results.

    <br/>

**Example Request**

```
curl -X POST https://api.digifarm.io/deep-resolution/available_dates \
 -H "Content-Type: application/json" \
 -d '{
 "bbox": {
    "x_min": 11.16545302951809,
    "y_min": 60.73687126373886,
    "x_max": 11.21197326267199,
    "y_max": 60.75331374707744
 },
 "aoi": { "type": "Feature", "properties": { "id": 1 }, "geometry": { "type": "MultiPolygon", "coordinates": [ [ [ [ 138.629423808807388, 35.396884261237581 ], [ 138.627417448995601, 35.396586287008105 ], [ 138.627417448995601, 35.396586287008105 ], [ 138.626682445896222, 35.39814568547569 ], [ 138.627169137137685, 35.399148865381584 ], [ 138.627477043841481, 35.399446839611059 ], [ 138.627993532505911, 35.399466704559693 ], [ 138.628281574261081, 35.399516366931273 ], [ 138.62859941343919, 35.399367379816532 ], [ 138.628807995399796, 35.398989945792529 ], [ 138.629125834577906, 35.398801228780535 ], [ 138.629821107780032, 35.398890621049375 ], [ 138.630417056238969, 35.398890621049375 ], [ 138.631161991812661, 35.398413862282212 ], [ 138.63163875057981, 35.398552916922633 ], [ 138.631608953156871, 35.398890621049375 ], [ 138.631241451607195, 35.399029675689796 ], [ 138.630844152634552, 35.399466704559693 ], [ 138.629890635100224, 35.399655421571694 ], [ 138.62936421396148, 35.400360627248112 ], [ 138.629523133550549, 35.400787723643695 ], [ 138.629632390768023, 35.40094664323275 ], [ 138.629602593345084, 35.401135360244751 ], [ 138.629185429423814, 35.4013538746797 ], [ 138.629016577360431, 35.401473064371487 ], [ 138.628976847463179, 35.401721376229382 ], [ 138.629562863447802, 35.402416649431487 ], [ 138.630019757266325, 35.402645096340756 ], [ 138.631013004697934, 35.402913273147277 ], [ 138.631350708824669, 35.4033006396456 ], [ 138.631946657283606, 35.402823880878437 ], [ 138.63195658975792, 35.402645096340756 ], [ 138.632075779449707, 35.402257729842439 ], [ 138.632701525331612, 35.401502861794434 ], [ 138.633625245442971, 35.401542591691694 ], [ 138.633486190802557, 35.401880295818437 ], [ 138.633595448020031, 35.402088877779065 ], [ 138.634529100605732, 35.403211247376753 ], [ 138.634469505759824, 35.40478057831865 ], [ 138.634688020194773, 35.404949430382025 ], [ 138.635552145460252, 35.405038822650866 ], [ 138.636356675879824, 35.405446054097816 ], [ 138.636585122789086, 35.405058687599499 ], [ 138.636465933097298, 35.404830240690231 ], [ 138.636942691864476, 35.404125035013813 ], [ 138.636833434647002, 35.40379726336139 ], [ 138.636654650109307, 35.403608546349389 ], [ 138.637886276924462, 35.402436514380121 ], [ 138.636604987737712, 35.398155617950003 ], [ 138.629423808807388, 35.396884261237581 ] ] ] ] } },
 "cloud_cover": 70,
 "start_date": "2023-10-01T00:00:00",
 "end_date": "2024-01-01T00:00:00",
 "client_token": "********************************",
 "callback_url": "https://webhook.site/e6249d0a-ed68-4b71-9d09-dc694acb34e4"
 }'
```

**Response**

*   `statusCode`: \[string\] The status of the request.

*   `message`: \[string\] A message describing the outcome of the request.

*   `version`: \[string\] API version number

*   `timestamp`: \[datetime\] The ID of the initiated task.

```json
{
    "statusCode":200,
    "data": {
        "message":"Fetching available dates, callback url will be called to https://webhook.site/e6249d0a-ed68-4b71-9d09-dc694acb34e4 when done"
    },
    "version":"v1.0",
    "timesetamp":"2023-08-02T03:17:23.340688"
}
```


### POST /deep-resolution

This endpoint provides deep resolution data for a specified bounding box over a given date range.

#### Request URL `POST https://api.digifarm.io/deep-resolution`

**Method**: POST

#### **Request Headers**

```
Content-Type: application/json
```

#### **Request Body**

*   `bbox`: \[object, optional\] The bounding box coordinates.

    *   `x_min`: \[float, required\] The minimum longitude.
    *   `y_min`: \[float, required\] The minimum latitude.
    *   `x_max`: \[float, required\] The maximum longitude.
    *   `y_max`: \[float, required\] The maximum latitude.

*   `aoi`: \[object, optional\] The GeoJSON format. We have only enabled Polygon and MultiPolygon single geometry GeoJson functionality for our API. Check the valid GeoJSON instructions [here](#instructions-for-valid-geojson-format).

**_NOTE:_** At least need to provide either bbox or aoi. If provide both aoi and bbox, aoi has more precedence.

*   `cloud_cover`: \[integer, required\] The maximum allowed cloud cover percentage.

*   `start_date`: \[string, optional\] The start date in ISO 8601 format

*   `end_date`: \[string, optional\] The end date in ISO 8601 format 

*   `client_token`: \[string, optional\] The client token for authorization.

*   `callback_url`: \[string, required\] The URL to call with the results.

*   `dates`: \[list of string, optional\] The dates to use for processing the area of interest.

* At least dates is required, or, alternatively, start_date and end_date.
** If start_date and end_date are provided, all available dates in between will be processed.


    <br/>

**Example Request**

```
curl -X POST "https://api.digifarm.io/deep-resolution" \
 -H "Content-Type: application/json" \
 -d '{
 "bbox": {
    "x_min": 11.16545302951809,
    "y_min": 60.73687126373886,
    "x_max": 11.21197326267199,
    "y_max": 60.75331374707744
 },
 "aoi": { "type": "Feature", "properties": { "id": 1 }, "geometry": { "type": "MultiPolygon", "coordinates": [ [ [ [ 138.629423808807388, 35.396884261237581 ], [ 138.627417448995601, 35.396586287008105 ], [ 138.627417448995601, 35.396586287008105 ], [ 138.626682445896222, 35.39814568547569 ], [ 138.627169137137685, 35.399148865381584 ], [ 138.627477043841481, 35.399446839611059 ], [ 138.627993532505911, 35.399466704559693 ], [ 138.628281574261081, 35.399516366931273 ], [ 138.62859941343919, 35.399367379816532 ], [ 138.628807995399796, 35.398989945792529 ], [ 138.629125834577906, 35.398801228780535 ], [ 138.629821107780032, 35.398890621049375 ], [ 138.630417056238969, 35.398890621049375 ], [ 138.631161991812661, 35.398413862282212 ], [ 138.63163875057981, 35.398552916922633 ], [ 138.631608953156871, 35.398890621049375 ], [ 138.631241451607195, 35.399029675689796 ], [ 138.630844152634552, 35.399466704559693 ], [ 138.629890635100224, 35.399655421571694 ], [ 138.62936421396148, 35.400360627248112 ], [ 138.629523133550549, 35.400787723643695 ], [ 138.629632390768023, 35.40094664323275 ], [ 138.629602593345084, 35.401135360244751 ], [ 138.629185429423814, 35.4013538746797 ], [ 138.629016577360431, 35.401473064371487 ], [ 138.628976847463179, 35.401721376229382 ], [ 138.629562863447802, 35.402416649431487 ], [ 138.630019757266325, 35.402645096340756 ], [ 138.631013004697934, 35.402913273147277 ], [ 138.631350708824669, 35.4033006396456 ], [ 138.631946657283606, 35.402823880878437 ], [ 138.63195658975792, 35.402645096340756 ], [ 138.632075779449707, 35.402257729842439 ], [ 138.632701525331612, 35.401502861794434 ], [ 138.633625245442971, 35.401542591691694 ], [ 138.633486190802557, 35.401880295818437 ], [ 138.633595448020031, 35.402088877779065 ], [ 138.634529100605732, 35.403211247376753 ], [ 138.634469505759824, 35.40478057831865 ], [ 138.634688020194773, 35.404949430382025 ], [ 138.635552145460252, 35.405038822650866 ], [ 138.636356675879824, 35.405446054097816 ], [ 138.636585122789086, 35.405058687599499 ], [ 138.636465933097298, 35.404830240690231 ], [ 138.636942691864476, 35.404125035013813 ], [ 138.636833434647002, 35.40379726336139 ], [ 138.636654650109307, 35.403608546349389 ], [ 138.637886276924462, 35.402436514380121 ], [ 138.636604987737712, 35.398155617950003 ], [ 138.629423808807388, 35.396884261237581 ] ] ] ] } },
 "cloud_cover": 70,
 "start_date": "2023-10-01T00:00:00",
 "end_date": "2024-01-01T00:00:00",
 "client_token": "********************************",
 "callback_url": "https://webhook.site/e6249d0a-ed68-4b71-9d09-dc694acb34e4"
 }'
```

**Response**

*   `statusCode`: \[string\] The status of the request.

*   `message`: \[string\] A message describing the outcome of the request.

*   `version`: \[string\] API version number

*   `timestamp`: \[datetime\] The ID of the initiated task.

```json

{
    "statusCode":200,
    "data":{
        "message":"Job queued",
        "job_id":"037d4b03-0b7a-46b5-b618-f81bdbe08739"
    },
    "version":"v1.0",
    "timesetamp":"2023-08-02T03:17:23.340688"
}
```

### POST /deep-resolution/status/{job_id}

This endpoint provides available dates for deep resolution imagery within a specified bounding box and time range.

#### Request URL `POST https://api.digifarm.io/deep-resolution/status/{job_id}`

**Method**: GET

#### **Request Headers**

```
Content-Type: application/json

client_token: \[string, required\] The client token for authorization.

```

    <br/>

**Example Request**

```
curl -X GET https://api.digifarm.io/deep-resolution/status/{job_id} \
 -H "Content-Type: application/json" \
```

**Response**

*   `statusCode`: \[string\] The status of the request.

*   `message`: \[string\] A message describing the outcome of the request.

*   `version`: \[string\] API version number

*   `timestamp`: \[datetime\] The ID of the initiated task.

```json

{
    "statusCode":200,
    "data":{
        "job_id":"e6249d0a-ed68-4b71-9d09-dc694acb34e4",
        "dates": []
    }
    "version":"v1.0",
    "timesetamp":"2023-08-02T03:17:23.340688"
}
```



### **Error Response**

In case of errors, the API will return an HTTP status code and a JSON object containing an error message.

**Condition**: If fields are missing or the data is in the wrong format.

**Code**: 400 BAD REQUEST

**Content Example**:

Type json

```
{
  "statusCode":400,
  "data":{
      "message": "Bad request. Invalid input data. Please provide all fields in correct format."
  },
  "version":"v1.0",
  "timestamp": "Current timestamp"
}
```

**Condition**: If the 'client\_token' is missing or invalid.

**Code**: 401 UNAUTHORIZED

**Content Example**:

json

```
{
  "statusCode":401,
  "data":{
      "message": "Unauthorized. Invalid client token."
  },
  "version":"v1.0",
  "timestamp": "Current timestamp"
}
```

**Condition**: If there's a problem initiating the task.

**Code**: 500 INTERNAL SERVER ERROR

**Content Example**:

json

```
"statusCode":500,
"data":{
    "message": "Internal Server Error. Could not initiate task due to an internal server error. Please try again later."
},
"version":"v1.0",
"timestamp": "Current timestamp"
```

# Webhook Service

## Overview

The On demand Webhook API provides a way for your application to interact with our service, notifying clients of the completion of tasks. It allows you to register URL endpoints (webhooks) to which we will send notifications regarding specific events happening within our system. When a job is completed, the API makes a POST request to the `callback_url` that was specified in the original request.

## Webhook Event

### Task Completion

When a task is completed, a POST request will be sent to the specified `callback_url`. Please ensure your API service is ready to accept POST requests at the callback URL you provide.

## Request Headers

*   `Content-Type`: `application/json`

*   `X-Digifarm-Signature`: A HMAC SHA256 signature computed from the payload and a shared secret, providing a way to ensure the request is from On demand API.

## Request Body

*   `job_id`: \[string\] The ID of the completed task.

*   `statusCode`: \[string\] The http status code of the task.

*   `data`: \[object\] The result of the task. This will be present if the task completed successfully.

```
{
    
    "statusCode":200,
    "data": {
        "job_id": "fdj4hi8e-3ds4-kl3d-fk4d-4dk3ji8e9sdf",
        "output":  "S3 download URL of the deep-resolution output in .zip format",
        "version":"v1.0",
        "timestamp": "[string] Time when the task was completed"
     },
    
}
```

## Security

Webhook delivery uses HTTPS to encrypt data during transmission. You may want to set up your endpoint to verify that incoming webhooks are from us.

### Verification

We will include a signature in each webhook event's HTTP headers. The header key is `X-Digifarm-Signature`. Your application can use this signature to verify the request was sent our On demand API. The value of this header is computed by generating a HMAC SHA256 hash of the payload using your client token as the key.

Here's an example in Python showing how you could verify a webhook request:

```python
import hmac
import hashlib

def is_valid_signature(x_digifarm_signature, data, client_token):
    payload_str = json.dumps(data.dict(), sort_keys=True)
    expected_signature = hmac.new(
        client_token.encode(),
        msg=payload_str.encode(),
        digestmod=hashlib.sha256
    ).hexdigest()
    return hmac.compare_digest(expected_signature, x_digifarm_signature)

```

### Error Handling

If there's an error during the processing of the task, the webhook request will still be sent, but the `statusCode` field will be `error` and the `message` field will contain information about the error.

**Condition**: If there's a problem processing the callback request.

**Code**: 400 BAD REQUEST

**Content Example**:

```
"statusCode":400,
"data":{
    "data": {
        "job_id": "fdj4hi8e-3ds4-kl3d-fk4d-4dk3ji8e9sdf",
        "version": "v1.0",
        "error": "Bad Request. There was an error processing the task.",
        "timestamp": "Current timestamp"
    },
},
```

<br />

### Instructions for valid GeoJSON Format

###### Instructions for Polygon Feature

1. Coordinates length should always be one. 
2. There should not be more than one feature.
3. There should always be only one geometry type.

```json
{
    "type": "Feature",
    "properties": {},
    "geometry": {
        "type": "Polygon",
        "coordinates": [
            [
                [],
                [],
                [],
                []
            ]
        ]
    }
}
```

###### Instructions for MultiPolygon FeatureCollection

1. Coordinates length should always be one and should be in the provided format.
2. There should not be more than one feature.
3. There should always be only one geometry type.

```json
{
    "type": "FeatureCollection",
    "features": [
        {
            "type": "Feature",
            "properties": {},
            "geometry": {
                "coordinates": [
                    [
                        [
                            [],
                            [],
                            [],
                            []
                        ]
                    ]
                ],
                "type": "MultiPolygon"
            }
        }
    ]
}
```

###### Instructions for MultiPolygon Feature

1. Coordinates length should always be one and should be in the provided format.
2. There should not be more than one feature.
3. There should always be only one geometry type.

```json
{
    "type": "Feature",
    "properties": {},
    "geometry": {
        "type": "MultiPolygon",
        "coordinates": [
            [
                [
                    [],
                    [],
                    [],
                    []
                ]
            ]
        ]
    }
}
```

###### Instructions for Polygon FeatureCollection
1. Coordinates length should always be one.
2. There should not be more than one feature.
3. There should always be only one geometry type.


```json
{
    "type": "FeatureCollection",
    "features": [
        {
            "type": "Feature",
            "properties": {},
            "geometry": {
                "coordinates": [
                    [
                        [],
                        [],
                        [],
                        []
                    ]
                ],
                "type": "Polygon"
            }
        }
    ]
}
```

<br />

## FAQ

**Q: What HTTP status codes should I return?** Our service considers any status code between 200 and 299 as a success. Other status codes will be considered as a failure, and we will retry the delivery.

**Q: How can I verify that the request came from your service?** We include a signature in each HTTP header. You can use this signature to verify that the request came from our service.

<br/>
