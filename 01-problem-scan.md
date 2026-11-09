# 🔍 Phase 1 — SCAN (Cá nhân, 20 min)

Hãy sử dụng **4 Lenses** dưới đây để quét qua hoạt động vận hành của các công ty thành viên Vingroup. Ghi lại **ít nhất 5 bài toán/bottleneck** thực tế.

### 4 Lenses tìm bài toán AI cho Vingroup:

1. **Lặp lại (Repetitive):** Tác vụ lặp đi lặp lại nhiều lần hằng ngày. (Ví dụ: So khớp hóa đơn sạc điện tại VinFast, route lại chuyến taxi tại Xanh SM).
2. **Tốn thời gian (Time-consuming):** Tác vụ ngốn thời gian xử lý thủ công của nhân viên. (Ví dụ: Soạn thảo phản hồi đánh giá 1-star của cư dân Vinhomes).
3. **AI có thể tốt hơn (AI-upgrade):** Dịch vụ khách hàng hiện tại còn chậm hoặc phản hồi rập khuôn. (Ví dụ: Chatbot CSKH Vinpearl hỗ trợ đặt vé vui chơi).
4. **Pain từ người khác (Stakeholder Pain):** Bottleneck khiến khách hàng hoặc nhân viên thực địa phàn nàn. (Ví dụ: Tài xế Xanh SM phàn nàn về việc hệ thống gợi ý điểm đón khách không chính xác).

### 📝 List bài toán của tôi:

| #   | Subsidiary | Lens             | Mô tả ngắn bài toán                                                                                                                     |
| --- | ---------- | ---------------- | --------------------------------------------------------------------------------------------------------------------------------------- |
| 1   | Vinhomes   | Lặp lại          | Phân loại ticket ý kiến, phản ánh của dân cư đến các phòng ban phù hợp, tự động soạn phản hồi giải đáp cho các vấn đề lặp lại.          |
| 2   | XanhSM     | Tốn thời gian    | Nhân viên mất nhiều thời gian tổng hợp ticket tố cáo, dữ liệu hành trình và log giao tiếp để chuẩn bị hồ sơ xác minh.                   |
| 3   | Vinfast    | Lặp lại          | Kiểm tra lỗi ngoại quan trên dây chuyền sản xuất ô tô.                                                                                  |
| 4   | XanhSM     | Lặp lại          | Điều phối lại xe nhàn rỗi dựa trên thông số pin hiện tại, địa điểm và nhu cầu người dùng.                                               |
| 5   | Vinmec     | Tốn thời gian    | Nhân viên CSKH mất thời gian trả lời câu hỏi lặp lại và xác nhận lịch hẹn qua Zalo; chatbot là giải pháp có thể hỗ trợ quy trình này.   |
| 6   | Xanh SM    | Stakeholder Pain | Tài xế phải gọi khách hoặc xác nhận lại khi điểm đón trên ứng dụng không chính xác, làm tăng thời gian chờ và nguy cơ khách hủy chuyến. |

# 🃏 Phase 2 — QUICK-ASSESS (Cá nhân, 30 min)

Chọn **top 3 bài toán** từ danh sách trên và hoàn thiện **3 Quick Problem Cards** dưới đây (10 phút/card).

### QUICK PROBLEM CARD #___

| Trường | Nội dung |
|---|---|
| Bài toán (1 câu): | `________________________________________` |
| Công ty thành viên: | [ ] VinFast  [ ] Xanh SM  [ ] Vinhomes<br>[ ] Vinmec   [ ] Khác (Ghi rõ)________ |
| Ai đang đau (Actor)? | `______________________________________` |
| Workflow thủ công hiện tại (3-5 bước): | 1. ___ ──> 2. ___ ──> 3. ___ ──> 4. ___ |
| Bước nào tốn thời gian/lỗi nhất? | ___ (⏱ ___ phút/lượt) |
| AI có thể nhảy vào hỗ trợ ở bước nào? | `_____________________` |
| Đo thành công bằng gì (Metric có số)? | `____________________`<br>VD: "Giảm thời gian soạn phản hồi từ 10 min ──> under 2 min" |
| Quick Architecture: | [ ] No AI  [ ] Rule  [ ] LLM  [ ] Agent |

### QUICK PROBLEM CARD #1

