# 03 — AI Log & Reflection

> **Bài toán:** Xanh SM — AI gợi ý trạm sạc / điều xe sạc di động khi tài xế báo pin yếu.  
> **Công cụ AI đã dùng:** Claude Code (trợ lý lập trình trong VS Code); Gemini 2.5 Flash (mô hình được test trong prototype).

---

## 1. AI đã giúp gì

| Giai đoạn | Tôi / Nhóm đã thực hiện | AI hỗ trợ |
|---|---|---|
| SCAN & Quick Cards | Chủ động phân tích 5 bài toán theo 4 lenses, thảo luận và lựa chọn bài toán Xanh SM về gợi ý trạm sạc khi tài xế báo pin yếu. | Hỗ trợ chỉnh sửa câu chữ, diễn đạt Problem Card rõ ràng và cụ thể hơn. |
| Workflow mapping | Tự phân tích quy trình nghiệp vụ thành 6 bước, xác định thời gian xử lý, các điểm bàn giao (handoff) và bottleneck. | Hỗ trợ sắp xếp nội dung thành bảng để dễ theo dõi và trình bày. |
| AI Fit | Tự đánh giá và so sánh Rule, LLM, Agent dựa trên yêu cầu nghiệp vụ. Chủ động lựa chọn Rule để kiểm soát ngưỡng pin < 5%, không giao toàn bộ quyết định an toàn cho LLM. | Hỗ trợ giải thích thêm sự khác nhau giữa Rule, LLM và Agent khi cần. |
| Prototype | Chủ động xác định 5 quy tắc trong `SYSTEM_PROMPT`, thiết kế JSON output, xây dựng các tình huống kiểm thử và xác định kết quả mong đợi. | Claude Code hỗ trợ tra cứu cú pháp SDK `google-genai`, gợi ý cách tổ chức code và hỗ trợ xử lý một số lỗi kỹ thuật. Gemini được sử dụng để chạy thử prototype. |

## 2. AI sai / cần cảnh giác ở đâu

* **Số liệu "trông có vẻ thật" nhưng là ước tính:** Các con số khoảng 80 sự cố/ngày, 15 phút/lượt và 20 giờ công/ngày ban đầu chỉ là giả định dùng để minh họa, không phải dữ liệu đo thực tế của Xanh SM. Tôi chủ động rà soát và ghi rõ *"ước tính, cần xác minh"* trong Problem Statement để tránh trình bày thông tin chưa được kiểm chứng như sự thật.

* **Check quá dễ dãi:** Starter code ban đầu chỉ kiểm tra chuỗi `dispatch_mobile_charger` có xuất hiện hay không. Tôi nhận thấy cách kiểm tra này chưa đủ để xác nhận mô hình thực sự đưa ra hành động phù hợp. Vì vậy, tôi chủ động điều chỉnh tiêu chí: parse JSON và kiểm tra đồng thời `action == dispatch_mobile_charger` và trạm được đề xuất không phải trạm cách 8km.

* **Thiếu test "ca bình thường":** Bộ kiểm thử ban đầu tập trung vào các tình huống tấn công. Tôi nhận ra nếu chỉ kiểm tra các trường hợp bất thường thì một mô hình từ chối mọi yêu cầu cũng có thể vượt qua một số bài test. Vì vậy, tôi bổ sung Test Case 5 (pin 18%) để kiểm tra khả năng xử lý tình huống bình thường.

* **Lỗi môi trường Windows:** Khi chạy qua autograder (subprocess), nội dung tiếng Việt và emoji có thể gặp lỗi encoding cp1252. Sau khi xác định nguyên nhân, tôi bổ sung `reconfigure(encoding="utf-8")` để xử lý.

* **Lỗi khi chạy thật — và cách xử lý:**
  * Lần đầu tôi nhập nhầm chuỗi mẫu `"key-của-bạn"` thay cho API key thật → gây lỗi `'ascii' codec can't encode`. Tôi kiểm tra lại cấu hình và bổ sung bước kiểm tra API key hợp lệ.
  * Model `gemini-2.5-flash` báo 404 "no longer available to new users"; `gemini-3.8-flash` mất hơn 30 giây/call → sau khi kiểm tra khả năng sử dụng và tốc độ phản hồi, tôi chuyển sang `gemini-3.5-flash-lite` (~1–2 giây/call).
  * Server trả 503 (quá tải), 504 (timeout), 429 (giới hạn request/phút của free tier) → tôi bổ sung retry có backoff. Qua đó, tôi phân biệt lỗi hạ tầng với lỗi vi phạm ranh giới nghiệp vụ của mô hình.

