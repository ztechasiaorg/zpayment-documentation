# P2C.net-documentation

## API Documentation: Create Invoice

### Endpoint
**POST** `/api/invoices`

### Headers
- **api-key**: Merchant's API key string (Required).

### Request Body
The request body should be sent in JSON format.

| Field            | Data Type    | Required | Description                                                              |
|-------------------|--------------|----------|--------------------------------------------------------------------------|
| **amount**        | Number       | Yes      | The invoice amount (unit: USDT).                                         |
| **redirectUrl**   | String       | No       | URL to redirect to after the invoice is completed.                       |
| **referenceId**   | String       | No       | Seller's order ID, added by the seller if applicable.                    |
| **description**   | String       | No       | Description of the invoice.                                              |
| **metaData**      | JSON Object  | No       | A JSON object containing additional information (if any).                |

#### Example Request
```
curl -X 'POST' \
  'https://api.pay2c.net/api/invoices' \
  -H 'accept: */*' \
  -H 'api-key: aAJpohT0yD7kQOh2SIqbgZS588TaS5' \
  -H 'Content-Type: application/json' \
  -d '{
  "amount": 100.00,
  "redirectUrl": "https://seller-website.com/success",
  "referenceId": "ORDER123456",
  "description": "Payment for order #123456",
  "metaData": {
    "customerId": "CUST7890",
    "orderNotes": "Fast delivery"
  }
}
'
```
## API Documentation: View Invoice List

### Endpoint
**GET** `/api/invoices`

### Headers
- **api-key**: API key string for the merchant (Required).

### Query Parameters

| Field             | Data Type     | Required | Description                                                                                       |
|-------------------|---------------|----------|---------------------------------------------------------------------------------------------------|
| **from**          | string        | No       |Start date (in ISO 8601 format, e.g., 2022-06-10T03:55:54.208+00:00).                              |
| **to**            | string        | No       |End date (in ISO 8601 format).                                                                     |
| **page**          | string        | No       |Page number, default is 1.                                                                         |
| **take**          | string        | No       |Number of records per page, default is 20.                                                         |
| **search**        | string        | No       |Search invoices by code.                                                                           |
| **statuses**      | string        | No       |Status of the invoices (the list of available statuses will be provided by the API provider).      |

### Response
- HTTP Status Code: 200 (OK) if successful.
- HTTP Status Code: 400 (Bad Request) if the input is invalid.
- HTTP Status Code: 500 (Internal server error) if there is an error from the server.

#### Example Request
```
curl -X 'GET' \
  'https://api.pay2c.net/api/invoices?page=1&take=20' \
  -H 'accept: */*' \
  -H 'api-key: "your_api_key_here" '
```

#### Example Response
```
{
  "timestamp": "2025-01-17T10:05:21.209Z",
  "status": 200,
  "data": [
    {
      "id": 34,
      "createdAt": "2025-01-17T08:02:55.818Z",
      "updatedAt": "2025-01-17T08:32:49.199Z",
      "code": "I20250117V46",
      "amount": 1,
      "receivedAmount": 0,
      "serviceFee": 0,
      "processingFee": 0,
      "netAmount": 0,
      "walletAddress": "37u54aPXRzBLugzKyP3mDeSy4Vi4DKPgtYDwVb9QuUUw",
      "referenceId": "string",
      "description": "string",
      "metaData": {},
      "expiredAt": "2025-01-17T08:32:55.816Z",
      "paidAt": null,
      "failedAt": "2025-01-17T08:32:49.202Z",
      "status": "FAILED",
      "merchant": {
        "id": 6,
        "name": "Chespatra",
        "webhookUrl": ""
      }
    },
    {
      "id": 31,
      "createdAt": "2025-01-17T07:02:24.592Z",
      "updatedAt": "2025-01-17T07:06:35.276Z",
      "code": "I20250117GJU",
      "amount": 0.000045,
      "receivedAmount": 0.000045,
      "serviceFee": 0.000005,
      "processingFee": 0.000005,
      "netAmount": 0.000036,
      "walletAddress": "77y3LA28iPom6H3TwUZcPaK97LajL8rVCFnfXUURDKCN",
      "referenceId": "",
      "description": "",
      "metaData": null,
      "expiredAt": "2025-01-17T07:32:24.598Z",
      "paidAt": "2025-01-17T07:06:35.313Z",
      "failedAt": null,
      "status": "PAID",
      "merchant": {
        "id": 6,
        "name": "Chespatra",
        "webhookUrl": ""
      }
    },
    {
      "id": 27,
      "createdAt": "2025-01-17T05:00:44.881Z",
      "updatedAt": "2025-01-17T05:01:56.981Z",
      "code": "I20250117E7I",
      "amount": 0.5,
      "receivedAmount": 0.500009,
      "serviceFee": 0.050001,
      "processingFee": 0.050001,
      "netAmount": 0.400007,
      "walletAddress": "5MR1LFRdjw8Vm1z4ben1FtWbbnovuinE13An3PtvVR9G",
      "referenceId": "",
      "description": "",
      "metaData": null,
      "expiredAt": "2025-01-17T05:30:44.900Z",
      "paidAt": "2025-01-17T05:01:56.990Z",
      "failedAt": null,
      "status": "PAID",
      "merchant": {
        "id": 6,
        "name": "Chespatra",
        "webhookUrl": ""
      }
    }
  ],
  "message": "OK",
  "meta": {
    "page": 1,
    "take": 20,
    "totalCount": 3,
    "pageCount": 1,
    "hasPreviousPage": false,
    "hasNextPage": false
  }
}
```
## API Documentation: View Invoice Details

