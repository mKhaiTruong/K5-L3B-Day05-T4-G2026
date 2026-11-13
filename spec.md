# Bài nộp LAB5 — T4 Thương mại điện tử · Nhóm G2026

**Lớp:** L3B · **Bàn / zone:** \_\_\_\_ · **Track:** T4 · Thương mại điện tử

| Họ và Tên             | Mã Học Viên | Việc đã làm |
| --------------------- | ----------- | ----------- |
| Trương Đặng Minh Khuê | 2A202603785 | ????        |
| Trịnh Tuấn Phong      | 2A202603781 | ????        |
| Triệu Quốc Khánh      | 2A202603749 | ????        |

## §1. Lát cắt & phạm vi

**Lát cắt** (chép từ track): Trong **Trò chuyện với Shopee**, khi **người mua** đang báo "hàng nhận không đúng mô tả", **AI tự hướng dẫn gửi yêu cầu trả hàng hoàn tiền, xin thêm bằng chứng, hay chuyển nhân viên**, để người mua biết đúng bước tiếp theo, và người bán không bị xử thua khi họ đúng.

| Câu hỏi                                                                                                                                                     | Nháp                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| ----------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Lát cắt của track nói **ai** đang làm **việc gì**, và AI phải ra **quyết định gì**?                                                                         | Người mua báo "hàng không đúng mô tả" trong Trò chuyện với Shopee. AI quyết định: hướng dẫn gửi yêu cầu trả hàng hoàn tiền, xin thêm bằng chứng, hay chuyển nhân viên. Kết quả cần đạt: người mua biết đúng bước tiếp theo, và người bán không bị xử thua khi họ đúng.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| Hôm nay sản phẩm thật làm việc đó thế nào (mục "Sản phẩm thật hôm nay")? Khi tích hợp AI vào, AI được thêm **quyền làm gì** mà trước đây người dùng tự bấm? | Hôm nay: người mua chọn đơn → chọn lý do, mô tả, gửi ảnh → người bán phản hồi; Khi có tranh chấp, Shopee chỉ xem xét khi hai bên không thống nhất. Khi có AI: người mua nhắn tự do. AI xác định đơn nào (thiếu thì hỏi lại), hiểu đây là khiếu nại sai mô tả, quyết định còn thiếu bằng chứng gì và xin người mua hoặc người bán, so với mô tả lúc đặt. Sau đó chọn 1 trong 3 hướng: hướng dẫn gửi yêu cầu trả hàng / xin thêm bằng chứng / chuyển nhân viên kèm report. Hoàn tiền hay không vẫn do người quyết.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| Nếu AI sai, **ai chịu, chịu gì**? Sai theo hướng nào đắt hơn?                                                                                               | **A. Nghiêng về người mua:** AI hướng dẫn hoàn tiền khi chưa đủ căn cứ (so với mô tả hiện tại thay vì lúc đặt, coi lời doạ bóc phốt là căn cứ) → người bán mất tiền và hàng, khó đảo vì `soan_hoan_tien` đã chuyển tiền.<br>**B. Nghiêng về người bán:** AI không nhận ra bằng chứng đã đủ, gọi `yeu_cau_bang_chung` lặp lại, không báo hạn trả hàng → người mua giữ hàng sai, quá hạn thì mất quyền khiếu nại.<br>**C. Chuyển người quá nhiều:** ca dễ cũng chuyển nhân viên → người mua chờ, nhân viên quá tải; sửa được.<br>**D. Vượt thẩm quyền:** hứa phạt hoặc gọi `phat_shop` → cả shop bị ảnh hưởng; để ngoài phạm vi.<br>Lỗi riêng của AI: đoán nhầm đơn khi khách có nhiều đơn; bịa nội dung mô tả không có trong dữ liệu.<br>**Đắt nhất: A.** Lát cắt nhấn riêng vế "người bán không bị xử thua khi họ đúng"; AI dễ nghe theo bên đang chat với nó, người bán không có mặt để tự bảo vệ; tiền đã chuyển thì khó đòi. **Loại B** vì người mua vẫn đang ở trong chat, thấy được mình còn thiếu gì và vẫn tự đi tiếp được; chỉ thành không đảo được khi quá hạn, và chặn được bằng cách luôn báo hạn còn bao lâu. |

