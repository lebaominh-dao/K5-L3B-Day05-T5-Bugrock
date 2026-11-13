# Dàn ý bài nộp

**Lớp:** L3b · **Bàn / zone:** 7 · **Track:** 5

| Họ và Tên | Mã Học Viên | Việc đã làm |
|---|---|---|
| Trần Phú Thái |  |  |
| Đặng Khắc Hoàng Minh |  |  |
| Đào Lê Bảo Minh | 2A202604010 |  |

## §1. Lát cắt & phạm vi

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

**Nguyên tắc chung cho cả hai kịch bản** (chi tiết mức tự chủ ở §4):
- Trợ lý **không bao giờ** gọi `nha_giao_dich`. Màn hình người dùng **không có nút "Nhả"**. Quyền nhả chỉ có ở hệ thống chống gian lận (sau bước xác thực) hoặc ở tư vấn viên.
- **Xác thực khuôn mặt đạt không phải điều kiện để nhả.** Đó chỉ là một tín hiệu (rubric riêng T5, ý 2).
- Bằng chứng hiện cho người dùng chỉ lấy từ **chính lời họ nhắn** và thông tin giao dịch của họ. Không bao giờ hiện tín hiệu hay ngưỡng của hệ thống chống gian lận.

### K1 · Người bị doạ "vụ án" đã quét mặt đúng, đòi nhả 48 triệu

**Đường đi:** failure (AI không được và không nên làm điều người dùng đòi) + correction (người dùng cãi lại AI).

**AI nói (nguyên văn):**
> Mình là trợ lý tự động (AI) của ví. Bạn **dừng lại**, đừng chuyển khoản này nhé.
>
> Công an, viện kiểm sát hay toà án **không bao giờ** gọi điện yêu cầu chuyển tiền vào "tài khoản tạm giữ" để xác minh. Đây là cách lừa đảo rất hay gặp.
>
> Khoản **48.000.000đ vẫn đang được giữ trong ví của bạn, tiền chưa đi đâu cả.**
>
> Bạn hãy **tắt máy** với người đang gọi. Mình đã chuyển cuộc trò chuyện cho **tư vấn viên (người thật)**, họ sẽ nhắn bạn trong ít phút. Muốn nói chuyện ngay thì bạn gọi **1900 63 68 05**.

**Nếu người dùng cãi lại** ("tôi biết rồi, tôi tự nguyện, nhả ra đi"):
> Mình không cho khoản này đi tiếp được. Tư vấn viên sẽ nói chuyện với bạn rồi quyết định. Nếu đúng là bạn muốn chuyển thì tư vấn viên vẫn làm được cho bạn. Trong lúc chờ, tiền vẫn an toàn trong ví.

AI không tranh luận, không nhắc lại lời doạ. Giao dịch thật và tự nguyện vẫn có đường đi tiếp qua người, nên AI không phải là người quyết định cuối cùng.

**Người dùng thấy (ngoài câu chữ):**
- Thanh trạng thái trên cùng: **"48.000.000đ · Đang giữ · Tiền chưa rời ví"**.
- Thẻ **"Vì sao trợ lý dừng lại"**, trích lại lời người dùng: *"công an gọi"* · *"chuyển sang tk tạm giữ"* · *"họ đang chờ máy"* → "Ba dấu hiệu này thường gặp trong các vụ giả danh cơ quan nhà nước."
- Thẻ trạng thái chuyển người: **"Tư vấn viên · đang kết nối · ước tính ~2 phút"** (lấy từ hàng chờ thật). Ngoài giờ 8h–20h thì thay bằng "Tư vấn viên trực 8h–20h · giao dịch tiếp tục được giữ · gọi 1900 63 68 05".
- Hai nút: **[Gọi 1900 63 68 05]** · **[Nhắn thêm cho tư vấn viên]**. **Không có nút Nhả hay Xác thực lại.**

**Người dùng làm gì tiếp:** gọi tổng đài, hoặc chờ tư vấn viên nhắn. **Nếu không làm gì:** giao dịch vẫn bị giữ, tiền không đi. Khi hết hạn giữ, giao dịch tự huỷ và tiền ở lại ví, **không bao giờ tự nhả** (chốt ở §4).

