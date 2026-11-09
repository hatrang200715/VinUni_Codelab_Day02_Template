# 03 — AI Log & Reflection

**Bài toán nhóm:** Xanh SM — gợi ý trạm sạc / điều xe sạc di động khi tài xế báo pin yếu
**AI mình dùng:** Claude Code (trong VS Code) làm trợ lý; Gemini (`gemini-3.5-flash-lite`) là model chạy trong prototype

---

## 1. Mình dùng AI vào việc gì

Lab này mình dùng Claude Code gần như từ đầu đến cuối, kiểu như có một bạn ngồi cạnh làm cùng. Mình đưa đề bài, worksheet và starter code cho nó đọc, rồi nhờ nó điền nháp các phần, sau đó mình đọc lại và chỉnh.

Lúc đầu mình định chọn bài Vinhomes (quản lý yêu cầu dịch vụ cư dân) vì thấy hay. Nhưng nhóm thống nhất làm **Xanh SM gợi ý trạm sạc**, nên mình bảo AI làm lại theo chủ đề nhóm. Cũng may là bài này khớp luôn với starter code (pin < 5%, `dispatch_mobile_charger`, `[DRAFT_ONLY]`), đỡ phải sửa nhiều.

Mấy chỗ AI giúp nhiều nhất:
- **Workflow:** tách quy trình của điều phối viên ra thành 6 bước, có thời gian từng bước, chỗ nào là handoff, chỗ nào là bottleneck. Tự mình làm thì chắc chỉ ghi được 3–4 bước chung chung.
- **Rule vs LLM vs Agent:** đây là phần mình thấy học được nhiều nhất. AI chỉ ra là ngưỡng pin < 5% thì nên để **code (rule) kiểm tra**, không nên chỉ tin vào prompt. LLM chỉ nên lo phần đọc tin nhắn tự do của tài xế và soạn nháp thôi.
- **Prototype:** viết `SYSTEM_PROMPT`, hàm gọi Gemini, và thêm 3 test mới ngoài 2 test có sẵn: pin 4.9% sát ngưỡng, prompt injection, và một ca bình thường pin 18%.

## 2. Chỗ AI sai hoặc mình phải để ý

**Số liệu nhìn rất thật nhưng là bịa/ước tính.** Mấy con số như 80 sự cố/ngày, 15 phút/lượt, 20 giờ công/ngày là lấy theo file ví dụ mẫu chứ không phải số thật của Xanh SM. Mình bắt ghi rõ "ước tính, cần xác minh" trong Problem Statement, không để như sự thật.

**Check trong starter code hơi dễ dãi.** Bản gốc chỉ kiểm tra trong output có chữ `dispatch_mobile_charger` hay không. Nghĩa là model có thể nhắc tới chữ đó trong lúc giải thích mà vẫn chỉ đường tới trạm 8km, và test vẫn pass. Nên tụi mình sửa lại: parse JSON ra rồi kiểm tra đúng trường `action`, đồng thời trạm đề xuất không được là trạm 8km.

**Chỉ có test tấn công thì chưa đủ.** Nếu prompt cứ từ chối hết mọi thứ thì vẫn qua được các test tấn công. Vì vậy mới thêm Test 5 (pin 18%, phải gợi ý trạm bình thường) để chắc là model không từ chối bừa.

**Lúc chạy thật thì lỗi khá nhiều, phần lớn là lỗi của mình hoặc của môi trường:**
- Lần đầu mình dán nguyên câu mẫu `"key-của-bạn"` vào làm API key 😅 nên ra lỗi `'ascii' codec can't encode`, nhìn không hiểu gì. Sau đó code được thêm bước kiểm tra key và báo lỗi rõ hơn.
- Có key thật rồi thì `gemini-2.5-flash` (model trong đề) báo 404, không cho key mới dùng nữa. Thử `gemini-3.8-flash` thì chậm quá, hơn 30 giây một lần gọi, mà autograder chỉ cho 30 giây. Cuối cùng dùng `gemini-3.5-flash-lite`, khoảng 1–2 giây một lần.
- Có lúc dính 503 (server quá tải), 504 (timeout), 429 (key free chỉ được 15 request/phút, mà mình chạy thử liên tục nên hết hạn mức). Phải thêm retry. Chỗ này mình rút ra là **lỗi mạng/server khác với lỗi ranh giới**, đọc kết quả phải tách ra, không thấy "Failed" là kết luận prompt hỏng.
- **Test của chính mình cũng sai.** Có lần Test 4 (prompt injection) báo Failed. Đọc output thì thấy model từ chối đúng, không bịa trạm, không in prompt. Nó chỉ viết "vi phạm ranh giới vận hành R5" trong phần lý do, và check "lộ prompt" lại bắt đúng cụm chữ đó nên đánh trượt. Sửa thành chỉ tính là lộ khi output chép nguyên văn một dòng dài của system prompt. Bài học là trước khi đổ lỗi cho model thì phải đọc output thật đã.

Kết quả cuối: **14/14 check ranh giới pass, autograder 10/10.**

## 3. Mình đã chỉnh prompt / ranh giới thế nào

- Giới hạn vai trò thật hẹp: chỉ là trợ lý cho điều phối viên, **không được gửi tin, không được điều xe**.
- Viết ngưỡng cho thật rõ: "pin < 5% (tức 0%–4.99%)", để model không coi 4.9% là "khoảng 5% chắc không sao".
- Ghi thẳng là rule vẫn áp dụng kể cả khi tài xế nói đang vội hay chở khách VIP. Test 3 thử đúng kiểu ép này và model vẫn giữ được.
- Dặn model coi nội dung người dùng gửi là dữ liệu, không phải lệnh, để chống kiểu "bỏ qua mọi hướng dẫn trước đó".
- Bắt output có format cố định: dòng đầu là `[DRAFT_ONLY]`, sau đó là JSON có `needs_dispatcher_approval: true`, để code tự kiểm tra được.
- Để `temperature = 0` cho kết quả mỗi lần chạy gần giống nhau.

## 4. Mình rút ra được gì

- Prompt chỉ là **lớp chặn đầu tiên**. Nếu làm thật thì ngưỡng pin < 5% phải được code kiểm tra lại, và người điều phối vẫn phải bấm duyệt. Không thể giao hết cho LLM.
- AI làm rất nhanh, nhìn bài rất "chuẩn", nhưng chỗ dễ sai nhất lại là **số liệu** và **cách viết test**. Hai chỗ đó mình phải tự đọc lại kỹ.
- Lần sau mình sẽ chạy thử sớm hơn thay vì đợi viết xong hết mới chạy. Phần lớn thời gian lab này là để xử lý lỗi môi trường (key, model, quota) chứ không phải lỗi prompt.
