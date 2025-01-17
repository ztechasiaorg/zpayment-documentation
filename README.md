# P2C.net-documentation


## Tài liệu API: Tạo Hóa Đơn

### Endpoint
**POST** `/api/invoices`

### Headers
- **api-key**: Chuỗi API key của merchant (Bắt buộc).

### Request Body
Request body cần được gửi dưới định dạng JSON.

| Trường           | Kiểu dữ liệu  | Bắt buộc | Mô tả                                                                 |
|-------------------|---------------|----------|-----------------------------------------------------------------------|
| **amount**        | Số            | Có       | Số tiền của hóa đơn (đơn vị: USDT).                                  |
| **redirectUrl**   | Chuỗi         | Không    | URL để điều hướng sau khi hóa đơn hoàn thành.                        |
| **callback**      | Chuỗi         | Không    | Tự động gọi nếu seller đã cài đặt `webhookUrl` ở merchant.           |
| **referenceId**   | Chuỗi         | Không    | ID đơn hàng của seller, seller tự thêm nếu có.                       |
| **description**   | Chuỗi         | Không    | Mô tả hóa đơn.                                                       |
| **metaData**      | JSON Object   | Không    | Đối tượng JSON chứa thông tin bổ sung (nếu có).                      |

#### Ví dụ Request
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
