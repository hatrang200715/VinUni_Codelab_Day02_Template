# 01 — Problem Scan & Quick Cards (Vin Smart Future)

> **Nhóm chọn Deep-Dive:** Card #1 — **Xanh SM: AI gợi ý trạm sạc / điều xe sạc di động khi tài xế báo pin yếu.**

---

## 🔍 Phase 1 — SCAN

Dùng 4 lenses (Repetitive · Time-consuming · AI-upgrade · Stakeholder pain) quét 4 công ty thành viên.

### Danh sách bài toán
| # | Subsidiary (VinFast/Xanh SM...) | Lens | Mô tả ngắn bài toán |
|---|----------------------------------|------|---------------------|
| 1 | Xanh SM | Stakeholder pain + Time-consuming | Tài xế báo pin yếu giữa ca; điều phối viên phải tra thủ công trạm sạc VinFast còn trụ trống, đúng chuẩn sạc, đủ pin để tới → ~15 phút/lượt, xe nằm chờ, có ca cạn pin giữa đường. |
| 2 | Xanh SM | Repetitive | Gợi ý điểm đón và lộ trình tối ưu cho tài xế trong giờ cao điểm (khách đổi vị trí, tắc đường). |
| 3 | VinFast | Time-consuming | Nhân viên CSKH đọc, phân loại và chuyển tiếp thủ công khiếu nại bảo hành pin/xe điện (~12 phút/ticket). |
| 4 | Vinhomes | AI-upgrade | Chatbot cư dân trả lời rập khuôn, không xử lý tốt câu hỏi phức tạp về hóa đơn, phí dịch vụ, quy định. |
| 5 | Vinmec | Repetitive | Điều dưỡng tốn thời gian tổng hợp hồ sơ bệnh án và triage ban đầu tại khoa cấp cứu. |


---

## 🃏 Phase 2 — QUICK-ASSESS: 3 Quick Problem Cards

### Quick Problem Card #1 ⭐ (nhóm chọn để Deep-Dive)
Bài toán: AI gợi ý trạm sạc phù hợp (hoặc điều xe sạc di động) khi tài xế Xanh SM báo pin yếu, và soạn nháp tin nhắn chỉ đường cho điều phối viên duyệt.

- Công ty thành viên: [x] Xanh SM
- Ai đang đau: Tài xế Xanh SM (xe nằm chờ, mất cuốc), điều phối viên Trung tâm Điều vận (quá tải giờ cao điểm)
- Workflow thủ công hiện tại:
  1. Tài xế gọi/nhắn tổng đài báo pin yếu
  2. Điều phối viên tra GPS + % pin của xe trên dashboard đội xe
  3. Tra dashboard trạm sạc VinFast: trạm nào còn trụ trống, đúng chuẩn sạc, gần nhất
  4. Ước lượng pin có đủ đi tới trạm không → quyết định chỉ đường hay gọi xe sạc di động
  5. Soạn tin nhắn chỉ đường gửi qua app tài xế
- Bước nào tốn thời gian/lỗi nhất: Bước 3–5 (⏱ ~10 phút/lượt); bước 4 ước lượng cảm tính → có ca xe cạn pin giữa đường
- AI có thể nhảy vào hỗ trợ ở bước nào: Bước 3–5 — rule lọc trạm + kiểm tra ngưỡng pin, LLM chọn phương án và soạn nháp tin nhắn
- Đo thành công bằng gì: Giảm thời gian xử lý từ ~15 phút → dưới 3 phút/lượt; 0 ca đề xuất trạm > 5km khi pin < 5%
- Quick Architecture: [x] Rule + [x] LLM (không cần Agent)

---

### Quick Problem Card #2
Bài toán: AI hỗ trợ phân loại và tóm tắt yêu cầu bảo hành pin/xe điện của khách hàng VinFast.

- Công ty thành viên: VinFast
- Ai đang đau: Nhân viên CSKH, kỹ thuật viên
- Workflow thủ công hiện tại:
  1. Khách gửi khiếu nại
  2. Nhân viên đọc nội dung
  3. Phân loại lỗi
  4. Chuyển cho kỹ thuật viên hoặc bảo hành
  5. Yêu cầu thêm thông tin nếu thiếu
- Bước nào tốn thời gian/lỗi nhất: Phân loại và trích xuất thông tin từ khiếu nại
- AI có thể nhảy vào hỗ trợ ở bước nào: Tóm tắt yêu cầu, lọc lỗi, gợi ý loại hỗ trợ
- Đo thành công bằng gì: Giảm thời gian xử lý từ 12 phút xuống còn 4 phút/lượt
- Quick Architecture: LLM Feature

---

### Quick Problem Card #3
Bài toán: AI hỗ trợ tóm tắt và trả lời câu hỏi của cư dân Vinhomes về hóa đơn, dịch vụ và quy định.

- Công ty thành viên: Vinhomes
- Ai đang đau: Nhân viên hỗ trợ cư dân
- Workflow thủ công hiện tại:
  1. Cư dân gửi câu hỏi
  2. Nhân viên đọc và tìm thông tin
  3. Soạn phản hồi
  4. Kiểm tra lại chính tả và nội dung
  5. Gửi phản hồi
- Bước nào tốn thời gian/lỗi nhất: Soạn phản hồi dựa trên nhiều nguồn thông tin
- AI có thể nhảy vào hỗ trợ ở bước nào: Tạo draft trả lời, tóm tắt chính sách, gợi ý câu trả lời
- Đo thành công bằng gì: Giảm thời gian soạn phản hồi từ 10 phút xuống còn dưới 2 phút
- Quick Architecture: LLM Feature

---

## 🗳️ Vì sao chọn Card #1
* **Card #1 (Xanh SM trạm sạc):** pain xảy ra hằng ngày, đo được bằng phút; phần nguy hiểm (ngưỡng pin) giải bằng rule, phần ngôn ngữ giải bằng LLM; có HITL tự nhiên (dispatcher duyệt).
* **Card #2 (VinFast bảo hành):** khả thi nhưng ít khẩn cấp hơn (xử lý back-office, SLA tính bằng giờ), cần thêm dữ liệu phân loại lỗi đã gán nhãn.
* **Card #3 (Vinhomes):** rủi ro trả lời sai về phí dịch vụ/quy định có thể gây khiếu nại pháp lý; cần chuẩn hoá kho tri thức chính sách trước.