| Trường | Nội dung |
|---|---|
| Bài toán (1 câu): | AI phân loại ticket, gợi ý phòng ban và soạn nháp trả lời dựa trên FAQ/quy định đã duyệt; nhân viên kiểm tra trước khi gửi. |
| Công ty thành viên: | [ ] VinFast  [ ] Xanh SM  [x] Vinhomes<br>[ ] Vinmec   [ ] Khác (Ghi rõ)________ |
| Ai đang đau (Actor)? | Nhân viên chăm sóc khách hàng |
| Workflow thủ công hiện tại (3-5 bước): | 1. Kiểm tra phiếu của khách hàng ──> 2. Điều phối phiếu đến phòng ban phù hợp ──> 3. Phản hồi đã tiếp nhận lại người dùng, kèm nội dung trả lời nếu sẵn có trong FAQ. |
| Bước nào tốn thời gian/lỗi nhất? | 2 (⏱ 3 phút/lượt) |
| AI có thể nhảy vào hỗ trợ ở bước nào? | Gợi ý phòng ban và soạn nháp từ FAQ/quy định đã duyệt; nhân viên kiểm tra trước khi gửi. |
| Đo thành công bằng gì (Metric có số)? | Gợi ý đúng phòng ban ≥90% trên bộ ticket gán nhãn; giảm thời gian 3 ──> dưới 1 phút/ticket. |
| Quick Architecture: | [ ] No AI  [ ] Rule  [x] LLM  [ ] Agent |

### QUICK PROBLEM CARD #2

| Trường | Nội dung |
|---|---|
| Bài toán (1 câu): | Tổng hợp bằng chứng từ phiếu tố cáo để nhân viên đánh giá nhất quán; AI không tự kết luận vi phạm. |
| Công ty thành viên: | [ ] VinFast  [x] Xanh SM  [ ] Vinhomes<br>[ ] Vinmec   [ ] Khác (Ghi rõ)________ |
| Ai đang đau (Actor)? | Nhân viên chăm sóc khách hàng |
| Workflow thủ công hiện tại (3-5 bước): | 1. Nhân viên nhận phiếu tố cáo ──> 2. Thu thập hành trình, log hội thoại và thông tin xác minh ──> 3. Nhân viên đánh giá theo tiêu chí ──> 4. Quyết định xử lý và gửi phản hồi. |
| Bước nào tốn thời gian/lỗi nhất? | 2 (⏱ 10 phút/lượt) |
| AI có thể nhảy vào hỗ trợ ở bước nào? | Tóm tắt bằng chứng, đánh dấu điểm cần xác minh và đề xuất hướng xử lý để nhân viên xem xét. AI không tự kết luận vi phạm, phạt, khóa tài khoản hoặc đóng khiếu nại. |
| Đo thành công bằng gì (Metric có số)? | Giảm thời gian xét duyệt từ 10 xuống dưới 3 phút/ticket; |
| Quick Architecture: | [ ] No AI  [x] Rule  [x] LLM  [ ] Agent |

### QUICK PROBLEM CARD #3

| Trường | Nội dung |
|---|---|
| Bài toán (1 câu): | Kiểm tra lỗi ngoại quan trên dây chuyền sản xuất ô tô. |
| Công ty thành viên: | [x] VinFast  [ ] Xanh SM  [ ] Vinhomes<br>[ ] Vinmec   [ ] Khác (Ghi rõ)________ |
| Ai đang đau (Actor)? | Nhân viên kiểm soát chất lượng. |
| Workflow thủ công hiện tại (3-5 bước): | 1. Quan sát, kiểm tra ngoại hình, chi tiết bên ngoài bằng mắt và công cụ ──> 2. Xác nhận xe đạt tiêu chuẩn về ngoại hình hay không ──> 3. Báo cáo rõ phần chi tiết bị lỗi |
| Bước nào tốn thời gian/lỗi nhất? | 1 (⏱ 20 phút/lượt) |
| AI có thể nhảy vào hỗ trợ ở bước nào? | Computer Vision phát hiện, định vị và lập báo cáo lỗi; QC xác nhận lỗi nghiêm trọng hoặc kết quả độ tin cậy thấp. |
| Đo thành công bằng gì (Metric có số)? | Giảm thời gian kiểm tra từ 20 xuống dưới 10 phút/xe, đồng thời theo dõi tỷ lệ bỏ sót lỗi và báo lỗi nhầm. |
| Quick Architecture: | Computer Vision (ML) |