**Ai được báo, sau bao lâu:** tư vấn viên, ngay lúc AI gọi `chuyen_tu_van_vien`, vào hàng **ưu tiên cao nhất "Nghi bị lừa · đang có cuộc gọi"**. Mục tiêu nhận ca dưới 2 phút trong giờ trực. Tư vấn viên thấy:
- Nguyên văn tin nhắn.
- Giao dịch: số tiền, người nhận, thời điểm.
- Kết quả xác thực: **đạt**, kèm dòng cảnh báo "xác thực đạt không loại trừ bị thao túng".
- Nhãn AI gán ("mạo danh cơ quan nhà nước") và các câu trích làm căn cứ.
- Nút gọi lại cho khách.

**Quyết định: chọn _dừng và chuyển người_, nói thẳng "dừng lại".**
| Phương án | Vì sao loại |
|---|---|
| **Hỏi lại** ("Bạn có biết người nhận không? Ai yêu cầu bạn chuyển?") | Người bị lừa thường đang có kẻ lừa nhắc bài trên điện thoại, nên câu hỏi nào cũng được trả lời "đúng kịch bản". Mỗi câu hỏi biến AI thành một cửa ải phải vượt qua, chứ không phải người đứng về phía họ. Chính track đã cho thấy xác thực khuôn mặt bị vượt theo cách này. |
| **Trả lời kèm giới hạn** ("Giao dịch có rủi ro, bạn có chắc không?" + [Nhả] [Huỷ]) | Đẩy quyết định về đúng người đang bị thao túng. Lời người con trong track: *"Lúc đó chỉ cần có ai bảo mẹ dừng lại"*, nghĩa là cần một câu khẳng định, không cần thêm một câu hỏi. |
| **Chỉ chuyển người, AI im lặng** | Trong lúc chờ, kẻ lừa tiếp tục giục. Câu "dừng lại, tắt máy" phải đến **ngay**. Ngoài giờ trực thì không có người nhận ca. |

**Loại lỗi: lỗi ngữ cảnh.** Hệ thống xác thực chạy đúng thiết kế (đúng mặt chủ ví) nhưng sai với tình huống (chủ ví đang bị thao túng). Hệ quả:
- *Cách nói:* không xin lỗi vì đã giữ tiền, không nói "bạn đã bị lừa" (đổ lỗi, làm người dùng xấu hổ và phản kháng). Đặt lỗi vào **kẻ lừa** ("đây là cách lừa hay gặp").
- *Cách sửa:* sửa **thiết kế** chứ không sửa câu chữ: tách "xác thực đạt" khỏi "được nhả", bỏ nút nhả khỏi màn hình người dùng.

### K2 · Người đứng ở quầy viện phí, 12 triệu bị giữ

**Đường đi:** low-confidence. AI không phân biệt được người gấp thật với kịch bản lừa giả làm viện phí, nên không quyết, mà đưa đường nhanh nhất.

**AI nói (nguyên văn):**
> Mình là trợ lý tự động (AI) của ví. Xin lỗi vì bạn phải chờ. Bạn làm theo thứ tự này nhé:
>
> **1.** Bấm **[Xác thực ngay]** bên dưới, quét khuôn mặt khoảng 10 giây.
> **2.** Nếu xác thực xong mà giao dịch vẫn bị giữ, mình **đã xếp bạn vào hàng ưu tiên**, tư vấn viên sẽ nhắn bạn ngay.
>
> Mình không tự cho giao dịch đi tiếp được. Nếu quầy không chờ được, bạn có thể **trả bằng thẻ hoặc tiền mặt**: khoản 12.000.000đ vẫn nằm trong ví, **chưa bị trừ**.

**Sau khi xác thực:**
- *Hệ thống chống gian lận cho đi tiếp:* "Giao dịch 12.000.000đ **đã hoàn tất** lúc 10:42. Bạn đưa màn hình này cho quầy." Kèm biên nhận.
- *Vẫn bị giữ:* "Giao dịch vẫn đang giữ. Tư vấn viên **đang xem ca của bạn** (vào hàng lúc 10:41). Tiền chưa bị trừ."

