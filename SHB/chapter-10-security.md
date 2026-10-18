# Chương 10 - Security

> Câu 93 – 102 | JWT, OAuth2, SQL Injection, XSS, CSRF, Replay Attack, mã hóa dữ liệu

---

## Câu 93: Bảo vệ API chuyển tiền như thế nào?

### ✅ Câu trả lời — Defense in Depth (nhiều lớp bảo vệ)

| Lớp | Mô tả |
|-----|--------|
| 1. **HTTPS/TLS** | Mã hóa toàn bộ dữ liệu trên đường truyền |
| 2. **Authentication** | JWT, OAuth2, Session |
| 3. **Authorization** | Kiểm tra quyền: chuyển tiền? xem tài khoản? |
| 4. **Rate Limit** | 5 request/giây — tránh Brute Force |
| 5. **Idempotency** | Bấm nhiều lần không chuyển tiền nhiều lần |
| 6. **Audit Log** | User, IP, Device, Time, TransactionId |
| 7. **Monitoring** | Login Fail, TPS, Fraud |

### 🔄 Chỉ HTTPS có đủ không?

**Không.** HTTPS chỉ bảo vệ khi truyền trên mạng. Không bảo vệ:
- SQL Injection, XSS, CSRF
- Broken Access Control
- Logic Bug, Replay Attack

---

## Câu 94: JWT là gì?

### ✅ Câu trả lời

JWT (JSON Web Token) gồm ba phần:

```
Header.Payload.Signature
```

Payload chứa: UserId, Role, Expire Time

Signature dùng để **chống sửa token**.

### 🔄 Có nên lưu Password trong JWT không?

**Không.** Payload có thể đọc bằng Base64. Chỉ lưu: UserId, Role, Permission, Expiration.

---

## Câu 95: JWT có nhược điểm gì?

### ✅ Câu trả lời

JWT là **Stateless** → Server rất khó thu hồi token ngay.

### 🔄 Vậy làm sao Logout?

- Access Token ngắn hạn (10–15 phút) + Refresh Token
- Lưu Refresh Token trong DB/Redis để có thể thu hồi
- Blacklist token trong trường hợp đặc biệt

---

## Câu 96: OAuth2 là gì?

### ✅ Câu trả lời

OAuth2 là chuẩn **ủy quyền** (Authorization), không phải xác thực.

```
Đăng nhập Google → Website A nhận Access Token (không biết Password Google)
```

### 🔄 OAuth khác JWT thế nào?

- **OAuth2** = giao thức ủy quyền
- **JWT** = định dạng token

OAuth2 có thể dùng JWT làm Access Token.

---

## Câu 97: SQL Injection là gì?

### ✅ Câu trả lời

```java
String sql = "SELECT * FROM User WHERE username='" + username + "'";
// Nếu user nhập: ' OR 1=1 --
// → SELECT * FROM User WHERE username='' OR 1=1
// → Trả về toàn bộ dữ liệu!
```

### 🔄 Làm sao chống?

- **Prepared Statement** / Parameterized Query
- ORM (JPA, MyBatis...) đúng cách
- Validate Input
- Database Least Privilege

---

## Câu 98: XSS là gì?

### ✅ Câu trả lời

User nhập `<script>alert(1)</script>` → Website render → Javascript chạy.

**Cách chống:**
- Escape Output
- Encode HTML
- Content Security Policy (CSP)
- Không render HTML từ dữ liệu người dùng

---

## Câu 99: CSRF là gì?

### ✅ Câu trả lời

Trình duyệt tự gửi Cookie xác thực mà người dùng không chủ động thực hiện.

```
User đã login Internet Banking → Truy cập website độc hại
→ Website gửi POST /transfer → Browser tự gửi Cookie
→ Server tin là user thật
```

### 🔄 JWT có bị CSRF không?

- JWT trong **Authorization Header** → thông thường không bị
- JWT trong **Cookie** → vẫn cần CSRF Token hoặc SameSite

---

## Câu 100: Replay Attack là gì?

> 🏆 **Câu ngân hàng cực kỳ thích**

### ✅ Câu trả lời

Tin tặc nghe lén request chuyển 100 triệu → gửi lại nhiều lần → tiền chuyển nhiều lần.

**Cách chống:**
- Idempotency Key
- Timestamp
- Nonce
- Digital Signature
- Token/Request hết hạn ngắn

Server từ chối: request quá cũ, nonce đã sử dụng, chữ ký không hợp lệ.

---

## Câu 101: Dữ liệu nào trong Database cần mã hóa?

### ✅ Câu trả lời

**Mã hóa/bảo vệ đặc biệt:**
- CCCD
- Số tài khoản (tùy yêu cầu)
- Số thẻ
- CVV (theo PCI DSS, thường không được lưu)
- Thông tin cá nhân nhạy cảm

**Password:** Hash bằng Argon2, bcrypt hoặc PBKDF2 (kèm salt)

### 🔄 Tại sao Password không Encrypt?

Server không cần biết Password gốc. Chỉ cần:
```
Hash(password nhập vào) == Hash trong Database?
```

Nếu encrypt → ai có khóa giải mã đều xem được mật khẩu gốc → rủi ro cao.

---

## Câu 102: Nếu Database bị Hack thì sao?

### ✅ Câu trả lời

Thiết kế theo nguyên tắc **"giả định có thể bị xâm nhập"**:

- Password được hash
- Dữ liệu nhạy cảm được mã hóa
- Database Least Privilege
- Audit Log
- Giám sát truy cập bất thường
- Backup được mã hóa
- KMS/HSM cho quản lý khóa

→ Ngay cả khi dữ liệu bị lộ, thiệt hại cũng được giảm thiểu.

---

## ⭐ Câu SHB rất thích: "Nếu thiết kế Internet Banking, anh sợ điều gì nhất?"

### ✅ Câu trả lời

Không phải hệ thống chậm. Mà là **sai giao dịch hoặc giao dịch gian lận**:
- Chuyển tiền hai lần
- Sai số dư
- Người A xem được tài khoản người B
- Replay Attack
- Broken Authorization

**Ưu tiên:**
- Thiết kế đúng nghiệp vụ
- Kiểm soát phân quyền
- Ghi nhận Audit
- Theo dõi bất thường
- Kiểm thử bảo mật trước phát hành

---

## 🎯 Góc nhìn của Interviewer

> "Nếu giao người này phụ trách Internet Banking, liệu họ có thiết kế hệ thống đủ an toàn để bảo vệ tiền của khách hàng không?"
