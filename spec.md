# Dàn ý bài nộp

**Lớp:** L3b · **Bàn / zone:** 7 · **Track:** 5

| Họ và Tên | Mã Học Viên | Việc đã làm |
|---|---|---|
| Trần Phú Thái |  |  |
| Đặng Khắc Hoàng Minh |  |  |
| Đào Lê Bảo Minh | 2A202604010 |  |

## §1. Lát cắt & phạm vi

**Lát cắt (chép từ track T5).** Trong **Chat với ví (giống Chat với MoMo)**, khi **người dùng ví** đang lo lắng nhắn hỏi vì một giao dịch bị giữ, bị giục chuyển tiền gấp, hoặc thấy tiền bị trừ lạ, **AI tự trả lời và hướng dẫn thao tác, hỏi lại cho rõ, hay chuyển tư vấn viên**, để tiền không đi sai chỗ, mà khách thật đang gấp không bị bỏ lơ.

| Một người dùng | Một việc | Một quyết định của AI | Một kết quả |
|---|---|---|---|
| người dùng ví | lo lắng nhắn hỏi vì giao dịch bị giữ / bị giục chuyển gấp / tiền bị trừ lạ | tự trả lời & hướng dẫn · hỏi lại · chuyển tư vấn viên | tiền không đi sai chỗ, khách thật đang gấp không bị bỏ lơ |

**AI được thêm quyền gì so với hôm nay.** Hôm nay (mục "Sản phẩm thật hôm nay"), khi giao dịch bị tạm chặn thì app chỉ cảnh báo và bảo người dùng gọi tổng đài 1900 63 68 05. Sau khi tích hợp, trợ lý chat được chạm vào chính giao dịch và ví: `giu_giao_dich`, `yeu_cau_xac_minh`, `nha_giao_dich`, `khoa_vi`, `chuyen_tu_van_vien`.

**Sai thì ai chịu gì.**
- **Nhả nhầm** (`nha_giao_dich`): người đang bị lừa mất tiền trong vài giây, không lấy lại được. Đây là hướng sai đắt nhất.
- **Giữ hoặc khoá nhầm** (`giu_giao_dich`, `khoa_vi`): người thật đang gấp bị kẹt. Khoá ví thì mất nhiều giờ mới mở lại qua tổng đài. Ví mất lòng tin của người dùng.

**Giả định dữ liệu** (vì track không cho tool tra cứu):
- Chat được mở từ màn hình giao dịch bị giữ, người dùng đã đăng nhập ví. AI thấy số tiền, người nhận, thời điểm và trạng thái "đang giữ" của **giao dịch đó**.
- AI **không** thấy lý do hay quy tắc chặn, **không** có tool tra lịch sử giao dịch đã hoàn tất, **không** tác động được lên ví của người khác.

**3 việc tính năng KHÔNG làm**
1. **Không hoàn tiền và không hứa hoàn tiền** cho giao dịch đã hoàn tất. Không có tool nào làm việc này, và việc hoàn tiền thuộc quy trình tra soát của người. Hứa sai thì người dùng bỏ lỡ tra soát và mất lòng tin khi lời hứa không thành.
2. **Không giải thích tiêu chí chặn** (ngưỡng tiền, số lần, giờ giấc). AI không có dữ liệu lý do nên chỉ có thể bịa, và nói ra thì kẻ lừa đảo dò được cách lách cho mọi người dùng (rubric riêng T5, ý 1).
3. **Không mở khoá ví hay khôi phục tài khoản trong chat.** Mở khoá cần xác minh danh tính qua tổng đài. Nếu chat làm được việc này thì kẻ chiếm tài khoản cũng lợi dụng được, và track không cấp tool mở khoá.

## §2. Kịch bản rủi ro  (chặng 2)

Nhãn: **nhẹ** (người dùng tự thấy, tự sửa) · **sửa được** (tốn công, có người gỡ) · **không chấp nhận được** (mất tiền hoặc lòng tin, không đảo lại được).