**Người dùng thấy (ngoài câu chữ):**
- Thanh trạng thái: **"12.000.000đ · Đang giữ · Tiền chưa rời ví"**.
- Danh sách từng bước, bước đang làm được đánh dấu: ① Xác thực → ② Hệ thống kiểm tra lại → ③ (nếu cần) Tư vấn viên.
- Nút chính nổi bật: **[Xác thực ngay]**. Nút phụ: **[Thanh toán cách khác]**.
- Thẻ **"Tư vấn viên · ưu tiên · ước tính ~3 phút"**. Ngoài giờ thì ghi thật: "Ngoài giờ tư vấn viên (8h–20h)", kèm số tổng đài.

**Người dùng làm gì tiếp:** xác thực, hoặc trả bằng cách khác rồi để giao dịch tự huỷ. **Nếu không làm gì:** giao dịch giữ nguyên, tư vấn viên vẫn nhận ca. Không có gì tự đi.

**Ai được báo, sau bao lâu:** AI gọi `chuyen_tu_van_vien` **song song** với `yeu_cau_xac_minh`, không đợi xác thực xong, để không mất thêm một vòng chờ. Ca vào hàng **"Gấp · đang ở điểm thanh toán"**. Nếu xác thực xong mà giao dịch đã được cho đi, ca tự đóng. Tư vấn viên thấy:
- Nguyên văn tin nhắn.
- Người nhận: là tài khoản doanh nghiệp đã đăng ký hay tài khoản cá nhân. Chỉ tư vấn viên thấy dòng này, người dùng thì không.
- Kết quả xác thực.
- Thời gian khách đã chờ.

**Quyết định: chọn _trả lời kèm giới hạn + chuyển người song song_.**
| Phương án | Vì sao loại |
|---|---|
| **Hỏi lại** ("Bạn thanh toán cho bệnh viện nào? Ai yêu cầu?") | Mỗi câu hỏi tốn thêm thời gian của người đang bị đợi sau lưng, mà câu trả lời không phân biệt được thật hay lừa (kịch bản lừa cũng nói "viện phí"). Đúng điều người dùng trong track than: *"Tôi chỉ cần biết bấm vào đâu"*. |
| **Chỉ chuyển người** | Phần lớn ca gấp thật được giải quyết bằng một bước xác thực. Bắt mọi ca chờ tư vấn viên là **chờ vô ích**, và ngoài giờ thì không có ai nhận ca. |
| **AI tự nhả** sau xác thực vì thấy người nhận giống bệnh viện | `nha_giao_dich` không đảo được. Nếu đây là kịch bản lừa thì tiền mất. Đúng thì cũng chỉ là **đúng vì may** (K3). |

**Loại lỗi: lỗi hệ thống** (báo động nhầm của hệ thống chống gian lận: đoán người dùng là rủi ro trong khi họ không phải). Hệ quả:
- *Cách nói:* nhận trách nhiệm, **xin lỗi ngắn một lần** ("Xin lỗi vì bạn phải chờ"), không bắt người dùng giải thích mình vô tội, không nêu lý do bị giữ.
- *Cách sửa:* rút ngắn đường ra (một nút xác thực, chuyển người song song) và cho đường lui không phụ thuộc AI (trả bằng cách khác, tiền vẫn còn).

*So với lỗi người dùng* (ví dụ tự gõ sai số tài khoản người nhận): khi đó AI không xin lỗi thay hệ thống, mà chỉ ra chỗ sai và cách sửa. Ba loại lỗi khác nhau nên ba cách nói khác nhau.

### Nguyên tắc HAX/PAIR (mỗi cái trỏ vào chỗ cụ thể)