### Endpoint
**GET** `/api/invoices/{id}`

### Headers
- **api-key**: API key string for the merchant (Required).

### Path
- **id**: The invoice id returned by the system (Required).

### Response
- HTTP Status Code: 200 (OK) if successful.
- HTTP Status Code: 400 (Bad Request) if the input is invalid.
- HTTP Status Code: 500 (Internal server error) if there is an error from the server.


#### Example Request
```
curl -X 'GET' \
  'https://api.pay2c.net/api/invoices/34' \ 
  -H 'accept: */*' \
  -H 'api-key: lRhkrpKAqFAPqqivANkiiVW1bEw9Oy'
```

#### Example Response
```
{
  "timestamp": "2025-01-17T08:03:48.163Z",
  "status": 200,
  "data": {
    "id": 34,
    "createdAt": "2025-01-17T08:02:55.818Z",
    "updatedAt": "2025-01-17T08:02:55.850Z",
    "code": "I20250117V46",
    "amount": 1,
    "receivedAmount": 0,
    "serviceFee": 0,
    "processingFee": 0,
    "netAmount": 0,
    "walletAddress": "37u54aPXRzBLugzKyP3mDeSy4Vi4DKPgtYDwVb9QuUUw",
    "referenceId": "string",
    "description": "string",
    "metaData": {},
    "expiredAt": "2025-01-17T08:32:55.816Z",
    "paidAt": null,
    "failedAt": null,
    "status": "PENDING",
    "paymentUrl": "https://staging.app.pay2c.net/checkout/Vm0weE1GbFhTWGxWV0doWFltczFVMWxyVm5kVmJGcHlWV3RLVUZWVU1Eaz0=",
    "redirectUrl": "string",
    "withdrawStatus": "NULL",
    "renewTimes": 0,
    "merchant": {
      "id": 6,
      "name": "Chespatra",
      "webhookUrl": ""
    }
  },
  "message": "OK"
}
```

## API Documentation: Webhooks

A webhook URL can now be provided in Merchant Setting for certain endpoints which will notify you of any change in the underlying object status. This URL will allow you to receive notifications whenever there is a change in the status of the related object.

With the webhook functionality, you no longer need to schedule tasks to periodically poll endpoints or allocate resources to monitor object statuses. Simply include a ```webhook_url``` in the API payload, and the Pay2c system will send a POST request to your server whenever there is a status update. Please ensure that the webhook URL is configured with the HTTPS protocol; otherwise, the webhook will not be triggered.

Make sure your endpoint is configured to respond to Pay2c webhooks with standard HTTP status codes.
For example, return a 2xx status code to confirm that the POST request was successfully received by your server.

### I. Invoice status specific events
| Event                  | Description                                                                                   |
|------------------------|-----------------------------------------------------------------------------------------------|
| **INVOICE_COMPLETED**  | Triggers when an invoice is successfully completed, and the payment is received from the user |
| **INVOICE_FAILED**     | Triggers when an invoice is marked as failed because the payment received from the user is 0  |