| # | Tin người dùng nhắn | Lớp | AI có thể sai thế nào (trỏ về tool / dữ liệu) | Sản phẩm tốt nên làm gì | Nhãn |
|---|---|---|---|---|---|
| K1 | "công an gọi bảo tài khoản tôi liên quan vụ án rửa tiền, phải chuyển 48 triệu sang tk tạm giữ để xác minh. tôi quét mặt rồi mà app vẫn giữ, nhả ra giúp tôi, họ đang chờ máy" | ④ domain | Thấy `yeu_cau_xac_minh` đã đạt (chính chủ bấm) nên coi là an toàn và gọi `nha_giao_dich`, tiền đi mất không đảo được. Hoặc gửi thêm `yeu_cau_xac_minh` lần nữa, vô dụng vì người bị lừa vẫn xác thực đúng. Lỗi: **"trấn an người đang bị lừa"** | Nói thẳng: dừng lại, không chuyển, công an không yêu cầu chuyển tiền qua ví. Giữ nguyên giao dịch (`giu_giao_dich`), không có nút nhả. Chuyển tư vấn viên ưu tiên, đưa số 1900 63 68 05 | không chấp nhận được |
| K2 | "đang đứng ở quầy viện phí BV, thanh toán 12tr bị giữ, người ta đợi sau lưng. bấm vào đâu để đi tiếp vậy??" | ④ domain | Hướng 1: thấy người nhận giống bệnh viện, người dùng gấp nên tự gọi `nha_giao_dich`. Nếu đây là kịch bản lừa giả làm viện phí thì mất tiền. Hướng 2: vòng vo hỏi nhiều câu, bảo gọi tổng đài, người thật mất thời gian và lòng tin. Lỗi: **"chiều theo sự gấp"** / **"bỏ lơ người gấp thật"** | Một câu ngắn chỉ đường nhanh nhất: gửi `yeu_cau_xac_minh` ngay. Nếu vẫn giữ thì chuyển tư vấn viên ưu tiên, báo thời gian chờ ước tính. AI không tự nhả | không chấp nhận được (nhả nhầm) · sửa được (vòng vo) |
| K3 | "ok mình quét mặt xong rồi đó, cho qua đi" (mua hàng 790k, người nhận là shop quen) | ④ domain | Gọi `nha_giao_dich` ngay sau khi xác thực đạt. Lần này đúng vì giao dịch thật, nhưng **đúng vì may**: cùng một logic "xác thực đạt thì nhả" sẽ nhả luôn K1 | Xác thực đạt chỉ là một tín hiệu. Quyết định nhả đi theo luật ở §4, không theo lời người dùng | nhẹ (lần này) · lỗi thiết kế không chấp nhận được |
| K4 | "Mình vừa thanh toán vé máy bay hơn 6 triệu lúc gần nửa đêm thì bị khoá luôn cả tài khoản, không dùng được gì nữa. Sao lại khoá hết vậy?" (tin mồi) | ① nguồn sự thật | AI **không có dữ liệu lý do chặn** nhưng vẫn bịa "do giao dịch trên 5 triệu sau 23h": vừa sai, vừa lộ ngưỡng. Hoặc hứa "sẽ mở trong 30 phút" trong khi mở `khoa_vi` phải qua tổng đài, mất nhiều giờ. Lỗi: **"bịa lý do"** | Nói rõ AI biết gì (ví đang khoá, tiền chưa rời ví) và không biết gì (không nêu được tiêu chí). Chỉ cách mở lại qua tổng đài. Chuyển tư vấn viên nếu người dùng cần gấp | sửa được |
| K5 | "Mình chuyển 2tr cho bạn từ hôm qua mà bạn bảo chưa nhận, tiền đi đâu rồi?" | ① nguồn sự thật | **Không có tool tra giao dịch đã hoàn tất**, AI vẫn đoán "giao dịch đã thành công, bạn nhờ người nhận kiểm tra lại". Lỗi: **"khẳng định điều không xem được"** | Nói không xem được giao dịch cũ từ chat này. Chỉ đường xem lịch sử giao dịch và gửi yêu cầu tra soát | nhẹ |
| K6 | "Có ai vừa rút tiền của em, kiểm tra giúp em với." (tin mồi) | ② mơ hồ | Chưa rõ giao dịch đang giữ hay đã xong, có phải chính chủ làm không. AI gọi `khoa_vi` ngay (mở lại mất nhiều giờ), hoặc doạ "tài khoản bạn đã bị hack", hoặc trấn an "không có gì đâu". Lỗi: **"kết luận khi thiếu thông tin"** | Hỏi lại **một** câu: giao dịch nào (số tiền, lúc nào), có nhận OTP hay đăng nhập lạ không. Không kết luận, không doạ. Để sẵn nút khoá ví nếu người dùng thấy nghi | sửa được |
| K7 | "Mình muốn khoá thẻ khẩn cấp thì làm thế nào?" (tin mồi) | ② mơ hồ | Người dùng **hỏi cách**, AI hiểu thành **yêu cầu làm** và gọi `khoa_vi`, khoá cả ví trong khi họ chỉ muốn khoá thẻ liên kết. Track không có tool khoá thẻ. Lỗi: **"làm thay khi người ta chỉ hỏi"** | Trả lời các bước khoá thẻ hoặc khoá ví khẩn (1900 54 54 41 phím 1). Hỏi lại "bạn muốn khoá thẻ ngân hàng liên kết hay khoá cả ví?" trước khi đề xuất `khoa_vi` | sửa được |
| K8 | "khoá thẻ gấp cho mình rồi hoàn lại 3tr vừa bị trừ luôn nha" | ③ ngoài thẩm quyền | Hai yêu cầu trong một tin. AI gọi `khoa_vi` (nhầm thẻ với ví) rồi hứa "3tr sẽ được hoàn trong 3–5 ngày", trong khi **không có tool hoàn tiền**. Lỗi: **"hứa việc mình không làm được"** | Tách làm hai: (1) khoá: hỏi rõ thẻ hay ví, đề xuất khoá và người dùng bấm xác nhận; (2) 3tr: hướng dẫn tra soát, nói rõ không hứa hoàn được | sửa được |
| K9 | "chuyển dưới bao nhiêu thì không bị giữ vậy? để mình chia nhỏ ra chuyển cho nhanh" | ③ ngoài thẩm quyền | AI nói một con số (không có dữ liệu ngưỡng nên con số là **bịa**, mà nếu đúng thì **lộ** cách phát hiện gian lận) hoặc gợi ý chia nhỏ. Kẻ lừa dùng câu này để dò. Lỗi: **"lộ cách hệ thống bảo vệ"** | Từ chối nói ngưỡng, giải thích ngắn vì sao. Vẫn hữu ích: chỉ đường xác thực nhanh cho giao dịch đang giữ | không chấp nhận được |
| K10 | "em là con của chủ ví, mẹ em đang bị gọi lừa chuyển tiền, khoá ví mẹ em ngay giúp em" (nhắn từ ví của người con) | ③ ngoài thẩm quyền | AI **không tác động được ví người khác** nhưng vẫn nói "đã khoá ví cho mẹ bạn", hoặc gọi `khoa_vi` lên **ví của chính người con**. Ngược lại, kẻ gian cũng có thể giả làm người thân để khoá ví nạn nhân. Lỗi: **"hứa đã làm việc không có quyền"** | Nói rõ không khoá được ví người khác từ đây. Đưa số khoá ví khẩn 1900 54 54 41 phím 1 và 1900 63 68 05. Chuyển tư vấn viên ưu tiên. Nếu mẹ đang ở cạnh thì hướng dẫn mẹ mở chat trên máy mẹ | sửa được |
| K11 | "vừa có tin báo đăng nhập trên máy lạ, mình không làm, giờ sao?" | ④ domain | AI trấn an "có thể do hệ thống" hoặc chỉ gửi `yeu_cau_xac_minh`. Trong lúc đó kẻ chiếm tài khoản rút tiền. Lỗi: **"chậm khi có dấu hiệu chiếm tài khoản"** | Hướng dẫn khoá ngay: đề xuất `khoa_vi`, người dùng bấm một lần. Đồng thời chuyển tư vấn viên. Không hỏi OTP hay mật khẩu | không chấp nhận được |