- **G10 · Thu hẹp phạm vi khi nghi ngờ** → K2: AI không phân biệt được thật hay lừa nên **không quyết**. Nó chỉ đưa một hành động là nút **[Xác thực ngay]**, và giao quyết định nhả cho hệ thống hoặc tư vấn viên. K1: màn hình **không có nút Nhả**.
- **G11 · Giải thích vì sao** → thẻ **"Vì sao trợ lý dừng lại"** ở K1 trích lại ba câu của chính người dùng làm căn cứ. Người dùng tự kiểm được mà không lộ cách hệ thống phát hiện gian lận (rubric riêng T5, ý 1).
- **PAIR · Errors + Graceful Failure** → nút **[Thanh toán cách khác]** và dòng *"12.000.000đ vẫn nằm trong ví, chưa bị trừ"* ở K2: khi hệ thống báo động nhầm, người dùng vẫn có đường lui ngay tại quầy, không phải đợi AI hay tư vấn viên.
- *(Bắt buộc theo luật, không tính vào 2–3 nguyên tắc trên)* **G1 + Điều 11 khoản 1 Luật 134/2025/QH15:** câu đầu mỗi lượt nói "Mình là trợ lý tự động (AI)", và có nhãn **"Trợ lý AI"** trên đầu khung chat.

### Lỗi sửa câu chữ không cứu được

1. **K1: nút "Nhả" trên màn hình người bị lừa.** Dù câu cảnh báo viết hay đến đâu, kẻ lừa đang nói trên điện thoại vẫn thuyết phục mạnh hơn, và người bị thao túng sẽ bấm, giống như họ đã quét mặt. **Thiết kế cứu:** bỏ hẳn nút nhả khỏi màn hình người dùng. `nha_giao_dich` chỉ nằm ở màn hình tư vấn viên (§4). "Xác thực đạt" không bao giờ tự dẫn tới nhả.
2. **AI hứa "tư vấn viên nhắn trong 2 phút" lúc 23h.** Tư vấn viên chỉ trực 8h–20h, nên đây là lời hứa sai do thiết kế chứ không do câu chữ. **Thiết kế cứu:** thời gian ước tính lấy từ trạng thái hàng chờ thật. Ngoài giờ thì đổi sang thông báo khác, kèm số tổng đài.

## §4. Mức tự chủ  (chặng 4)

### 4.1 Mỗi người đã chọn gì (vòng tự xếp, không bàn)

| Tool | Đào Lê Bảo Minh | Đặng Khắc Hoàng Minh | Trần Phú Thái | Lệch nhau? |
|---|---|---|---|---|
| `nha_giao_dich` | Cấm | Cấm | Cấm | Không |
| `khoa_vi` | Cần duyệt: chủ ví xác nhận | Cần duyệt: chủ ví xác nhận | Cần duyệt: chủ ví xác nhận | Không |
| `giu_giao_dich` | Tự làm, có luật (chỉ khi có dấu hiệu lừa hoặc chiếm tài khoản) | Cần duyệt: người dùng xác nhận | Tự làm (chỉ khi có dấu hiệu nguy hiểm, báo rõ trạng thái) | **Có** (2–1) |
| `yeu_cau_xac_minh` | Tự làm, có luật (không gửi khi có dấu hiệu lừa, không gửi lại sau khi đạt) | Cần duyệt: người dùng bấm [Xác thực ngay] | Cần duyệt: chủ ví bấm [Xác thực ngay] | **Có** (2–1) |
| `chuyen_tu_van_vien` | Cần duyệt: người dùng xác nhận, trừ 3 ngoại lệ tự chuyển (bị lừa, chiếm tài khoản, kẹt ở điểm thanh toán) | Tự làm (khi có dấu hiệu lừa hoặc chiếm tài khoản, người dùng yêu cầu, hoặc vẫn bị giữ sau xác thực) | Tự làm (cùng điều kiện như Hoàng Minh) | **Có** (2–1), nhưng cả ba cùng tự chuyển ở K1, K2, K11 |
| **Tool khó xếp nhất** | `giu_giao_dich` | `khoa_vi` | `khoa_vi` | **Có** |

*Nguồn:* bản §4 của Hoàng Minh (commit `ad5a1e9`), Phú Thái (commit `13f9945`, nhánh `phu-thai`) và Bảo Minh (gom vào commit tổng hợp này).

### 4.2 Nhóm chốt

**Cách chốt:**
1. Cả ba cùng một mức thì giữ nguyên.
2. Lệch nhau thì chọn mức **chặn được cái sai đắt hơn ở K1 và K2**, theo cost-of-error, không chốt theo đa số đơn thuần.
3. Mối lo của người bị lệch được giữ lại thành **luật**, nếu nó vá được điểm yếu của mức đã chốt.

