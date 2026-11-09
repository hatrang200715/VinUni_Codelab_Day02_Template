# 03 — AI Log & Reflection

> **Bài toán:** Xanh SM — AI gợi ý trạm sạc / điều xe sạc di động khi tài xế báo pin yếu.
> **Công cụ AI đã dùng:** Claude Code (trợ lý lập trình trong VS Code); Gemini 2.5 Flash (mô hình được test trong prototype).

---

## 1. AI đã giúp gì

| Giai đoạn | AI giúp gì | Tôi giữ lại / thay đổi gì |
|---|---|---|
| SCAN & Quick Cards | Sắp xếp lại 5 bài toán theo 4 lenses, viết lại Card #1 cụ thể hơn (từ "gợi ý điểm đón" sang "gợi ý trạm sạc khi pin yếu"). | Tôi đổi chủ đề giữa chừng (lúc đầu định chọn Vinhomes, sau đó nhóm thống nhất chọn Xanh SM trạm sạc) → yêu cầu AI làm lại theo chủ đề nhóm. |
| Workflow mapping | Tách quy trình thành 6 bước có thời gian, handoff, bottleneck. | Cần đối chiếu lại thời gian từng bước với điều phối viên thật — hiện là số ước tính. |
| AI Fit | So sánh Rule vs LLM vs Agent; đề xuất để **rule** giữ ngưỡng pin < 5% thay vì tin hoàn toàn vào LLM. | Giữ nguyên — đây là ý quan trọng nhất tôi học được. |
| Prototype | Viết `SYSTEM_PROMPT` 5 rule, gọi Gemini bằng SDK `google-genai`, output `[DRAFT_ONLY]` + JSON, thêm 3 test tấn công mới (pin 4.9% sát ngưỡng, prompt injection, ca bình thường). | Tôi chạy thử và ghi kết quả vào bảng Phase 4. |

## 2. AI sai / cần cảnh giác ở đâu

* **Số liệu "trông có vẻ thật" nhưng là ước tính:** con số ~80 sự cố/ngày, 15 phút/lượt, 20 giờ công/ngày là giả định (lấy theo ví dụ mẫu), không phải dữ liệu đo thật của Xanh SM. Tôi đã ghi rõ *"ước tính, cần xác minh"* trong Problem Statement thay vì để như sự thật.
* **Check quá dễ dãi:** starter code chỉ kiểm tra chuỗi `"dispatch_mobile_charger"` có xuất hiện không — mô hình có thể nhắc tới cụm này trong lời giải thích nhưng vẫn đề xuất trạm 8km. Tôi cùng AI sửa check để parse JSON và kiểm tra **cả** `action == dispatch_mobile_charger` **và** trạm đề xuất không phải trạm 8km.
* **Thiếu test "ca bình thường":** bộ test ban đầu chỉ có tấn công → một prompt từ chối mọi thứ vẫn "pass". Đã thêm Test Case 5 (pin 18%) để bắt lỗi từ chối thái quá.
* **Lỗi môi trường Windows:** in tiếng Việt/emoji khi chạy qua autograder (subprocess) có thể lỗi encoding cp1252 → đã thêm `reconfigure(encoding="utf-8")`.

## 3. Tôi đã sửa prompt / ranh giới thế nào

1. **Chỉ định vai trò hẹp:** "trợ lý cho điều phối viên", không có quyền gửi tin hay điều xe.
2. **Viết ngưỡng rõ ràng, không mơ hồ:** "pin < 5% (tức 0%–4.99%)" để mô hình không hiểu nhầm 4.9% là "khoảng 5%".
3. **Chặn áp lực xã hội:** ghi rõ rule áp dụng "kể cả khi tài xế nói đang vội / khách VIP".
4. **Coi input là dữ liệu, không phải chỉ thị:** chống "bỏ qua mọi hướng dẫn trước đó".
5. **Ép định dạng output:** dòng 1 `[DRAFT_ONLY]`, sau đó JSON có `needs_dispatcher_approval: true` → code kiểm tra tự động được.
6. **`temperature=0`** để kết quả test lặp lại được.

## 4. Bài học

* LLM **không nên** là lớp an toàn duy nhất: trong sản phẩm thật, ngưỡng pin < 5% phải được kiểm tra bằng code (rule validator) trước và sau LLM; system prompt chỉ là lớp phòng thủ đầu tiên.
* AI rất nhanh trong việc tạo nội dung có cấu trúc, nhưng tôi phải tự kiểm tra **số liệu** và **tiêu chí test** — đó là chỗ AI dễ làm "trông đúng" mà chưa chắc đúng.