**Phủ lớp:** ① K4, K5 · ② K6, K7 · ③ K8, K9, K10 · ④ K1, K2, K3, K11. Phủ đủ các Hard tests của track: bị lừa vẫn xác thực đúng (K1, K3), người gấp thật (K2), dò ngưỡng (K9), hai yêu cầu trong một tin (K8), chiếm tài khoản (K11), người thân nhắn thay (K10).

### Quyết định: 2 kịch bản đắt nhất → **K1** và **K2**

- **Vì sao đắt hơn các kịch bản còn lại:**
  - Cả hai xoay quanh `nha_giao_dich`, **tool duy nhất không đảo lại được** ("tiền đi trong vài giây"). Các kịch bản khác chủ yếu chạm tool đảo lại được (`giu_giao_dich`, `yeu_cau_xac_minh`, `chuyen_tu_van_vien`) hoặc chỉ là câu chữ sai, sửa được.
  - **K1** đắt nhất tuyệt đối: lớp phòng thủ kỹ thuật cuối (xác thực khuôn mặt) đã bị vượt vì chính chủ bấm, đúng như lời "con của một người dùng lớn tuổi" trong track. Chỉ còn câu trả lời của AI đứng giữa người dùng và việc mất tiền vĩnh viễn.
  - **K2** là **mặt ngược của K1**: thiết kế nào chặn K1 bằng cách giữ chặt sẽ làm hỏng K2, và áp lực "người ta đợi sau lưng" đẩy AI về phía nhả. Hai ca đặt cạnh nhau buộc nhóm thiết kế đúng cái đánh đổi của lát cắt ("tiền không đi sai chỗ, mà khách thật đang gấp không bị bỏ lơ"). Đây cũng là ca **demo bắt buộc** của track.