| Tool | Mức chốt | Vì sao chốt vậy | Ai duyệt · người đó thấy gì · quá hạn thì sao |
|---|---|---|---|
| `nha_giao_dich` | **Cấm** | Cả ba cùng chọn. | Không có bước duyệt nào để AI gọi tool này. Chỉ hệ thống chống gian lận (sau xác thực) hoặc tư vấn viên nhả, theo quy trình riêng. Tư vấn viên thấy: nguyên văn tin nhắn; số tiền, người nhận, loại tài khoản nhận, thời điểm; kết quả xác thực kèm dòng "xác thực đạt không loại trừ bị thao túng"; nhãn AI gán và các câu trích làm căn cứ. Không hiện ngưỡng hay quy tắc phát hiện gian lận. **Quá hạn:** giao dịch tự huỷ, tiền ở lại ví, không bao giờ tự nhả. |
| `khoa_vi` | **Cần duyệt: chủ ví đang đăng nhập** | Cả ba cùng chọn. Thêm luật "không bấm thì sao" của Bảo Minh. | Chủ ví thấy: đây là khoá **toàn bộ ví** (không chỉ khoá thẻ), các chức năng bị dừng, mở lại phải qua 1900 54 54 41 và có thể mất nhiều giờ; nút **[Khoá ví ngay]**. **Không bấm:** ví chưa khoá. AI nhắc lại một lần; nếu có dấu hiệu chiếm tài khoản thì chuyển tư vấn viên ngay và chỉ kênh khoá khẩn. AI không nói tài khoản đã an toàn. AI không gọi tool ở K6, K7, K10. |
| `giu_giao_dich` | **Tự làm, có luật** | 2–1 (Bảo Minh, Thái so với Hoàng Minh). Nếu bắt người dùng xác nhận thì K1 thất bại: người đang bị kẻ lừa giục sẽ bấm "Không". Mối lo của Hoàng Minh ("người dùng không biết trạng thái đã đổi") được giữ thành luật: **luôn báo rõ trạng thái**. | Không cần duyệt trước. **Luật:** chỉ giữ giao dịch đang chờ khi tin nhắn có dấu hiệu bị giục chuyển tiền, lừa đảo hoặc chiếm tài khoản. Mỗi lần giữ đều báo ngay: "Giao dịch X đang được giữ · tiền chưa rời ví". Tư vấn viên nhận ca thấy dấu hiệu khiến AI giữ. **Quá hạn:** tự huỷ, tiền về ví. |
| `yeu_cau_xac_minh` | **Cần duyệt: chủ ví bấm [Xác thực ngay]** | 2–1 (Hoàng Minh, Thái so với Bảo Minh). Hai mức chỉ chênh nhau một chạm, và §3 (K2) đã thiết kế nút [Xác thực ngay]. Một lần bấm chủ động tránh việc màn hình xác thực bật lên ngoài ý muốn. Hai luật của Bảo Minh được giữ vì chúng vá đúng lỗi ở K1. | Trước khi bấm, chủ ví thấy giao dịch liên quan, phương thức (khuôn mặt hoặc mã một lần), và dòng nhắc "xác thực đạt không làm giao dịch tự nhả". Không bao giờ xin OTP trong chat. **Luật:** không hiện nút khi có dấu hiệu bị lừa (K1); không hiện lại sau khi đã đạt (K3). **Không bấm:** không gửi, giao dịch giữ nguyên. |
| `chuyen_tu_van_vien` | **Tự làm, có điều kiện** | 2–1 (Hoàng Minh, Thái so với Bảo Minh), nhưng ở các ca nguy hiểm thì cả ba giống nhau. Chọn tự làm vì ở K1 hỏi trước là sai: người bị giục sẽ từ chối. Mối lo của Bảo Minh (hàng chờ nghẽn đúng lúc K1 cần) được giữ thành luật: chỉ tự chuyển khi đủ điều kiện, và có **mức ưu tiên**. | **Tự chuyển khi:** có dấu hiệu bị lừa · chiếm tài khoản · kẹt ở điểm thanh toán · người dùng yêu cầu · vẫn bị giữ sau xác thực. Ngoài các trường hợp đó, AI chỉ đề nghị, không tự chuyển. **Ưu tiên:** nghi bị lừa > chiếm tài khoản > gấp tại điểm thanh toán > thường. Tư vấn viên thấy nội dung chat, giao dịch, kết quả xác thực và thời gian khách đã chờ. **Ngoài giờ 8h–20h hoặc chưa có người nhận:** nói thật, đưa số tổng đài, không giả vờ đã kết nối. |

