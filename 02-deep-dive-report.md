# 🏗️ Phase 3 — DEEP-DIVE

## 3.1. Current-State Workflow Mapping (25 min)
**Vẽ quy trình hiện tại lên bảng/giấy A3.** Sử dụng các ký hiệu:
* 🔴 **Bottleneck:** Bước gây tắc nghẽn, tốn thời gian, hoặc sai sót nhiều nhất.
* 🔄 **Handoff:** Điểm chuyển giao thông tin giữa người và hệ thống, hoặc giữa các bộ phận.
* Ghi rõ thời gian vận hành trung bình: **Tổng cộng = 20-25 phút/lượt**.

``` text

┌──────────────┐     ┌──────────────┐     ┌──────────────┐     ┌──────────────┐
│ Bước 1       │     │ Bước 2       │     │ Bước 3       │     │ Bước 4       │
│ Tài xế xác   │     │ Tài xế bắt   │     │ Trạm sạc     │     │ Tài xế phải  │
│ nhận đi sạc  │ ──→ │ đầu di chuyển│ ──→ │ hết chỗ      │ ──→ │ di chuyển đến│
│              │     │ đến trạm sạc │     │              │     │ trạm sạc khác│
└──────────────┘     └──────────────┘     └──────────────┘     └──────────────┘

```

## 3.2. Problem Statement (6-field) & Metrics (15 min)
Điền đầy đủ 6 trường thông tin của bài toán:

| Field | Nội dung chi tiết |
|---|---|
| **1. Actor / Operator** | Tài xế Xanh SM |
| **2. Current Workflow** | 1. Giờ cao điểm, tài xế nhận thấy xe cần phải sạc ──> 2. Tài xế bắt đầu di chuyển đến trạm sạc |
|   ──> 3. Trạm sạc hết trạm ──> 4. Tài xế phải di chuyển đến trạm sạc khác  |
| **3. Bottleneck** | Bước 2, 3 và 4 |
| **4. Business Impact** | Làm tài xế xe bị phí thời gian di chuyển và không phục vụ khách được |
| **5. Success Metric** | Giảm thời gian di chuyển thành công đến trạm sạc dùng được đi 75% |
| **6. Operational Boundary** | AI được phép tổng hợp và phân tích các tuyến đường đến trạm sạc, biết thông tin trạng thái của các trạm sạc. |

## 3.3. Future-State Flow & AI Fit (25 min)
* **Xác định mức AI Fit (AI-Fit Matrix):** Giải pháp thuộc nhóm nào?  [X] Agentic Loop.
* **Vẽ Future-State Flow:** Đánh dấu rõ:
  * 🔵 **AI Step:** Tác vụ LLM xử lý.
  * 🟢 **Human Step (HITL):** Bước con người phê duyệt/review (Human-in-the-loop).
  * ↩️ **Fallback:** Kế hoạch dự phòng khi LLM trả về kết quả lỗi hoặc không tự tin.

``` text

┌──────────────┐     ┌──────────────┐     ┌──────────────┐     ┌──────────────┐
│ Bước 1       │     │ Bước 2       │     │ Bước 3       │     │ Bước 4       │
│ Tài xế xác   │     │ 🔵 Hệ thống  │    │ 🔵 AI thông  │     │ Tài xế bắt   │
│ nhận đi sạc  │ ──→ │ kiểm tra tình│ ──→ │ báo chỉ đường│ ──→ │ đầu di chuyển│
│              │     │ trạng các    |     | đến trạm sạc |     | đến trạm sạc |
|              |     │ trạm sạc     │     | phù hợp nhất │     │              │
└──────────────┘     └──────────────┘     └──────────────┘     └──────────────┘
┌──────────────┐     ┌──────────────┐     ┌──────────────┐     ┌──────────────┐
│ Bước 1       │     │ Bước 2       │     │ Bước 3       │     │ Bước 4       │
│ Nhận cuộc    │     │ 🔵 Auto-pull │     │ 🔵 AI draft  │     │ 🟢 Dispatch  │
│ gọi sự cố    │ ──→ │ vị trí &     │ ──→ │ SMS chỉ dẫn  │ ──→ │ click duyệt  │
│              │     │ trạm sạc trống│    │ & chỉ đường  │     │ & gửi tài xế │
└──────────────┘     └──────────────┘     └──────────────┘     └──────────────┘
                                                │
                                                ▼
                                          ↩️ Fallback:
                                          Nếu AI không tìm được
                                          trạm phù hợp, tài xế
                                          sẽ phải tự xoay sở

```

---