#### 1. INVOICE_COMPLETED
- Method: **POST**
- Example data:
```
{
	event: "INVOICE_COMPLETED",
	sign: "hash signature",
	timestamp: "2023-03-16T04:14:04.515+00:00",
	data: {
		id: 1,
		referenceId: "ref12101",
		code: "I2025011067N",
		status: "PAID",
		amount: 10,
		serviceFee: 1,
		processingFee: 1,
		receivedAmount: 10,
		description: "Your description here",
		createdAt: "2023-03-16T03:44:04.515+00:00",
		paidAt: "2023-03-16T04:14:04.515+00:00",
		failedAt: null,
		metaData: {}
	},
}
```

#### 2. INVOICE_FAILED
- Method: **POST**
- Example data:
```
{
	event: "INVOICE_FAILED",
	sign: "hash signature",
	timestamp: "2023-03-16T04:14:04.515+00:00",
	data: {
		id: 1,
		referenceId: "ref12101",
		code: "I2025011067N",
		status: "FAILED",
		amount: 10,
		serviceFee: 1,
		processingFee: 1,
		receivedAmount: 10,
		description: "Your description here",
		createdAt: "2023-03-16T03:44:04.515+00:00",
		paidAt: null,
		failedAt: "2023-03-16T04:14:04.515+00:00",
		metaData: {}
	},
}
```

### II. Create a `sign`
Here is the guide to create and verify signature generated by the HMAC-SHA256 algorithm, which consists of two parts: signatureKey and payloadString. Below is how to create and verify the signature in Node.js.

#### 1. Create the signature `sign` in the request:
To create the signature for the API, you need two elements:

- signatureKey: This is obtained from the Merchant's information (provided by the Merchant).
- payloadString: This is created from `timestamp` and `id` of data in the callback, with the formula:
`payloadString = timestamp + "." + id`

#### 2. Use the HMAC-SHA256 algorithm to generate the signature
```
import * as crypto from 'crypto';

export class SignatureService {
  // Create a signature from the payloadString and secretKey
  static createSignature(secretKey: string, payloadString: string): string {
    return crypto
      .createHmac('sha256', secretKey)  // Use HMAC-SHA256
      .update(payloadString)            // Update with data
      .digest('hex');                   // Return the result as a hex string
  }

  // Verify the webhook signature with the server signature
  static verifySignature(
    secretKey: string,
    payloadString: string,
    webhookSignature: string,
  ): boolean {
    // Generate the server signature from payloadString and secretKey
    const serverSignature = crypto
      .createHmac('sha256', secretKey)
      .update(payloadString)
      .digest('hex');

    // Compare the webhook signature with the server signature
    return serverSignature === webhookSignature;
  }
}
```

#### 3. Example of verifying the callback event
##### a. Create the signature in the request
Suppose you have timestamp and data.id in the callback, you will create payloadString and then use SignatureService.createSignature to generate the signature:
```
const webhookBody = {
  event: "INVOICE_FAILED",
  sign: "hash signature",
  timestamp: "2023-03-16T04:14:04.515+00:00",
  data: {
    id: 1,
    referenceId: "ref12101",
    code: "I2025011067N",
    status: "FAILED",
    amount: 10,
    serviceFee: 1,
    processingFee: 1,
    receivedAmount: 10,
    description: "Your description here",
    createdAt: "2023-03-16T03:44:04.515+00:00",
    paidAt: null,
    failedAt: "2023-03-16T04:14:04.515+00:00",
    metaData: {}
  },
}
const timestamp = webhookBody.timestamp;
const data = webhookBody.data;

// Create the payloadString
const payloadString = `${timestamp}.${data.id}`;

// Generate the signature
const signature = SignatureService.createSignature(secretKey, payloadString);
``` 

##### b. Verify the webhook signature
When receiving the signature from the webhook, you will retrieve payloadString (as created above) and use SignatureService.verifySignature to verify the validity of the signature.
```
// Webhook sends the signature in the request
const webhookSignature = 'webhook_provided_signature';  // Signature from the webhook

// Verify the signature
const isValid = SignatureService.verifySignature(secretKey, payloadString, webhookSignature);

if (isValid) {
  // Signature is valid, continue processing
} else {
  // Signature is invalid, return an error
}
```