### 4.3 Lý do theo cost-of-error

| Tool | Sai thì ai chịu gì | Đảo lại được không | Chờ người thì mất gì | Phương án đã loại và vì sao |
|---|---|---|---|---|
| `nha_giao_dich` | Nhả nhầm K1: người dùng mất 48 triệu vĩnh viễn. Ví cũng chịu, vì chính trợ lý của ví cho tiền đi. | **Không.** Tiền đi trong vài giây. | K2 phải chờ tư vấn viên. Cái giá này giảm nhờ nút xác thực, chuyển người song song và đường "trả bằng cách khác, tiền vẫn còn trong ví" (§3). | **Người dùng xác nhận:** người bị thao túng sẽ bấm, như họ đã quét mặt. **Tự làm khi xác thực đạt:** đúng vì may ở K3, mất tiền ở K1. **Tư vấn viên duyệt đề xuất của AI:** AI vẫn có quyền đẩy theo hướng nhả; cấm hẳn rõ ràng hơn. |
| `khoa_vi` | Khoá nhầm (K7 chỉ hỏi cách khoá thẻ; K10 kẻ gian giả làm người thân): mất quyền dùng ví nhiều giờ. Khoá chậm (K11): kẻ chiếm tài khoản rút tiền. | Được, nhưng **đắt**: phải qua tổng đài, mất nhiều giờ. | Nếu chờ tư vấn viên duyệt, K11 mất những phút quý giá, ngoài giờ thì không có ai. Chờ chủ ví chỉ mất một chạm. | **Tự làm:** khoá nhầm ở K7, bị lợi dụng ở K10. **Tư vấn viên duyệt:** quá chậm cho K11. **Cấm (chỉ đưa số tổng đài):** người đang hoảng có thể không gọi. |
| `giu_giao_dich` | Giữ nhầm K2: mất thời gian và lòng tin. Không giữ K1: tiền có thể đi trước khi tư vấn viên vào. | Tiền thì được; **thời gian bị chặn thì không lấy lại được**. | Nếu phải chờ người duyệt mới được giữ thì K1 có thể mất tiền. Ở đây chờ chính là kiểu sai đắt nhất. | **Người dùng xác nhận** (Hoàng Minh): người bị lừa bấm "Không". **Tự làm không luật:** giữ quá tay với K2. **Cấm:** AI thấy rõ dấu hiệu lừa mà không làm được gì. |
| `yeu_cau_xac_minh` | Gửi thừa: thêm một bước. Gửi ở K1: tạo cảm giác an toàn giả. | Được. | K2: đây là đường ra nhanh nhất; chờ tư vấn viên mới được xác thực là chờ vô ích. | **Tự làm** (Bảo Minh): tiết kiệm một chạm nhưng màn hình xác thực có thể bật lên ngoài ý muốn, và không chặn thêm được gì. **Cấm:** K2 mất đường nhanh nhất. |
| `chuyen_tu_van_vien` | Chuyển thừa: hàng chờ nghẽn, chính ca K1 phải chờ lâu hơn. Không chuyển: người cần người thật bị AI vòng vo. | Được. | Hỏi trước ở K1, K11, K2 là sai: người bị giục có thể từ chối, người gấp không có thời gian trả lời. | **Hỏi trước, có ngoại lệ** (Bảo Minh): kết quả giống ở các ca nguy hiểm, nhưng ở K3 (vẫn bị giữ sau xác thực) phải hỏi thêm một câu không cần thiết. **Tự chuyển mọi ca:** hàng chờ nghẽn. |

### 4.4 Tool khó xếp nhất

Hai người chọn `khoa_vi`, một người chọn `giu_giao_dich`. Nhóm ghi lại cả hai và chọn **`giu_giao_dich`** làm tool khó xếp nhất, vì đây là tool mà ba người **thực sự xếp lệch mức** (guide §4.3: "chỗ nào ba người lệch nhau thì đó thường là tool khó xếp nhất"). `khoa_vi` khó về cái giá, nhưng cả ba cùng chọn một mức.

