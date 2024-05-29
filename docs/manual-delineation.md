## Overview

The Manual Delineation API service allows users to create a manual delineation request for a predefined area of interest.

## Base URL

The URL for the Manual Delineation API is [https://api.digifarm.io/manual-delineation](https://api.digifarm.io/manual-delineation)

## Authentication

The Manual Delineation API uses client tokens for authentication. Include your client token in each request to the API.

## **Endpoints**

### POST /manual-delineation

This endpoint create manual delineation request for a specified bounding box.

#### Request URL `https://api.digifarm.io/manual-delineation`


#### **Request Headers**

```
Content-Type: application/json
X-Client-Token: The client token for authorization
```

#### **Request Body**

*   `bbox`: \[object, required\] The bounding box coordinates.

    *   `min_long`: \[float, required\] The minimum longitude.
    *   `min_lat`: \[float, required\] The minimum latitude.
    *   `max_long`: \[float, required\] The maximum longitude.
    *   `max_lat`: \[float, required\] The maximum latitude.

*   `reference_year`: \[datetime, required\] A reference year which will be used in the delineation process.

*   `intersecting_boundaries`: \[bool, optional\] Whether to include the boundaries which intersect with the requested BBOX or not. Default is true.

*   `reference_date`: \[datetime, optional\] A reference date which will be used in the delineation process. Normally chosen by the delineation team within the main agricultural season.

*   `callback_url`: \[string, required\] A URL that our delineation will trigger from UI when manual delineation is complete.

#### **Example Request**

```
curl --location 'https://api.digifarm.io/manual-delineation' \
--header 'X-Client-Token: ae4eb3dc-2479-42c1-b359-b7f0565c6b69' \
--header 'Content-Type: application/json' \
--data '{
    "bbox": {
        "min_long": "11.149314198272208",
        "min_lat": "60.735192337443834",
        "max_long": "60.735192337443834",
        "max_lat": "40.23342342"
    },
    "reference_year": "2021",
    "callback_url": "https://webhook.site/d6f2e77c-966a-4845-8b50-9b5d9d5cb7d4",
    "intersecting_boundaries": "true",
    "reference_date": "20/12/2021"
}'
```

#### **Response**

*   `id`: \[string\] Subscription id.

*   `status`: \[string\] The status of the request.

*   `timestamp`: \[datetime\] Current date and time.

*   `version`: \[string\] API version number.

```json

{
    "id": "831d92ff-bc9b-406d-90c3-9d14b8c67b2c",
    "status": "QUEUED",
    "timestamp":"2024-05-29T08:45:34.054Z",
    "version":"v1.0"
}
```

### GET /manual-delineation/{subscription_id}/status

This endpoint provides manual delineation job status.

#### **Job Status**

* `QUEUED`: Manual delineation process will start soon. 
* `PROCESSING`: Manual delineation process already started.
* `PROCESSED`: Manual delineation process complete.
* `FAILED`: Manual delineation process fail.

#### Request URL `https://api.digifarm.io/manual-delineation/{subscription_id}/status`

#### **Request Headers**

```
Content-Type: application/json
X-Client-Token: The client token for authorization
```

#### **Path Parameter**

*   `subscription_id`: \[staring, required\] The subscription id get from manual delineation create API response.

#### **Example Request**

```
curl --location 'https://api.digifarm.io/manual-delineation/831d92ff-bc9b-406d-90c3-9d14b8c67b2c/status' \
--header 'X-Client-Token: ae4eb3dc-2479-42c1-b359-b7f0565c6b69'
```

#### **Response**

*   `status`: \[string\] The status of the request.

*   `file_link`: \[string\] If the manual delineation process is complete and S3 file downloadable link available, provide the pre-sign s3 file URL otherwise this key will hide from the response.

*   `timestamp`: \[datetime\] Current date and time.

*   `version`: \[string\] API version number.

```json

{
    "status": "PROCESSED",
    "file_link": "S3 file downloadable link",
    "timestamp":"2024-05-29T08:45:34.054Z",
    "version":"v1.0"
}
```


# Webhook Service

## Overview

The Manual Delineation Webhook API provides a way for your application to interact with our service, notifying clients of the completion of tasks. It allows you to register URL endpoints (webhooks) to which we will send notifications regarding specific events happening within our system. When a job is completed, the API makes a POST request to the `callback_url` that was specified in the create manual delineation request.

## Webhook Event

### Task Completion

When a task is completed, a POST request will be sent to the specified `callback_url`. Please ensure your API service is ready to accept POST requests at the callback URL you provide.

## Request Headers

*   `Content-Type`: `application/json`

*   `X-Digifarm-Signature`: A HMAC SHA256 signature computed from the payload and a shared secret, providing a way to ensure the request is from On demand API.

## Request Body

*   `id`: \[string\] Subscription id.

*   `file_name`: \[string\] File name.

*   `file_link`: \[string\] S3 file downloadable link.

*   `timestamp`: \[datetime\] Current date and time.

*   `version`: \[string\] API version number.

successfully.

```json
{
    "id": "831d92ff-bc9b-406d-90c3-9d14b8c67b2c",
    "file_name": "File name",
    "file_link": "S3 file downloadable link",
    "timestamp":"2024-05-29T08:45:34.054Z",
    "version":"v1.0"
    
}
```

## Security

Webhook delivery uses HTTPS to encrypt data during transmission. You may want to set up your endpoint to verify that incoming webhooks are from us.

### Verification

We will include a signature in each webhook event's HTTP headers. The header key is `X-Digifarm-Signature`. Your application can use this signature to verify the request was sent our On demand API. The value of this header is computed by generating a HMAC SHA256 hash of the payload using your client token as the key.

Here's an example in Python and node.js showing how you could verify a webhook request:

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

```javascript
import { createHmac } from 'crypto';

function isValidSignature(x_digifarm_signature, payload, clientToken){
    const expectedSignature = createHmac('sha256', clientToken)
			.update(JSON.stringify(payload))
			.digest('hex')
    return x_digifarm_signature === expectedSignature
}
```

## FAQ
**Q: How can I verify that the request came from your service?** We include a signature in each HTTP header. You can use this signature to verify that the request came from our service.