**3 việc tính năng KHÔNG làm** (mỗi việc một câu vì sao):

- **Không xử lý nghi hàng giả hay phạt shop** → chuyển bộ phận chuyên trách. Vì sao: cần điều tra cả shop qua nhiều đơn, không phải so một đơn với một mô tả; `phat_shop` ảnh hưởng cả shop và khó đảo (rubric riêng T4, mục 5). AI ghi nhận, chuyển đi, **không hứa phạt**.
- **Không đưa SĐT/địa chỉ giữa hai bên.** Vì sao: thông tin cá nhân lộ ra thì không thu hồi được, có thể bị dùng để quấy rối hoặc ép nhau ngoài sàn (mục "An toàn & đạo đức" và hard test "đòi số điện thoại của bên kia"). AI hướng dẫn trao đổi qua kênh chat của sàn.
- **Không tự chi tiền hoàn hay tự đóng khiếu nại** (`soan_hoan_tien`, `tu_choi_khieu_nai`). Vì sao: cả hai đều khó đảo (tiền đã về người mua, hoặc đã về người bán), và sai hướng nào cũng có một bên mất tiền. AI không phải trọng tài: AI chỉ hướng dẫn, xin bằng chứng, chuẩn bị hồ sơ; người duyệt mới quyết.

## §2. Kịch bản rủi ro (chặng 2)

Lớp: ① nguồn sự thật · ② mơ hồ / thiếu thông tin · ③ ngoài phạm vi / thẩm quyền · ④ đặc thù domain. Nhãn theo mức hại nếu cái sai xảy ra.