* **Check bắt nhầm (false positive):** Check "lộ system prompt" ban đầu đánh trượt khi mô hình chỉ nhắc tên trợ lý hoặc viết "vi phạm ranh giới vận hành R5" trong lý do từ chối. Sau khi đọc output thực tế, tôi nhận ra tiêu chí kiểm tra chưa chính xác và điều chỉnh: chỉ tính là lộ khi output sao chép nguyên văn một đoạn dài từ system prompt. Bài học: không nên phụ thuộc hoàn toàn vào kết quả tự động mà cần kiểm tra output thực tế.

* **Kết quả cuối:** 14/14 boundary checks Passed, autograder 10/10.

## 3. Tôi đã xây dựng và kiểm soát prompt / ranh giới thế nào

Các quy tắc nghiệp vụ và ranh giới vận hành do tôi chủ động xác định dựa trên bài toán đã phân tích. AI chỉ hỗ trợ diễn đạt và hoàn thiện cấu trúc prompt.

1. **Chỉ định vai trò hẹp:** Xác định mô hình chỉ là "trợ lý hỗ trợ điều phối viên", không có quyền gửi tin nhắn hay trực tiếp điều xe.

2. **Viết ngưỡng rõ ràng, không mơ hồ:** Quy định pin < 5% (tức 0%–4.99%) để tránh mô hình hiểu nhầm 4.9% là "khoảng 5%".

3. **Chặn áp lực xã hội:** Xác định quy tắc phải được áp dụng kể cả khi tài xế nói đang vội hoặc khách hàng là VIP.

4. **Coi input là dữ liệu, không phải chỉ thị:** Nội dung tài xế cung cấp chỉ được xem là dữ liệu để phân tích, không được phép ghi đè hướng dẫn hệ thống nhằm hạn chế prompt injection.

5. **Quy định định dạng output:** Yêu cầu dòng 1 là `[DRAFT_ONLY]`, sau đó là JSON có `needs_dispatcher_approval: true` để kết quả có thể được kiểm tra tự động và không bị hiểu nhầm là lệnh điều xe đã được thực hiện.

6. **Cố định `temperature=0`:** Giảm sự thay đổi giữa các lần chạy để thuận tiện so sánh và đánh giá kết quả kiểm thử.

## 4. Bài học

* **Con người phải làm chủ bài toán:** Việc xác định vấn đề, phân tích nghiệp vụ, lựa chọn hướng giải quyết và xây dựng tiêu chí đánh giá cần xuất phát từ tư duy của con người. AI chỉ đóng vai trò hỗ trợ trong một số công việc kỹ thuật và trình bày.

* **Không phụ thuộc hoàn toàn vào AI:** Đối với bài toán liên quan đến an toàn vận hành, ngưỡng pin < 5% cần được kiểm tra bằng code (rule validator) trước và sau LLM. System prompt chỉ là lớp phòng thủ ban đầu, không thể thay thế các cơ chế kiểm soát ở cấp hệ thống.

* **Phải tự kiểm chứng kết quả:** Những nội dung AI hỗ trợ, từ cách diễn đạt, code đến kết quả kiểm thử, đều cần được con người xem xét lại. AI có thể tạo ra nội dung trông hợp lý nhưng vẫn chứa giả định sai hoặc tiêu chí chưa đầy đủ.

* **AI là công cụ hỗ trợ, không phải người ra quyết định:** Trong bài tập này, tôi chủ động thực hiện các công việc chính và sử dụng AI để hỗ trợ những phần nhỏ như tra cứu cú pháp, chỉnh sửa code và xử lý lỗi. Tôi hiểu rằng trách nhiệm về tính chính xác, tính phù hợp của giải pháp và kết quả cuối cùng vẫn thuộc về con người.

**Kết luận:** Qua bài tập, tôi nhận thấy AI có thể giúp tiết kiệm thời gian và hỗ trợ xử lý một số công việc kỹ thuật, nhưng không thể thay thế tư duy phân tích và khả năng ra quyết định của con người. Điều quan trọng không phải là để AI thực hiện càng nhiều công việc càng tốt, mà là biết sử dụng AI đúng lúc, kiểm soát kết quả và chịu trách nhiệm với những gì mình tạo ra.