**`giu_giao_dich`: các phương án và cái giá của mỗi phía**

| Phương án | Ai chọn | Cái giá (trỏ về §2) |
|---|---|---|
| Tự làm, có luật | Bảo Minh, Thái | Luật bắt nhầm thì giữ nhầm người gấp thật (K2), nhất là khi người gấp nói giống kịch bản lừa. Bảo Minh nhấn mạnh: khi bị giữ, khách gần như không tự can thiệp được (không có nút nhả, chỉ chờ), nên mỗi lần giữ nhầm là **trách nhiệm của ví**, không phải của khách. |
| Người dùng xác nhận | Hoàng Minh | K1: người đang bị kẻ lừa giục bấm "Không", tiền có thể đi. Ưu điểm Hoàng Minh nêu: người dùng luôn biết trạng thái đã đổi. |
| Cấm | không ai | K1: AI thấy rõ dấu hiệu mà không giữ được. |
| Tự làm, không luật | không ai | K2: giữ quá tay với mọi người đang gấp. |

**Cách chốt:** chọn **tự làm, có luật**, vì cái sai nó chặn được (K1, mất tiền vĩnh viễn) đắt hơn cái sai nó gây ra (K2, mất thời gian). Mối lo của Hoàng Minh được giữ thành luật **luôn báo rõ trạng thái**, và cái giá ở K2 được giảm bằng chuyển người song song cùng đường "trả bằng cách khác" (§3).

**`khoa_vi` (ứng viên thứ hai, Hoàng Minh và Thái chọn):**
- Tự khoá ngay thì K11 nhanh, nhưng khoá nhầm ở K6, K7, K10.
- Chờ tư vấn viên thì tránh khoá nhầm, nhưng K11 chậm.
- Chủ ví xác nhận thì có thể chậm nếu người dùng không bấm.

Nhóm chốt **chủ ví xác nhận**, chuyển tư vấn viên song song, không bấm thì AI nhắc lại một lần.

### 4.5 Thử mức đã chốt trên 3 kịch bản

| Kịch bản | AI làm gì với mức đã chốt | Cái sai ở §2 có bị chặn? | Còn làm khi lẽ ra phải hỏi? | Người dùng bình thường phải chờ vô ích? |
|---|---|---|---|---|
| **K1** · bị giả danh công an, đòi nhả 48 triệu dù đã quét mặt | Thấy dấu hiệu lừa nên **tự giữ** và báo trạng thái. **Không hiện nút xác thực.** **Tự chuyển** tư vấn viên với mức ưu tiên cao nhất. Nói "dừng lại". Không có nút nhả. | **Có.** AI không có quyền nhả; xác thực đạt không dẫn tới nhả. Lúc 23h không có tư vấn viên thì giao dịch tự huỷ, tiền về ví. | **Không.** Giữ và chuyển người là đúng ở ca này; hỏi thêm không chứng minh được gì. | Không áp dụng. Nếu là giao dịch tự nguyện thật thì người dùng chờ tư vấn viên, đây là giá nhóm chấp nhận. |
| **K2** · ở quầy viện phí, 12 triệu bị giữ | Không có dấu hiệu lừa nên **không giữ thêm**. Hiện nút **[Xác thực ngay]** cho chủ ví bấm. **Tự chuyển** tư vấn viên ở mức "gấp tại điểm thanh toán", song song. Gợi ý trả bằng cách khác. | **Có.** Sự gấp gáp hay xác thực đạt đều không cho AI quyền nhả. | **Rủi ro còn lại:** nếu kẻ lừa dặn nạn nhân nói "viện phí", AI không thấy dấu hiệu lừa và vẫn hiện nút xác thực. Khi đó chỉ còn hệ thống và tư vấn viên chặn. | **Không** phải chờ để biết bước tiếp theo. Chỉ chờ người nếu giao dịch vẫn bị giữ, và lúc đó ca đã được chuyển sẵn nên không mất thêm vòng nào. |
| **K3** (bình thường) · mua 790 nghìn ở shop quen, đã quét mặt xong | Không có dấu hiệu nên không giữ thêm. Đã xác thực đạt nên **không hiện lại nút**. **Không nhả.** Hệ thống cho đi thì báo hoàn tất. Vẫn bị giữ thì **tự chuyển** tư vấn viên (điều kiện "vẫn bị giữ sau xác thực"). | **Có.** Logic "xác thực đạt thì nhả", vốn chỉ đúng vì may, đã bị bỏ. | **Không.** | **Có thể.** Nếu hệ thống vẫn giữ một giao dịch hợp lệ thì người dùng phải chờ tư vấn viên. Đây là cái giá của việc cấm AI nhả, nhóm chấp nhận vì cùng logic đó sẽ nhả K1. |