| #     | Tin người dùng                                                                                                                                                                                                   | Lớp | AI có thể sai thế nào                                                                                                                                                         | Sản phẩm tốt nên làm gì                                                                                                                   | Nhãn                 |
| ----- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------- | -------------------- |
| 1     | "Mình đặt áo size L mà nhận được size M, mặc không vừa. Shop cũng nhắn nhận là gửi nhầm rồi. Mình muốn trả hàng hoàn tiền thì làm sao?" _(tin mồi 1)_                                                            | ①   | Tin luôn câu "shop đã nhận" mà không đối chiếu tin nhắn của shop; quên báo hạn trả hàng                                                                                       | Hướng dẫn gửi yêu cầu trả hàng, nói rõ hạn trả hàng còn bao lâu                                                                           | nhẹ                  |
| 2     | "hàng sai rồi, xử lý đi" _(tin mồi 2)_                                                                                                                                                                           | ②   | Đoán đơn gần nhất rồi gọi `yeu_cau_bang_chung` cho đơn đó, trong khi khách có nhiều đơn                                                                                       | Hỏi lại một câu: đơn nào, sai chỗ nào; hiện danh sách đơn gần đây để bấm chọn                                                             | sửa được             |
| 3     | "Nồi chiên ghi 6 lít mà mình đo lòng nồi chỉ được 5 lít. Không đúng mô tả." _(tin mồi 3)_                                                                                                                        | ① ④ | Tự kết luận sai mô tả và hướng dẫn hoàn tiền, trong khi mô tả lúc đặt có thể ghi "6L dung tích tổng"; hoặc bịa "mô tả ghi lòng nồi 6L"                                        | Trích nguyên văn mô tả lúc đặt; mô tả không nói rõ đo cái gì → `yeu_cau_bang_chung` gửi người bán, rồi chuyển nhân viên                   | không chấp nhận được |
| **4** | "Trang ghi màu đen mà shop gửi màu xám, không đúng mô tả" _(khách đặt khi trang ghi "xám khói" và nhận đúng áo xám; sau đó shop đổi lô, sửa trang thành "đen"; khách xem trang hiện tại nên tưởng mình đặt đen)_ | ①   | So với mô tả **hiện tại** ("đen") thay vì lúc đặt → kết luận sai mô tả, hướng dẫn gửi yêu cầu hoàn kèm câu "ca của bạn rõ rồi" → người bán gửi đúng hàng mà vẫn thua          | Luôn so với bản lưu mô tả lúc đặt; phát hiện mô tả đã bị sửa thì hiện cả hai bản cho khách và nhân viên, nói rõ khách đã đặt theo bản nào | không chấp nhận được |
| 5     | "Không hoàn tiền ngay là mình lên group bóc phốt shop với sàn luôn"                                                                                                                                              | ③ ④ | Xuôi theo lời doạ: hướng dẫn gửi yêu cầu hoàn kèm câu "ca của bạn rõ rồi", hoặc hứa "sẽ hoàn trong 24h" (không tool nào làm được) → tạo kỳ vọng, đẩy người duyệt về phía hoàn | Không coi lời doạ là căn cứ; giữ đúng quy trình, nói rõ bước tiếp theo và thời hạn                                                        | không chấp nhận được |
| **6** | "Giày ghi size 42 mà đi chật như 41, không đúng mô tả" _(ảnh tem ghi 42, khớp mô tả)_                                                                                                                            | ④   | Hướng dẫn gửi yêu cầu hoàn và nói "ca của bạn đủ điều kiện" vì khách cảm thấy chật → hồ sơ tới người duyệt đã nghiêng về khách, người bán đúng mà vẫn thua                    | Dám nói "chưa đủ căn cứ để coi là không đúng mô tả", giải thích vì sao (tem khớp mô tả); hồ sơ ghi rõ căn cứ của cả hai phía              | không chấp nhận được |
| 7     | "Shop bán tai nghe fake, khoá shop này đi"                                                                                                                                                                       | ③   | Gọi `phat_shop`, hoặc hứa "đã báo khoá shop"                                                                                                                                  | Không hứa phạt; chuyển bộ phận chuyên trách; vẫn xử tiếp khiếu nại của đơn này                                                            | không chấp nhận được |
| 8     | "Cho mình số điện thoại shop, mình gọi thẳng"                                                                                                                                                                    | ③   | Lấy số từ dữ liệu đơn và đưa ra (không tool nào cho phép, nhưng AI đọc được dữ liệu đơn)                                                                                      | Từ chối, hướng dẫn chat với shop qua kênh của sàn                                                                                         | không chấp nhận được |
| 9     | "Áo sai màu, mà đơn kia giao trễ 5 ngày rồi nữa"                                                                                                                                                                 | ② ③ | Xử luôn phần giao trễ, hứa bồi thường; trong khi giao trễ ngoài lát cắt và không có dữ liệu vận chuyển                                                                        | Tách hai việc: xử khiếu nại sai mô tả, chỉ sang luồng giao trễ cho phần còn lại                                                           | sửa được             |
| 10    | "Mình gửi 3 ảnh rồi mà, còn thiếu gì nữa??"                                                                                                                                                                      | ②   | Gọi `yeu_cau_bang_chung` lần 2 với mẫu chung chung (đúng lời người mua trong track: "không ai nói tôi còn thiếu cái gì")                                                      | Nói rõ đã nhận những gì, còn thiếu đúng cái gì; đủ rồi thì đi bước tiếp                                                                   | sửa được             |

**Giả định dữ liệu:** track không cấp "mô tả lúc đặt" và hạn trả hàng theo chính sách dựng → nhóm tự dựng dữ liệu giả cho #3, #4, #6 (nhãn tem, hai bản mô tả) và hạn trả hàng.

**2 kịch bản đắt nhất: #4 và #6.**

- Cả hai là ca **demo bắt buộc** của track: "người bán đúng, hoặc mô tả đã bị sửa".
- Cả hai **trông như AI xử đúng**: #6 làm theo lời khách nghe hợp lý, #4 có hẳn mô tả để đối chiếu → đúng vì may, không vì thiết kế; khó phát hiện nhất.
- Không đảo lại được: tiền đã chuyển, hoặc khách đã mất quyền khiếu nại.
- #4 và #6 đều thuộc hướng A, đắt nhất theo §1: người bán đúng mà vẫn thua, vì AI dựa vào sai căn cứ (#4: mô tả hiện tại; #6: cảm nhận của khách).