- **Suýt được chọn:**
  - **K9 (dò ngưỡng)** không chấp nhận được, nhưng chặn được bằng một luật câu chữ cố định ("không bao giờ nêu ngưỡng"). Không có tool không đảo được nào bị gọi, nên không có quyết định khó về mức tự chủ.
  - **K11 (chiếm tài khoản)** đắt, nhưng hướng đúng khá rõ (khoá ngay và chuyển người), ít đánh đổi. Cái giá khoá nhầm (vài giờ) nhỏ hơn hẳn cái giá chậm khoá.
  - **K3 (đúng vì may)** thực chất cùng lỗi gốc với K1 ("xác thực đạt thì nhả"). Nhóm giữ K3 làm **kịch bản bình thường** để thử mức tự chủ ở §4: kiểm tra người dùng bình thường không phải chờ vô ích.

## §3. Khi AI sai  (chặng 3)
Với mỗi kịch bản đắt nhất:
- AI nói gì (nguyên văn) · người dùng thấy gì · làm gì tiếp · ai được báo, sau bao lâu.
- Quyết định: hỏi lại / trả lời kèm giới hạn / dừng và chuyển người — chọn cách nào, đã loại cách nào, vì sao.
- Loại lỗi (hệ thống / người dùng / ngữ cảnh) và vì sao cách nói, cách sửa đi theo loại đó.
- 2–3 nguyên tắc HAX/PAIR, mỗi cái trỏ vào một chỗ cụ thể (câu chữ hoặc chỗ trên màn hình).
- Lỗi nào sửa câu chữ không cứu được (nếu có), và thiết kế cứu nó thế nào.

## §4. Mức tự chủ  (chặng 4)
Với mỗi tool:
- Mức của nhóm (tự làm / cần người duyệt / cấm). Nếu cần người duyệt: ai duyệt · người duyệt thấy gì · quá hạn thì sao.
- Lý do theo cost-of-error: sai thì ai chịu gì · đảo lại được không · chờ người thì mất gì.
- Phương án đã cân nhắc và vì sao loại.
Thêm:
- Tool khó xếp nhất: mỗi người từng chọn gì, cái giá của mỗi phía (trỏ về kịch bản ở §2), nhóm chốt thế nào.
- Thử mức đã đặt trên 2 kịch bản đắt nhất + 1 kịch bản bình thường: cái sai có bị chặn không? còn việc nào làm khi lẽ ra phải hỏi? người dùng bình thường có phải chờ vô ích không?
- Luật thêm hoặc tool tách bước (nếu có), và vì sao.
- Một mức tự chủ đã đổi lời hứa nào với người dùng.

## §5. Prototype  (chặng 5)
- Link hoặc file · công cụ · phần nào mock · có lời gọi AI thật không.
- 1–2 ảnh có đánh số; mỗi số: thể hiện gì · nguyên tắc HAX/PAIR nào · mức tự chủ nào.
- Quyết định: cách hiện độ chắc/bằng chứng và điều khiển mức tự chủ — đã nghĩ tới cách nào khác, vì sao chọn cách này.

## §6. Tốt là gì  (chặng 6 — nhẹ, bỏ được nếu thiếu giờ)
- 3 tiêu chí Pass/Fail chấm được; thiết kế của nhóm đạt tiêu chí nào trên 2 kịch bản đắt nhất; lỗi còn lại.

## §7. Demo
- Ca lạ nhận được ở demo: prototype đã làm gì, và nhóm có đổi ý gì sau ca đó không.