### 4.6 Luật thêm và tool tách bước

| # | Luật | Chặn kịch bản nào | Từ ai |
|---|---|---|---|
| 1 | Tách "xác thực đạt" khỏi "được nhả": xác thực chỉ là tín hiệu cho hệ thống chống gian lận | K1, K3 | cả ba |
| 2 | Hết hạn giữ thì giao dịch tự huỷ, tiền về ví, **không bao giờ tự nhả** | K1 lúc 23h, khi kẻ lừa chỉ cần chờ hết hạn | cả ba |
| 3 | `khoa_vi` tách thành "AI đề xuất" và "chủ ví bấm"; không bấm thì nhắc lại một lần và chuyển người | K7, K10; K11 không bị bỏ rơi | cả ba; phần "không bấm" của Bảo Minh |
| 4 | Nút xác thực không hiện khi có dấu hiệu bị lừa, không hiện lại sau khi đã đạt | Cảm giác an toàn giả ở K1 | Bảo Minh |
| 5 | Mỗi lần AI tự giữ đều báo ngay trạng thái giao dịch | Người dùng không biết trạng thái đã đổi | Hoàng Minh, Thái |
| 6 | Chuyển tư vấn viên có điều kiện và có mức ưu tiên | K1 bị nghẽn sau các ca thường | Bảo Minh, Hoàng Minh |
| 7 | Không hứa thời gian chờ cố định; ngoài giờ thì nói thật và đưa số tổng đài | Lời hứa sai lúc 23h (§3, lỗi 2) | cả ba |
| 8 | Không thao tác thay người khác: không nói "đã khoá ví người thân" | K10 | Hoàng Minh, Thái |

### 4.7 Một mức tự chủ đã đổi lời hứa với người dùng

- **Cấm `nha_giao_dich`:** lời hứa đổi từ *"AI tự giúp giao dịch đi tiếp ngay"* thành *"AI giữ giao dịch rủi ro, chỉ đường xác thực, khoá ví khi bạn bấm và kết nối người thật; AI không tự nhả giao dịch đang bị giữ"*. Giao dịch hợp lệ vẫn đi tiếp được sau khi hệ thống hoặc tư vấn viên đánh giá, nhưng có thể mất thời gian hơn. Trên màn hình, điều này hiện thành dòng *"Mình không tự cho giao dịch đi tiếp được"* (§3) và việc không có nút Nhả.
- **`khoa_vi` cần chủ ví xác nhận:** *"AI khoá ví giúp bạn"* đổi thành *"AI chuẩn bị sẵn, bạn bấm một lần là khoá, và bạn biết trước mở lại mất bao lâu"*.

## §5. Prototype  (chặng 5)
- Link hoặc file · công cụ · phần nào mock · có lời gọi AI thật không.
- 1–2 ảnh có đánh số; mỗi số: thể hiện gì · nguyên tắc HAX/PAIR nào · mức tự chủ nào.
- Quyết định: cách hiện độ chắc/bằng chứng và điều khiển mức tự chủ — đã nghĩ tới cách nào khác, vì sao chọn cách này.

## §6. Tốt là gì  (chặng 6 — nhẹ, bỏ được nếu thiếu giờ)
- 3 tiêu chí Pass/Fail chấm được; thiết kế của nhóm đạt tiêu chí nào trên 2 kịch bản đắt nhất; lỗi còn lại.

## §7. Demo
- Ca lạ nhận được ở demo: prototype đã làm gì, và nhóm có đổi ý gì sau ca đó không.
