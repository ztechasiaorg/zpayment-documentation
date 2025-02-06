# P2C.net-documentation

## [Link API: api.pay2c.net/api](https://api.pay2c.net/api)
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

### Response
- HTTP Status Code: 200 (OK) if successful.
- HTTP Status Code: 400 (Bad Request) if the input is invalid.
- HTTP Status Code: 429 (Too Many Requests) if the rate limit of 5 requests per IP/user/path/second is exceeded. The block duration is 10 seconds
- HTTP Status Code: 500 (Internal server error) if there is an error from the server.

#### Example Request
```bash
curl -X POST "https://api.example.com/api/invoices" \
-H "Content-Type: application/json" \
-H "api-key: <API_KEY>" \
-d '{
  "amount": 100.00,
  "redirectUrl": "https://your-website.com/success",
  "callback": "https://your-website.com/webhook",
  "referenceId": "ORDER12345",
  "description": "Payment for order #12345",
  "metaData": {
    "customerId": "CUST67890",
    "orderNotes": "Fast delivery"
  }
}'
```
#### Guide to Replacing Placeholders
- <_API_KEY_>: Replace with the API key provided when you register as a merchant.
- <_URL_>: Replace with your webhook or redirect URL.

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
- HTTP Status Code: 429 (Too Many Requests) if the rate limit of 5 requests per IP/user/path/second is exceeded. The block duration is 10 seconds
- HTTP Status Code: 500 (Internal server error) if there is an error from the server.

#### Example Request
```
curl -X 'GET' \
  'https://api.example.com/api/invoices?page=1&take=20' \
  -H 'accept: */*' \
  -H 'api-key: <API_KEY> '
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
- HTTP Status Code: 429 (Too Many Requests) if the rate limit of 5 requests per IP/user/path/second is exceeded. The block duration is 10 seconds
- HTTP Status Code: 500 (Internal server error) if there is an error from the server.

#### Example Request
```
curl -X 'GET' \
  'https://api.example.com/api/invoices/34' \ 
  -H 'accept: */*' \
  -H 'api-key: <API_KEY>'
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
```typescript
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
```javascript
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