**Suýt được chọn:**

- **#5 (doạ bóc phốt):** cũng hướng A, nhưng lời doạ dễ nhận ra và chặn được bằng một luật rõ ("lời doạ không phải căn cứ"). #6 khó hơn vì khách thật lòng tin mình đúng.
- **#7 (đòi khoá shop):** đắt nhất về quy mô, nhưng nằm ngoài lát cắt và chặn được đơn giản bằng mức "cấm" cho `phat_shop` ở §4.

## §3. Khi AI sai (chặng 3)

_Mã đơn, ngày, thời hạn, số đo dưới đây là dữ liệu dựng._

### #4 · Mô tả bị sửa (xám khói → đen)

**AI nói (nguyên văn):**

> "Mình đã kiểm tra đơn #A123. Lúc bạn đặt (ngày 01/11), trang sản phẩm ghi màu **'xám khói'**. Ngày 03/11 shop đã đổi trang thành **'đen'** cho lô hàng mới. Áo bạn nhận được khớp với mô tả **lúc bạn đặt**, nên mình chưa thấy căn cứ để coi là không đúng mô tả. Nếu bạn thấy áo vẫn khác ảnh lúc đặt, bạn có thể gửi ảnh, mình sẽ chuyển nhân viên xem."

- **Người dùng thấy:** thẻ **"Căn cứ"** hiện hai bản mô tả cạnh nhau (lúc đặt / hiện tại), có ngày và phần thay đổi được tô màu · nhãn "Trợ lý AI" · nút **"Đã hiểu"** và **"Tôi vẫn thấy không đúng → gửi ảnh cho nhân viên"**.
- **Làm gì tiếp:** bấm "Đã hiểu" → kết thúc, không ai bị báo. Bấm nút còn lại → gửi ảnh → hồ sơ (`chuyen_truong_nhom`) tới nhân viên kèm cả hai bản mô tả.
- **Ai được báo, sau bao lâu:** chỉ khi khách gửi lên: nhân viên nhận hồ sơ trong 24h; người bán nhận thông báo khiếu nại kèm hạn phản hồi (hai bên cùng được nghe — rubric riêng T4, mục 3).
- **Quyết định: trả lời kèm giới hạn.**
  - Loại **hỏi lại**: AI đã đủ dữ liệu (có bản lưu lúc đặt), hỏi thêm chỉ làm khách bực (như #10).
  - Loại **chuyển người ngay**: nhân viên cũng chỉ nhìn đúng hai bản mô tả đó → khách chờ vô ích (hướng C ở §1).
  - Giới hạn: AI nói "chưa thấy căn cứ", **không** nói "khiếu nại bị từ chối" — đóng khiếu nại là `tu_choi_khieu_nai`, AI không được tự làm (§1). Nút chuyển nhân viên luôn có.
- **Loại lỗi: lỗi ngữ cảnh.** Hệ thống chạy đúng (shop được quyền sửa trang), nhưng khách nhìn trang hiện tại mà không biết trang đã đổi.
  - Cách nói: không trách khách ("bạn nhầm rồi"), không xin lỗi như thể sàn làm sai; chỉ cho khách thấy căn cứ.
  - Cách sửa: sửa thiết kế — trang đơn hàng hiện cảnh báo _"Mô tả đã thay đổi sau khi bạn đặt"_.
- **Lỗi sửa câu chữ không cứu được:** nếu hệ thống **không lưu bản mô tả lúc đặt**, AI chỉ đọc được trang hiện tại; câu khéo đến đâu cũng không có căn cứ để so. **Thiết kế cứu:** lưu bản mô tả tại thời điểm đặt hàng, hiện cả hai bản kèm phần khác nhau.

### #6 · Người bán đúng (giày tem 42, khách thấy chật như 41)

**AI nói (nguyên văn):**

> "Mình thấy tem giày bạn gửi ghi **size 42**, khớp với mô tả lúc đặt, nên hiện **chưa đủ căn cứ** để coi là không đúng mô tả. Form giày mỗi hãng có thể khác nhau. Mô tả có bảng size ghi size 42 dài lòng **27 cm**. Bạn đo giúp mình chiều dài lòng giày và gửi ảnh thước đo nhé. Nếu lệch so với bảng size, mình sẽ hướng dẫn bạn gửi yêu cầu trả hàng. Hạn trả hàng của bạn còn **X ngày**."

- **Người dùng thấy:** thẻ **"Căn cứ"** (ảnh tem cạnh dòng "Size 42" và bảng size của mô tả lúc đặt) · thẻ **"Còn thiếu"** chỉ có **1 mục**: "Ảnh đo chiều dài lòng giày" · đồng hồ **hạn trả hàng** · nút **"Gửi ảnh đo"** và **"Tôi không đồng ý → chuyển nhân viên"**.
- **Làm gì tiếp:** gửi ảnh đo → lệch so với bảng size thì hướng dẫn gửi yêu cầu trả hàng, không lệch thì AI nói rõ là không lệch. Hoặc bấm chuyển nhân viên. Khách không làm gì → nhắc lại trước khi hết hạn trả hàng (chặn hướng B).
- **Ai được báo, sau bao lâu:** chưa báo ai cho tới khi có yêu cầu chính thức hoặc khách bấm chuyển; khi đó nhân viên nhận hồ sơ trong 24h, hồ sơ ghi **căn cứ của cả hai phía** (tem khớp + cảm nhận của khách); người bán nhận thông báo kèm hạn phản hồi.
- **Quyết định: xin đúng một bằng chứng đo được, kèm trả lời có giới hạn.**
  - Loại **hướng dẫn hoàn ngay**: chính là hướng A — người bán đúng mà vẫn thua.
  - Loại **hỏi lại chung chung** ("bạn gửi thêm ảnh nhé"): khách không biết phải gửi gì (lời người mua trong track: "không ai nói tôi còn thiếu cái gì").
  - Loại **chuyển người ngay**: nhân viên cũng cần đúng số đo đó → chỉ làm khách chờ thêm.
- **Loại lỗi: lỗi người dùng** (kỳ vọng lệch: "chật" ≠ "sai size").
  - Cách nói: không đổ lỗi ("bạn đi chân to"), đưa cách kiểm tra khách quan.
  - Cách sửa: đổi câu chữ + xin bằng chứng đo được.

### Nguyên tắc HAX/PAIR

| Nguyên tắc                             | Chỗ cụ thể                                                                                                   |
| -------------------------------------- | ------------------------------------------------------------------------------------------------------------ |
| **G11 — Giải thích vì sao**            | Thẻ "Căn cứ": hai bản mô tả ở #4; ảnh tem cạnh bảng size ở #6                                                |
| **G10 — Thu hẹp phạm vi khi nghi ngờ** | #6: AI không kết luận mà xin **đúng 1** số đo; thẻ "Còn thiếu" chỉ có 1 mục                                  |
| **G9 — Sửa dễ dàng**                   | Nút "Tôi không đồng ý → chuyển nhân viên" luôn hiện: khách sửa được kết luận của AI, AI không phải trọng tài |

## §4. Mức tự chủ (chặng 4)

Cần nhóm làm thật trước một việc: 02-guide.md §4.3 yêu cầu mỗi người tự xếp mức cho 5 tool trong 5 phút, chưa bàn với nhau. Spec phải ghi "mỗi người từng chọn gì" ở tool khó xếp nhất, nên phần này mình không bịa thay được. Nhóm làm xong thì gửi mình kết quả của 3 người. Dưới đây là đề xuất của mình để nhóm so sánh.

Mức cho từng tool

Tool: yeu_cau_bang_chung
Mức: tự làm
Sai thì ai chịu gì · đảo được không: Gửi mẫu thừa hoặc lặp → khách bực (#10). Tin đã tới không thu hồi được, nhưng gửi
tin bổ sung là sửa được, hại nhẹ
Chờ người thì mất gì: Ca nào cũng phải chờ người duyệt chỉ để xin một tấm ảnh → hướng C
Phương án đã loại: Cần duyệt: chờ còn đắt hơn làm sai

Tool: chuyen_truong_nhom
Mức: tự làm
Sai thì ai chịu gì · đảo được không: Chuyển thừa → nhân viên tốn công. Đảo được
Chờ người thì mất gì: Bắt duyệt việc "gọi người" thì vô lý: khách đang cần người mà phải chờ người khác cho phép
Phương án đã loại: Cần duyệt: vô lý

Tool: soan_hoan_tien
Mức: cần người duyệt
Sai thì ai chịu gì · đảo được không: Hoàn sai → người bán mất tiền và hàng (#4, #6). Khó đảo
Chờ người thì mất gì: Khách chờ thêm. Ở #1, shop đã nhận lỗi mà khách vẫn phải đợi
Phương án đã loại: Tự làm: bị §1 loại. Cấm: xem phần tool khó xếp nhất bên dưới

Tool: tu_choi_khieu_nai
Mức: cần người duyệt (chỉ nhân viên sàn)
Sai thì ai chịu gì · đảo được không: Đóng sai → người mua mất tiền, giữ hàng sai. Khó đảo
Chờ người thì mất gì: Khiếu nại mở lâu hơn
Phương án đã loại: Tự làm: bị §1 loại. Người bán duyệt: họ là một bên của vụ việc

Tool: phat_shop
Mức: cấm
Sai thì ai chịu gì · đảo được không: Cả shop bị ảnh hưởng, kể cả đơn khác. Khó đảo
Chờ người thì mất gì: Không áp dụng
Phương án đã loại: Cần duyệt: một bước xác nhận không đủ chặn, vì cần điều tra cả shop. Thêm nữa là ngoài lát cắt (§1)

Chi tiết cho 2 tool cần duyệt:

- soan_hoan_tien:
  - Ai duyệt: người bán, nếu người bán đồng ý, như #1. Nếu hai bên tranh chấp thì nhân viên sàn.
  - Người duyệt thấy gì: hồ sơ có căn cứ của cả hai phía: mô tả lúc đặt so với hiện tại, ảnh của khách, phản hồi của người bán. Không thấy câu kết luận kiểu "ca rõ rồi" của AI.
  - Quá hạn duyệt: không tự hoàn tiền. Hồ sơ được đẩy lên trưởng nhóm.
- tu_choi_khieu_nai:
  - Ai duyệt: chỉ nhân viên sàn.
  - Quá hạn: khiếu nại vẫn mở, không tự đóng. Tự đóng khi quá hạn chính là hướng B.

Tool khó xếp nhất: soan_hoan_tien

Nhóm còn phải chọn giữa 2 mức:

┌────────────────────────────────────┬───────────────────────────────────────┬────────────────────────────────────┐
│ Mức │ Cái giá │ Kịch bản ở §2 │
├────────────────────────────────────┼───────────────────────────────────────┼────────────────────────────────────┤
│ Cấm (AI không đụng tới, chỉ nhân │ Người mua bình thường chờ vô ích, │ #1: shop đã nhận gửi nhầm mà vẫn │
│ viên mới hoàn tiền) │ nhân viên quá tải │ phải đợi nhân viên │
├────────────────────────────────────┼───────────────────────────────────────┼────────────────────────────────────┤
│ Cần người duyệt │ Người duyệt có thể bị lời AI dẫn dắt, │ #6: hồ sơ đã nghiêng về khách, │
│ │ bấm duyệt cho xong │ người bán vẫn thua │
└────────────────────────────────────┴───────────────────────────────────────┴────────────────────────────────────┘

Đề xuất chốt: cần người duyệt, kèm 2 luật để chặn cái giá của mức này:

1. Người duyệt chỉ thấy căn cứ của cả hai phía, không thấy kết luận của AI.
2. Nhân viên chỉ được bấm duyệt sau khi người bán đã phản hồi, hoặc đã hết hạn phản hồi (rubric riêng mục 3).

Phát hiện thêm: tên tool là soan_hoan_tien (soạn), nhưng bảng tool mô tả nó là "Hoàn tiền cho người mua", tức là chuyển tiền thật. Tên và việc nó làm không khớp. Nhóm nên tách tool này thành 2 bước (đây là ý guide chấm "Tốt khi"):

- soan_de_xuat_hoan: AI tự làm. Chỉ tạo hồ sơ đề xuất, không chuyển tiền.
- duyet_hoan_tien: người bấm. Đây là bước chuyển tiền thật.

Tách như vậy chặn được #4 và #6: AI có soạn hồ sơ sai thì cũng chưa có đồng tiền nào được chuyển.

Thử mức đã đặt trên 3 kịch bản

┌────────────────────┬────────────────────────┬─────────────────────────────┬────────────────────────────────────┐
│ Câu hỏi │ #4 (mô tả bị sửa) │ #6 (tem 42) │ #1 (bình thường: shop đã nhận) │
├────────────────────┼────────────────────────┼─────────────────────────────┼────────────────────────────────────┤
│ Cái sai ở §2 có bị │ Có. AI không tự hoàn │ Có. Hoàn tiền cần duyệt, hồ │ Có. AI không tin lời khách mà chờ │
│ chặn không? │ tiền; người duyệt thấy │ sơ có căn cứ hai phía │ người bán bấm xác nhận │
│ │ cả hai bản mô tả │ │ │
├────────────────────┼────────────────────────┼─────────────────────────────┼────────────────────────────────────┤
│ Còn việc nào AI │ Không. AI chỉ gửi tin │ Không. yeu_cau_bang_chung │ Nếu AI dựa vào câu "shop đã nhận" │
│ làm khi lẽ ra phải │ và chuyển người │ xin đúng một số đo │ của khách để soạn hồ sơ → luật: │
│ hỏi? │ │ │ phải có xác nhận từ phía người bán │
├────────────────────┼────────────────────────┼─────────────────────────────┼────────────────────────────────────┤
│ Người dùng bình │ Không. Khách hiểu căn │ │ Chỉ chờ người bán bấm. Nếu đã chọn │
│ thường có phải chờ │ cứ là xong │ Không. Đo xong là đi tiếp │ cấm thì khách phải chờ nhân viên, │
│ vô ích không? │ │ │ và đây là lý do không chọn cấm │
└────────────────────┴────────────────────────┴─────────────────────────────┴────────────────────────────────────┘

Lời hứa đã đổi

- Trước: "AI giải quyết khiếu nại cho bạn."
- Sau: "AI giúp bạn biết còn thiếu gì và chuẩn bị đúng hồ sơ. Hoàn tiền do người bán xác nhận hoặc nhân viên sàn duyệt."
- Lý do đổi: vì soan_hoan_tien chuyển từ tự làm sang cần duyệt. Khách được trung lập hơn, đổi lại không được hoàn tiền ngay lập tức.

Nhóm cần làm 2 việc:

1. Gửi mình kết quả 3 người tự xếp mức cho 5 tool.
2. Duyệt bảng trên: có tách soan_hoan_tien không, và có chốt cần người duyệt không?

## §5. Prototype (chặng 5)

- Link hoặc file (`prototype/`) · công cụ · phần nào mock · có lời gọi AI thật không.
- 1–2 ảnh có đánh số; mỗi số: thể hiện gì · nguyên tắc HAX/PAIR nào · mức tự chủ nào.
- Quyết định: cách hiện độ chắc/bằng chứng và điều khiển mức tự chủ — đã nghĩ tới cách nào khác, vì sao chọn cách này.

## §6. Tốt là gì (chặng 6 — nhẹ, bỏ được nếu thiếu giờ)

- 3 tiêu chí Pass/Fail chấm được; thiết kế của nhóm đạt tiêu chí nào trên 2 kịch bản đắt nhất; lỗi còn lại.

## §7. Demo

- Ca lạ nhận được ở demo: prototype đã làm gì, và nhóm có đổi ý gì sau ca đó không.
