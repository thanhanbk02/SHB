# Chương 11 - Leadership & Team Management

> Câu 103 – 110 | Quản lý team, code review, xung đột, production incident

---

## Câu 103: Tech Lead khác Senior Developer ở điểm nào?

### ✅ Câu trả lời

- **Senior Developer** — Chịu trách nhiệm về chất lượng phần code mình phát triển
- **Tech Lead** — Chịu trách nhiệm về **thành công của cả team và sản phẩm**

Tech Lead cần:
- Định hướng kiến trúc
- Phân chia công việc
- Review thiết kế và code
- Hỗ trợ các thành viên
- Quản lý rủi ro kỹ thuật
- Phối hợp với QA, BA, DevOps, Product

### 🔄 Tech Lead có nên code không?

Có, nhưng **không nên** code nhiều nhất. Ưu tiên:
- Module khó
- Review kiến trúc
- Mentoring
- Production issue

> Nếu Tech Lead ôm hết việc → Team sẽ không phát triển.

---

## Câu 104: Một Junior code rất chậm. Làm gì?

### ✅ Câu trả lời

**Đầu tiên hiểu nguyên nhân:** Chưa quen domain? Thiếu kiến thức? Chưa hiểu yêu cầu? Thiếu kinh nghiệm debug?

**Hành động:**
- Pair Programming
- Review code thường xuyên
- Chia task nhỏ hơn
- Đặt mục tiêu theo tuần

**Nếu vẫn không cải thiện:**
- Trao đổi thẳng thắn
- Improvement plan với mục tiêu và thời hạn rõ ràng
- Nếu đã hỗ trợ đầy đủ mà vẫn không đáp ứng → báo cáo quản lý

---

## Câu 105: Hai Senior cãi nhau về kỹ thuật. Xử lý thế nào?

> 🏆 **Câu SHB rất thích**

### ✅ Câu trả lời

**Không** quyết định theo chức vụ hoặc ai nói to hơn.

Yêu cầu cả hai trình bày:
- Giải pháp
- Ưu điểm / Nhược điểm
- Ảnh hưởng tới hệ thống

Đánh giá dựa trên:
- Yêu cầu nghiệp vụ
- Hiệu năng
- Khả năng bảo trì
- Chi phí triển khai
- Rủi ro

Nếu cần → làm **PoC** hoặc **benchmark** để có dữ liệu thực tế.

> Quyết định dựa trên **dữ liệu và mục tiêu**, không dựa trên cảm tính.

---

## Câu 106: Một Developer không chịu Review Code. Làm sao?

### ✅ Câu trả lời

1. Trao đổi riêng để hiểu nguyên nhân (quá bận? không tự tin? không thấy giá trị?)
2. Đưa Code Review vào **Definition of Done**
3. Quy định rõ thời gian review
4. Hướng dẫn cách review hiệu quả
5. Theo dõi qua Pull Request

> Mục tiêu: xây dựng **văn hóa chia sẻ kiến thức**, không phải kiểm soát.

---

## Câu 107: Production bị Down lúc 2 giờ sáng. Làm gì?

> 🏆 **Câu cực kỳ phổ biến**

### ✅ Câu trả lời

| Bước | Hành động |
|------|-----------|
| 1. **Restore Service** | Rollback, chuyển node dự phòng, restart |
| 2. **Thông báo** | Team, quản lý, các bên liên quan |
| 3. **Điều tra** | Log, Monitoring, Metrics, Tracing |
| 4. **Root Cause Analysis** | Postmortem sau khi hệ thống ổn định |
| 5. **Preventive Action** | Alert, Test, Monitoring, Quy trình Release |

### 🔄 Ai chịu trách nhiệm?

**Tech Lead.** Việc đầu tiên là khôi phục dịch vụ và phối hợp xử lý. Sau đó phân tích nguyên nhân để cải thiện quy trình, **không tập trung đổ lỗi cá nhân**.

---

## Câu 108: Deadline còn 2 ngày nhưng Team không kịp. Làm gì?

### ✅ Câu trả lời

1. Đánh giá lại: còn bao nhiêu việc? Task nào bắt buộc? Task nào có thể lùi?
2. Trao đổi với PO/Business để thống nhất phạm vi
3. Chia thành Release 1, Release 2 thay vì cố làm tất cả

### 🔄 Có bắt OT không?

OT có thể cần trong khẩn cấp, nhưng **không nên là giải pháp mặc định**.

Thường xuyên OT = dấu hiệu:
- Estimation chưa tốt
- Scope thay đổi nhiều
- Planning có vấn đề

→ Tech Lead cần **cải thiện quy trình** thay vì phụ thuộc OT.

---

## Câu 109: Review Code theo tiêu chí gì?

### ✅ Câu trả lời

| Tiêu chí | Kiểm tra |
|-----------|----------|
| **Correctness** | Code đúng nghiệp vụ? |
| **Readability** | Tên biến, tên method rõ ràng? |
| **Maintainability** | Dễ sửa? Tách module hợp lý? |
| **Performance** | N+1 Query? Object không cần thiết? |
| **Security** | SQL Injection? Lộ dữ liệu? Kiểm tra quyền? |
| **Testing** | Unit Test? Xử lý trường hợp lỗi? |

---

## Câu 110: Một Tech Lead giỏi cần gì?

### ✅ Câu trả lời

| Yếu tố | Mô tả |
|---------|--------|
| **Technical** | Giải quyết bài toán khó, định hướng kỹ thuật |
| **Communication** | Trao đổi hiệu quả với tất cả các bên |
| **Leadership** | Tạo điều kiện cho thành viên phát triển, trao quyền |
| **Decision Making** | Dám quyết định khi thông tin chưa hoàn hảo |

---

## ⭐ Câu mở rộng: Developer gây lỗi làm Internet Banking Down. Xử lý thế nào?

### ✅ Câu trả lời

Việc đầu tiên **không phải** tìm người để trách. Mà là: **Khôi phục hệ thống.**

Sau đó phân tích:
- Vì sao lỗi vào Production?
- Unit Test? Code Review? CI/CD? QA? Monitoring?

Sự cố nghiêm trọng thường là kết quả của **nhiều lỗ hổng trong quy trình**, không chỉ từ một cá nhân.

**Hành động:**
- Chia sẻ bài học với team
- Cập nhật checklist review
- Bổ sung test
- Cải thiện quy trình release

---

## 🎯 Góc nhìn của Interviewer

> "Nếu giao cho anh một team 10 người, liệu team đó có làm việc hiệu quả và ổn định không?"
>
> Điểm quan trọng:
> - Không đổ lỗi cá nhân
> - Dựa trên dữ liệu và quy trình
> - Cân bằng giữa con người và kỹ thuật
> - Luôn hướng tới cải tiến liên tục
