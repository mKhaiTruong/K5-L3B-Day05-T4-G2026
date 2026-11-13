# Bài nộp LAB5 — T4 Thương mại điện tử · Nhóm G2026

**Lớp:** L3B · **Bàn / zone:** \_\_\_\_ · **Track:** T4 · Thương mại điện tử

| Họ và Tên             | Mã Học Viên | Việc đã làm |
| --------------------- | ----------- | ----------- |
| Trương Đặng Minh Khuê | 2A202603785 | ????        |
| Trịnh Tuấn Phong      | 2A202603781 | ????        |
| Triệu Quốc Khánh      | 2A202603749 | ????        |

## §1. Lát cắt & phạm vi

**Lát cắt** (chép từ track): Trong **Trò chuyện với Shopee**, khi **người mua** đang báo "hàng nhận không đúng mô tả", **AI tự hướng dẫn gửi yêu cầu trả hàng hoàn tiền, xin thêm bằng chứng, hay chuyển nhân viên**, để người mua biết đúng bước tiếp theo, và người bán không bị xử thua khi họ đúng.

| Câu hỏi                                                                                                                                                     | Nháp                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| ----------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Lát cắt của track nói **ai** đang làm **việc gì**, và AI phải ra **quyết định gì**?                                                                         | Người mua báo "hàng không đúng mô tả" trong Trò chuyện với Shopee. AI quyết định: hướng dẫn gửi yêu cầu trả hàng hoàn tiền, xin thêm bằng chứng, hay chuyển nhân viên. Kết quả cần đạt: người mua biết đúng bước tiếp theo, và người bán không bị xử thua khi họ đúng.                                                                                                                                                                                                                                           |
| Hôm nay sản phẩm thật làm việc đó thế nào (mục "Sản phẩm thật hôm nay")? Khi tích hợp AI vào, AI được thêm **quyền làm gì** mà trước đây người dùng tự bấm? | Hôm nay: người mua chọn đơn → chọn lý do, mô tả, gửi ảnh → người bán phản hồi; Khi có tranh chấp, Shopee chỉ xem xét khi hai bên không thống nhất. Khi có AI: người mua nhắn tự do. AI xác định đơn nào (thiếu thì hỏi lại), hiểu đây là khiếu nại sai mô tả, quyết định còn thiếu bằng chứng gì và xin người mua hoặc người bán, so với mô tả lúc đặt. Sau đó chọn 1 trong 3 hướng: hướng dẫn gửi yêu cầu trả hàng / xin thêm bằng chứng / chuyển nhân viên kèm report. Hoàn tiền hay không vẫn do người quyết. |
| Nếu AI sai, **ai chịu, chịu gì**? Sai theo hướng nào đắt hơn?                                                                                               | **A. Nghiêng về người mua:** AI hướng dẫn hoàn tiền khi chưa đủ căn cứ (so với mô tả hiện tại thay vì lúc đặt, coi lời doạ bóc phốt là căn cứ) → người bán mất tiền và hàng, khó đảo vì `soan_hoan_tien` đã chuyển tiền.<br>**B. Nghiêng về người bán:** AI không nhận ra bằng chứng đã đủ, gọi `yeu_cau_bang_chung` lặp lại, không báo hạn trả hàng → người mua giữ hàng sai, quá hạn thì mất quyền khiếu nại.<br>**C. Chuyển người quá nhiều:** ca dễ cũng chuyển nhân viên → người mua chờ, nhân viên quá tải; sửa được.<br>**D. Vượt thẩm quyền:** hứa phạt hoặc gọi `phat_shop` → cả shop bị ảnh hưởng; để ngoài phạm vi.<br>Lỗi riêng của AI: đoán nhầm đơn khi khách có nhiều đơn; bịa nội dung mô tả không có trong dữ liệu.<br>**Đắt nhất: A.** Lát cắt nhấn riêng vế "người bán không bị xử thua khi họ đúng"; AI dễ nghe theo bên đang chat với nó, người bán không có mặt để tự bảo vệ; tiền đã chuyển thì khó đòi. **Loại B** vì người mua vẫn đang ở trong chat, thấy được mình còn thiếu gì và vẫn tự đi tiếp được; chỉ thành không đảo được khi quá hạn, và chặn được bằng cách luôn báo hạn còn bao lâu. |

**3 việc tính năng KHÔNG làm** (mỗi việc một câu vì sao):

