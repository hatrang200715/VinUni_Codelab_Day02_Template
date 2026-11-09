# 🔍 Phase 1 — SCAN

### 4 Lenses tìm bài toán AI cho Vingroup:
1. **Lặp lại (Repetitive):** Tác vụ lặp đi lặp lại nhiều lần hằng ngày. (Ví dụ: So khớp hóa đơn sạc điện tại VinFast, route lại chuyến taxi tại Xanh SM).
2. **Tốn thời gian (Time-consuming):** Tác vụ ngốn thời gian xử lý thủ công của nhân viên. (Ví dụ: Soạn thảo phản hồi đánh giá 1-star của cư dân Vinhomes).
3. **AI có thể tốt hơn (AI-upgrade):** Dịch vụ khách hàng hiện tại còn chậm hoặc phản hồi rập khuôn. (Ví dụ: Chatbot CSKH Vinpearl hỗ trợ đặt vé vui chơi).
4. **Pain từ người khác (Stakeholder Pain):** Bottleneck khiến khách hàng hoặc nhân viên thực địa phàn nàn. (Ví dụ: Tài xế Xanh SM phàn nàn về việc hệ thống gợi ý điểm đón khách không chính xác).

| # | Subsidiary (VinFast/Xanh SM...) | Lens | Mô tả ngắn bài toán |
|---|----------------------------------|------|---------------------|
| 1 | Xanh SM | Pain từ người khác | Bảo trì quan sát tình trạng pin |
| 2 | Xanh SM | Lặp lại | Xử lý quá tải trạm sạc xe điện |
| 3 | Xanh SM | AI có thể tốt hơn | Quản lý điều phối xe điện tùy theo khung giờ |
| 4 | Vinfast | Lặp lại | Tối ưu chuỗi cung ứng linh kiện xe điện |
| 5 | Xanh SM | Pain từ người khác | Khách hàng hủy chuyến đi nhiều lần, làm ảnh hưởng đến tài xế |

# 🃏 Phase 2 — QUICK-ASSESS
Chọn top 3 từ danh sách SCAN: **#2 Quá tải trạm sạc xe điện, #4 Tối ưu chuỗi cung ứng linh kiện xe điện, #5 Khách hàng hủy chuyến đi**

## Thẻ bài toán: Card #2 - Quá tải trạm sạc xe điện
```
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #2                                       │
│                                                             │
│ Bài toán (1 câu): Hiện tượng quá tải trạm sạc xe điện       │
│ Công ty thành viên: [X] Xanh SM                             │
│                                                             │
│                                                             │
│ Ai đang đau (Actor)? Tài xế Xanh SM mất thời gian di chuyển │
│                                                             │
│ Workflow thủ công hiện tại (3-5 bước):                      │
│   1. Giờ cao điểm, tài xế nhận thấy xe cần phải sạc ──> 2. Tài xế bắt đầu di chuyển đến trạm sạc |
|   ──> 3. Trạm sạc hết trạm ──> 4. Tài xế phải di chuyển đến trạm sạc khác │
│                                                             │
│ Bước nào tốn thời gian nhất? Bước 2, 3 và 4 (⏱ 15-20 phút/lượt) │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2 (AI phân tích và thông báo tình hình các trạm sạc để tài xế quyết định) |
│                                                             │
│ Đo thành công bằng gì (Metric có số)? Giảm thời gian di chuyển đến |
| trạm sạc của tài xế từ 10 phút còn 3-5 phút                 │
│                                                             │
│                                                             │
│ Quick Architecture:   [X] Agent (Phân tích hiện trạng trạm sạc xe điện) │
└─────────────────────────────────────────────────────────────┘
```

## Thẻ bài toán: Card #4 - Thời gian chờ cung ứng linh kiện xe điện
```
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #4                                       │
│                                                             │
│ Bài toán (1 câu): Thời gian chờ cung ứng linh kiện xe điện  │
│ Công ty thành viên: [X] Vinfast                             │
│                                                             │
│                                                             │
│ Ai đang đau (Actor)? Người dùng xe điện                     │
│                                                             │
│ Workflow thủ công hiện tại (3-5 bước):                      │
│   1. Người dùng xe điện đưa xe đến hãng để sửa chữa ──> 2. Hãng nhận và giữ xe của khách hàng để phục vụ |
|   ──> 3. Hãng xem xét và yêu cầu linh kiện ──> 4. Hãng chờ linh kiện đến để sửa │
|   ──> 5. Người dùng xe điện nhận lại xe sau thời gian dài để phục vụ |
│                                                             │
│ Bước nào tốn thời gian nhất? Bước 3 và bước 4 (⏱ 4 ngày/lượt) │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 3 và 4 (AI xử lý và phân tích số liệu linh kiện) |
│                                                             │
│ Đo thành công bằng gì (Metric có số)? Giảm thời gian chờ linh kiện từ 4 ngày còn dưới 1 ngày │
│                                                             │
│                                                             │
│ Quick Architecture:   [X] Agent (Phân tích số liệu ở trạm phục vụ của hãng) │
└─────────────────────────────────────────────────────────────┘
```

## Thẻ bài toán: Card #5 — Khách hàng hủy chuyến đi nhiều lần làm ảnh hưởng đến tài xế
```
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #5                                       │
│                                                             │
│ Bài toán (1 câu): Khách hàng hủy chuyến đi nhiều lần, làm ảnh hưởng đến tài xế  │
│ Công ty thành viên: [X] Xanh SM                             │
│                                                             │
│                                                             │
│ Ai đang đau (Actor)? Tài xế Xanh SM mất thời gian di chuyển │
│                                                             │
│ Workflow thủ công hiện tại (3-5 bước):                      │
│   1. Khách hàng đặt chuyến xe ──> 2. Tài xế nhận chuyến và bắt đầu di chuyển đến vị trí khách hàng |
|   ──> 3. Khách hàng hủy chuyến xe ──> 4. Tài xế mất thời gian và năng lượng pin xe để di chuyển │
|   ──> 5. Khách hàng tiếp tục đặt một chuyến xe nữa và hủy chuyến của tài xế khác |
│                                                             │
│ Bước nào tốn thời gian nhất? Bước 3 và bước 4 (⏱ 10 phút/lượt) │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 1 (AI phân tích hành vi người dùng và cảnh báo) |
| tài xế hoặc không cho phép người dùng sử dụng dịch vụ)      │
│                                                             │
│ Đo thành công bằng gì (Metric có số)? Giảm thời gian bị phí phạm của tài xế từ 10 phút còn 3-5 phút │
│                                                             │
│                                                             │
│ Quick Architecture:   [X] Agent (Phân tích hành vi người dùng) │
└─────────────────────────────────────────────────────────────┘
```