# P2C.net-documentation

## English below
### Tài liệu API: Tạo Hóa Đơn

#### Endpoint
**POST** `/api/invoices`

#### Headers
- **api-key**: Chuỗi API key của merchant (Bắt buộc).

#### Request Body
Request body cần được gửi dưới định dạng JSON.

| Trường           | Kiểu dữ liệu  | Bắt buộc | Mô tả                                                                 |
|-------------------|---------------|----------|-----------------------------------------------------------------------|
| **amount**        | Số            | Có       | Số tiền của hóa đơn (đơn vị: USDT).                                  |
| **redirectUrl**   | Chuỗi         | Không    | URL để điều hướng sau khi hóa đơn hoàn thành.                        |
| **callback**      | Chuỗi         | Không    | Tự động gọi nếu seller đã cài đặt `webhookUrl` ở merchant.           |
| **referenceId**   | Chuỗi         | Không    | ID đơn hàng của seller, seller tự thêm nếu có.                       |
| **description**   | Chuỗi         | Không    | Mô tả hóa đơn.                                                       |
| **metaData**      | JSON Object   | Không    | Đối tượng JSON chứa thông tin bổ sung (nếu có).                      |

##### Ví dụ Request
```json
{
  "amount": 100.00,
  "redirectUrl": "https://seller-website.com/success",
  "callback": "https://seller-website.com/webhook",
  "referenceId": "ORDER123456",
  "description": "Thanh toán đơn hàng #123456",
  "metaData": {
    "customerId": "CUST7890",
    "orderNotes": "Giao hàng nhanh"
  }
}
```

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
| **callback**      | String       | No       | Automatically called if the seller has set `webhookUrl` in the merchant. |
| **referenceId**   | String       | No       | Seller's order ID, added by the seller if applicable.                    |
| **description**   | String       | No       | Description of the invoice.                                              |
| **metaData**      | JSON Object  | No       | A JSON object containing additional information (if any).                |

#### Example Request
```json
{
  "amount": 100.00,
  "redirectUrl": "https://seller-website.com/success",
  "callback": "https://seller-website.com/webhook",
  "referenceId": "ORDER123456",
  "description": "Payment for order #123456",
  "metaData": {
    "customerId": "CUST7890",
    "orderNotes": "Fast delivery"
  }
}
```
## Tài liệu API: Xem danh sách hóa đơn

### Endpoint
- URL: **/api/invoices**
- Phương thức: **GET**

### Headers
- **api-key**: Chuỗi API key của merchant (Bắt buộc).

### Query Parameters

| Trường            | Kiểu dữ liệu  | Bắt buộc | Mô tả                                                                                             |
|-------------------|---------------|----------|---------------------------------------------------------------------------------------------------|
| **from**          | string        | Không    |Ngày bắt đầu (theo định dạng ISO 8601, ví dụ: 2022-06-10T03:55:54.208+00:00)                       |
| **to**            | string        | Không    |Ngày kết thúc (theo định dạng ISO 8601).                                                           |
| **page**          | string        | Không    |Số trang mặc định là 1                                                                             |
| **take**          | string        | Không    |Số lượng trên mỗi trang mặc là 20                                                                  |
| **search**        | string        | Không    |Tìm kiếm hóa đơn theo mã code                                                                      |
| **statuses**      | string        | Không    |Trạng thái của hóa đơn (danh sách các trạng thái có thể dùng sẽ được cung cấp bởi nhà cung cấp API)|


### Response
- HTTP Status Code: 200 (Created) nếu thành công.
- HTTP Status Code: 400 (Bad Request) nếu thông tin không hợp lệ.

#### Ví dụ Request
```
curl -X 'GET' \
  'https://staging.api.pay2c.net/api/invoices?page=1&take=20' \
  -H 'accept: */*' \
  -H 'api-key: "your_api_key_here" '
```

#### Ví dụ Response
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
## Tài liệu API: Xem chi tiết hóa đơn

### Endpoint
- URL: **/api/invoices/{id}**
- Phương thức: **GET**

### Headers
- **api-key**: Chuỗi API key của merchant (Bắt buộc).

### Path
- **id**: id của hóa đơn do hệ thống trả về(Bắt buộc)


### Response
- HTTP Status Code: 200 (Created) nếu thành công.
- HTTP Status Code: 400 (Bad Request) nếu thông tin không hợp lệ.

#### Ví dụ Request
```
curl -X 'GET' \
  'https://staging.api.pay2c.net/api/invoices/34' \
  -H 'accept: */*' \
  -H 'api-key: lRhkrpKAqFAPqqivANkiiVW1bEw9Oy'
```

#### Ví dụ Response
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