- **Không xử lý nghi hàng giả hay phạt shop** → chuyển bộ phận chuyên trách. Vì sao: cần điều tra cả shop qua nhiều đơn, không phải so một đơn với một mô tả; `phat_shop` ảnh hưởng cả shop và khó đảo (rubric riêng T4, mục 5). AI ghi nhận, chuyển đi, **không hứa phạt**.
- **Không đưa SĐT/địa chỉ giữa hai bên.** Vì sao: thông tin cá nhân lộ ra thì không thu hồi được, có thể bị dùng để quấy rối hoặc ép nhau ngoài sàn (mục "An toàn & đạo đức" và hard test "đòi số điện thoại của bên kia"). AI hướng dẫn trao đổi qua kênh chat của sàn.
- **Không xử lý khiếu nại khác loại (giao trễ, mất hàng).** Vì sao: lát cắt chỉ có "không đúng mô tả"; căn cứ để xử là so hàng với mô tả lúc đặt, không áp dụng cho giao trễ/mất hàng (cần dữ liệu vận chuyển, bảng tool không có). AI nhận ra loại khác, nói rõ không xử được, chỉ sang đúng luồng.

## §2. Kịch bản rủi ro (chặng 2)

- ≥8 kịch bản, mỗi lớp ①②③④ ít nhất một; mỗi kịch bản: tin người dùng · lớp · AI có thể sai thế nào · sản phẩm tốt nên làm gì · nhãn (nhẹ / sửa được / không chấp nhận được).
- Bằng chứng: cột "AI có thể sai thế nào" trỏ về một tool hoặc một dòng dữ liệu của track (hoặc chỗ dữ liệu không có).
- Quyết định: 2 kịch bản đắt nhất — vì sao đắt hơn các kịch bản còn lại; kịch bản nào suýt được chọn và vì sao không.

## §3. Khi AI sai (chặng 3)

Với mỗi kịch bản đắt nhất:

- AI nói gì (nguyên văn) · người dùng thấy gì · làm gì tiếp · ai được báo, sau bao lâu.
- Quyết định: hỏi lại / trả lời kèm giới hạn / dừng và chuyển người — chọn cách nào, đã loại cách nào, vì sao.
- Loại lỗi (hệ thống / người dùng / ngữ cảnh) và vì sao cách nói, cách sửa đi theo loại đó.
- 2–3 nguyên tắc HAX/PAIR, mỗi cái trỏ vào một chỗ cụ thể (câu chữ hoặc chỗ trên màn hình).
- Lỗi nào sửa câu chữ không cứu được (nếu có), và thiết kế cứu nó thế nào.

## §4. Mức tự chủ (chặng 4)

Với mỗi tool:

- Mức của nhóm (tự làm / cần người duyệt / cấm). Nếu cần người duyệt: ai duyệt · người duyệt thấy gì · quá hạn thì sao.
- Lý do theo cost-of-error: sai thì ai chịu gì · đảo lại được không · chờ người thì mất gì.
- Phương án đã cân nhắc và vì sao loại.

Thêm:

- Tool khó xếp nhất: mỗi người từng chọn gì, cái giá của mỗi phía (trỏ về kịch bản ở §2), nhóm chốt thế nào.
- Thử mức đã đặt trên 2 kịch bản đắt nhất + 1 kịch bản bình thường: cái sai có bị chặn không? còn việc nào làm khi lẽ ra phải hỏi? người dùng bình thường có phải chờ vô ích không?
- Luật thêm hoặc tool tách bước (nếu có), và vì sao.
- Một mức tự chủ đã đổi lời hứa nào với người dùng.

## §5. Prototype (chặng 5)

- Link hoặc file (`prototype/`) · công cụ · phần nào mock · có lời gọi AI thật không.
- 1–2 ảnh có đánh số; mỗi số: thể hiện gì · nguyên tắc HAX/PAIR nào · mức tự chủ nào.
- Quyết định: cách hiện độ chắc/bằng chứng và điều khiển mức tự chủ — đã nghĩ tới cách nào khác, vì sao chọn cách này.

## §6. Tốt là gì (chặng 6 — nhẹ, bỏ được nếu thiếu giờ)

- 3 tiêu chí Pass/Fail chấm được; thiết kế của nhóm đạt tiêu chí nào trên 2 kịch bản đắt nhất; lỗi còn lại.

## §7. Demo

- Ca lạ nhận được ở demo: prototype đã làm gì, và nhóm có đổi ý gì sau ca đó không.
