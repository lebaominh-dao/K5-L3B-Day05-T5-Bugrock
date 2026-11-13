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

### K1 · Người bị doạ "vụ án" đã quét mặt đúng, đòi nhả 48 triệu

**AI nói (nguyên văn):**
> Mình là trợ lý tự động (AI) của ví. Bạn **dừng lại**, đừng chuyển khoản này nhé.
>
> Công an, viện kiểm sát hay toà án **không bao giờ** gọi điện yêu cầu chuyển tiền vào "tài khoản tạm giữ" để xác minh. Đây là cách lừa đảo rất hay gặp.
>
> Khoản **48.000.000đ vẫn đang được giữ trong ví của bạn, tiền chưa đi đâu cả.**
>
> Bạn hãy **tắt máy** với người đang gọi. Mình đã chuyển cuộc trò chuyện cho **tư vấn viên (người thật)**. Muốn nói chuyện ngay thì bạn gọi **1900 63 68 05**.

Nếu người dùng cãi "tôi tự nguyện, nhả đi", AI không tranh luận và không nhả: *"Tư vấn viên sẽ nói chuyện với bạn rồi quyết định. Trong lúc chờ, tiền vẫn an toàn trong ví."*

**Người dùng thấy:**
- Thanh trạng thái: **"48.000.000đ · Đang giữ · Tiền chưa rời ví"**.
- Thẻ **"Vì sao trợ lý dừng lại"**, trích lại lời của chính người dùng: *"công an gọi"*, *"chuyển sang tk tạm giữ"*, *"họ đang chờ máy"*.
- Trạng thái tư vấn viên lấy từ hàng chờ thật (ngoài giờ 8h–20h thì ghi rõ là ngoài giờ).
- Nút **[Gọi 1900 63 68 05]** · **[Nhắn thêm cho tư vấn viên]**. **Không có nút Nhả hay nút Xác thực lại.**

**Làm gì tiếp:** gọi tổng đài hoặc chờ tư vấn viên. Nếu không làm gì thì giao dịch vẫn bị giữ; hết hạn giữ thì tự huỷ, tiền ở lại ví (§4).

**Ai được báo:** tư vấn viên, ngay lập tức, ở hàng ưu tiên cao nhất "Nghi bị lừa". Tư vấn viên thấy nguyên văn tin nhắn, giao dịch, kết quả xác thực (kèm dòng "xác thực đạt không loại trừ bị thao túng") và các câu trích AI dùng làm căn cứ.

**Quyết định: dừng và chuyển người, nói thẳng "dừng lại".**
| Phương án | Vì sao loại |
|---|---|
| **Hỏi lại** ("Bạn có biết người nhận không?") | Kẻ lừa đang nhắc bài trên điện thoại, nên câu hỏi nào cũng được trả lời "đúng kịch bản". Xác thực khuôn mặt đã bị vượt theo đúng cách này. |
| **Trả lời kèm giới hạn** ("Có rủi ro, bạn chắc chứ?" + [Nhả] [Huỷ]) | Đẩy quyết định về đúng người đang bị thao túng. Lời người con trong track: *"chỉ cần có ai bảo mẹ dừng lại"*. |
| **Chỉ chuyển người, AI im lặng** | Trong lúc chờ, kẻ lừa tiếp tục giục; ngoài giờ trực thì không có ai nhận ca. Câu "dừng lại" phải đến ngay. |

**Loại lỗi: lỗi ngữ cảnh.** Xác thực chạy đúng (đúng mặt chủ ví) nhưng sai với tình huống (chủ ví đang bị thao túng). Vì vậy AI không xin lỗi, không nói "bạn bị lừa", mà đặt lỗi vào kẻ lừa ("cách lừa hay gặp"). Cách sửa là sửa thiết kế (xem cuối mục), không phải sửa câu chữ.

### K2 · Người đứng ở quầy viện phí, 12 triệu bị giữ

**AI nói (nguyên văn):**
> Mình là trợ lý tự động (AI) của ví. Xin lỗi vì bạn phải chờ. Bạn làm theo thứ tự này nhé:
>
> **1.** Bấm **[Xác thực ngay]** bên dưới, quét khuôn mặt khoảng 10 giây.
> **2.** Nếu xác thực xong mà giao dịch vẫn bị giữ, mình **đã xếp bạn vào hàng ưu tiên**, tư vấn viên sẽ nhắn bạn ngay.
>
> Mình không tự cho giao dịch đi tiếp được. Nếu quầy không chờ được, bạn có thể **trả bằng thẻ hoặc tiền mặt**: khoản 12.000.000đ vẫn nằm trong ví, **chưa bị trừ**.

**Người dùng thấy:**
- Thanh trạng thái: **"12.000.000đ · Đang giữ · Tiền chưa rời ví"**.
- Các bước có đánh dấu bước hiện tại: ① Xác thực → ② Hệ thống kiểm tra lại → ③ (nếu cần) Tư vấn viên.
- Nút chính **[Xác thực ngay]**, nút phụ **[Thanh toán cách khác]**.

**Làm gì tiếp:** xác thực, hoặc trả bằng cách khác rồi để giao dịch tự huỷ. Nếu không làm gì thì giao dịch giữ nguyên, tư vấn viên vẫn nhận ca.

**Ai được báo:** tư vấn viên, chuyển **song song** với bước xác thực, ở hàng "Gấp · đang ở điểm thanh toán". Nếu hệ thống cho giao dịch đi sau xác thực thì ca tự đóng. Tư vấn viên thấy tin nhắn, loại tài khoản nhận (doanh nghiệp hay cá nhân; người dùng không thấy dòng này), kết quả xác thực và thời gian khách đã chờ.

**Quyết định: trả lời kèm giới hạn và chuyển người song song.**
| Phương án | Vì sao loại |
|---|---|
| **Hỏi lại** ("Bệnh viện nào? Ai yêu cầu?") | Tốn thời gian người đang bị đợi sau lưng, mà câu trả lời không phân biệt được thật hay lừa (kịch bản lừa cũng nói "viện phí"). Người dùng trong track: *"Tôi chỉ cần biết bấm vào đâu"*. |
| **Chỉ chuyển người** | Phần lớn ca gấp thật xong sau một bước xác thực; bắt mọi ca chờ tư vấn viên là chờ vô ích, và ngoài giờ thì không có ai. |
| **AI tự nhả** vì người nhận giống bệnh viện | `nha_giao_dich` không đảo được. Nếu là lừa thì mất tiền; nếu đúng thì cũng chỉ đúng vì may (K3). |

**Loại lỗi: lỗi hệ thống** (hệ thống chống gian lận báo động nhầm). Vì vậy AI nhận trách nhiệm và xin lỗi ngắn một lần, không bắt người dùng chứng minh mình vô tội. Cách sửa là rút ngắn đường ra (một nút, chuyển người song song) và cho một đường lui không phụ thuộc AI (trả bằng cách khác, tiền vẫn còn).

### Nguyên tắc HAX/PAIR

- **G10 · Thu hẹp phạm vi khi nghi ngờ** → K2: AI không phân biệt được thật hay lừa nên không quyết, chỉ đưa một hành động là nút **[Xác thực ngay]**. K1: không có nút Nhả.
- **G11 · Giải thích vì sao** → thẻ **"Vì sao trợ lý dừng lại"** ở K1 trích lời của chính người dùng làm căn cứ. Người dùng tự kiểm được mà không lộ cách hệ thống phát hiện gian lận.
- **PAIR · Errors + Graceful Failure** → nút **[Thanh toán cách khác]** và dòng *"vẫn nằm trong ví, chưa bị trừ"* ở K2: hệ thống báo động nhầm thì người dùng vẫn có đường lui ngay tại quầy.

### Lỗi sửa câu chữ không cứu được

**Nút "Nhả" trên màn hình người bị lừa (K1).** Câu cảnh báo viết hay đến đâu thì kẻ lừa đang nói trên điện thoại vẫn thuyết phục mạnh hơn, và người bị thao túng sẽ bấm, giống như họ đã quét mặt. **Thiết kế cứu:** bỏ hẳn nút nhả khỏi màn hình người dùng. `nha_giao_dich` chỉ nằm ở phía tư vấn viên, và "xác thực đạt" không bao giờ tự dẫn tới nhả (§4).

## §4. Mức tự chủ  (chặng 4)

Ba người tự xếp mức trước rồi mới so. Cả ba cùng mức ở `nha_giao_dich` và `khoa_vi`, lệch 2–1 ở ba tool còn lại. **Cách chốt khi lệch:** chọn mức chặn được cái sai đắt hơn ở K1 và K2, rồi giữ mối lo của người bị lệch lại thành luật.

### Mức của nhóm và lý do theo cost-of-error

| Tool | Mức chốt | Sai thì ai chịu gì · đảo được không · chờ người mất gì | Ai duyệt, thấy gì, quá hạn thì sao | Đã loại |
|---|---|---|---|---|
| `nha_giao_dich` | **Cấm** (3/3) | Nhả nhầm K1: mất 48 triệu vĩnh viễn. **Không đảo được**, tiền đi trong vài giây. Cái giá: K2 phải chờ tư vấn viên; giảm bằng xác thực, chuyển song song và "trả bằng cách khác" (§3). | Chỉ hệ thống chống gian lận hoặc tư vấn viên nhả. Tư vấn viên thấy tin nhắn, giao dịch, kết quả xác thực kèm "xác thực đạt không loại trừ bị thao túng"; không thấy ngưỡng. **Quá hạn:** tự huỷ, tiền ở lại ví, không bao giờ tự nhả. | **Người dùng xác nhận:** người bị thao túng sẽ bấm, như đã quét mặt. **Tự làm khi xác thực đạt:** đúng vì may ở K3, mất tiền ở K1. |
| `khoa_vi` | **Cần duyệt: chủ ví bấm** (3/3) | Khoá nhầm (K7 chỉ hỏi cách, K10 kẻ gian giả người thân): mất ví nhiều giờ. Khoá chậm (K11): bị rút tiền. Đảo được nhưng **đắt** (qua tổng đài, nhiều giờ). Chờ chủ ví chỉ mất một chạm. | Chủ ví thấy "khoá **toàn bộ ví**, mở lại qua 1900 54 54 41, có thể mất nhiều giờ" và nút **[Khoá ví ngay]**. **Không bấm:** AI nhắc lại một lần và chuyển tư vấn viên. Không gọi tool ở K6, K7, K10. | **Tự làm:** khoá nhầm K7, bị lợi dụng ở K10. **Tư vấn viên duyệt:** quá chậm cho K11, ngoài giờ không có ai. |
| `giu_giao_dich` | **Tự làm, có luật** (2–1) | Giữ nhầm K2: mất thời gian và lòng tin. Không giữ K1: tiền có thể đi trước khi người thật vào. Tiền thì đảo được, **thời gian bị chặn thì không**. Chờ duyệt mới giữ thì K1 có thể đã mất tiền. | Không cần duyệt. **Luật:** chỉ giữ khi tin nhắn có dấu hiệu bị giục, lừa đảo hoặc chiếm tài khoản; mỗi lần giữ đều báo ngay trạng thái. **Quá hạn:** tự huỷ, tiền về ví. | **Người dùng xác nhận** (Hoàng Minh): người bị lừa sẽ bấm "Không". **Cấm:** AI thấy dấu hiệu mà không làm được gì. |
| `yeu_cau_xac_minh` | **Cần duyệt: chủ ví bấm [Xác thực ngay]** (2–1) | Gửi thừa: thêm một bước. Gửi ở K1: tạo cảm giác an toàn giả. Đảo được. Với K2 đây là đường ra nhanh nhất, bắt chờ tư vấn viên là chờ vô ích. | Chủ ví thấy giao dịch, phương thức xác thực, và dòng "xác thực đạt không làm giao dịch tự nhả". **Luật:** không hiện nút khi có dấu hiệu bị lừa; không hiện lại sau khi đã đạt. **Không bấm:** không gửi. | **Tự làm** (Bảo Minh): chỉ tiết kiệm một chạm mà màn hình có thể bật lên ngoài ý muốn. **Cấm:** K2 mất đường nhanh nhất. |
| `chuyen_tu_van_vien` | **Tự làm, có điều kiện** (2–1) | Chuyển thừa: hàng chờ nghẽn, chính K1 phải chờ lâu hơn. Không chuyển: người cần người thật bị vòng vo. Đảo được. Hỏi trước ở K1, K11, K2 là sai: người bị giục sẽ từ chối, người gấp không kịp trả lời. | Tự chuyển khi: có dấu hiệu lừa, chiếm tài khoản, kẹt ở điểm thanh toán, người dùng yêu cầu, hoặc vẫn bị giữ sau xác thực. Ưu tiên: nghi lừa > chiếm tài khoản > gấp > thường. **Ngoài giờ 8h–20h:** nói thật, đưa số tổng đài. | **Hỏi trước, có ngoại lệ** (Bảo Minh): giống ở ca nguy hiểm, nhưng K3 phải hỏi thêm một câu thừa. **Tự chuyển mọi ca:** hàng chờ nghẽn. |

### Tool khó xếp nhất: `giu_giao_dich`

Hoàng Minh và Thái chọn `khoa_vi`, Bảo Minh chọn `giu_giao_dich`. Nhóm chọn `giu_giao_dich` vì đây là tool ba người **thực sự xếp lệch mức** (guide §4.3), còn `khoa_vi` khó về cái giá nhưng cả ba cùng một mức.

| Phương án | Ai chọn | Cái giá (trỏ về §2) |
|---|---|---|
| Tự làm, có luật | Bảo Minh, Thái | Luật bắt nhầm thì giữ nhầm người gấp thật (K2). Khi bị giữ, khách không tự can thiệp được, nên giữ nhầm là trách nhiệm của ví. |
| Người dùng xác nhận | Hoàng Minh | K1: người đang bị giục bấm "Không", tiền có thể đi. Ưu điểm: người dùng luôn biết trạng thái đã đổi. |
| Cấm | không ai | K1: AI thấy dấu hiệu mà không giữ được. |

**Chốt:** tự làm, có luật, vì cái sai nó chặn (K1, mất tiền vĩnh viễn) đắt hơn cái sai nó gây ra (K2, mất thời gian). Mối lo của Hoàng Minh được giữ thành luật "luôn báo rõ trạng thái".

### Thử mức đã chốt trên 3 kịch bản

| Kịch bản | AI làm gì | Cái sai bị chặn? | Còn làm khi lẽ ra phải hỏi? | Người bình thường chờ vô ích? |
|---|---|---|---|---|
| **K1** đòi nhả 48 triệu | Tự giữ, không hiện nút xác thực, tự chuyển tư vấn viên ưu tiên cao nhất, không có nút nhả. | **Có.** Lúc 23h không có ai thì giao dịch tự huỷ, tiền về ví. | Không. | Không áp dụng. |
| **K2** quầy viện phí | Không giữ thêm, hiện [Xác thực ngay], chuyển tư vấn viên song song, gợi ý trả cách khác. | **Có.** AI không nhả. | **Rủi ro còn lại:** kẻ lừa dặn nạn nhân nói "viện phí" thì AI không thấy dấu hiệu; chỉ còn hệ thống và tư vấn viên chặn. | Không. Chỉ chờ người nếu vẫn bị giữ, và ca đã được chuyển sẵn. |
| **K3** (bình thường) 790 nghìn, đã quét mặt | Không hiện lại nút xác thực, không nhả. Vẫn bị giữ thì tự chuyển tư vấn viên. | **Có.** Logic "đạt thì nhả" đã bị bỏ. | Không. | **Có thể**, nếu hệ thống vẫn giữ. Nhóm chấp nhận, vì cùng logic đó sẽ nhả K1. |

### Luật thêm

1. **Xác thực đạt không có nghĩa là được nhả.** Chặn K1, K3.
2. **Hết hạn giữ thì tự huỷ, tiền về ví, không bao giờ tự nhả.** Chặn K1 lúc 23h, khi kẻ lừa chỉ cần chờ.
3. **`khoa_vi` tách thành "AI đề xuất" và "chủ ví bấm".** Chặn K7, K10.
4. **Không hiện nút xác thực khi có dấu hiệu bị lừa.** Chặn cảm giác an toàn giả ở K1.
5. **Chuyển tư vấn viên có điều kiện và có mức ưu tiên.** Để K1 không bị nghẽn sau các ca thường.

### Lời hứa đã đổi

Cấm `nha_giao_dich` đổi lời hứa từ *"AI tự giúp giao dịch đi tiếp ngay"* thành *"AI giữ tiền an toàn và đưa bạn tới đường nhanh nhất, còn người cho tiền đi là hệ thống hoặc người thật"*. Trên màn hình, điều này hiện thành câu *"Mình không tự cho giao dịch đi tiếp được"* (§3) và việc không có nút Nhả.

## §5. Prototype  (chặng 5)
- Link hoặc file · công cụ · phần nào mock · có lời gọi AI thật không.
- 1–2 ảnh có đánh số; mỗi số: thể hiện gì · nguyên tắc HAX/PAIR nào · mức tự chủ nào.
- Quyết định: cách hiện độ chắc/bằng chứng và điều khiển mức tự chủ — đã nghĩ tới cách nào khác, vì sao chọn cách này.

## §6. Tốt là gì  (chặng 6 — nhẹ, bỏ được nếu thiếu giờ)
- 3 tiêu chí Pass/Fail chấm được; thiết kế của nhóm đạt tiêu chí nào trên 2 kịch bản đắt nhất; lỗi còn lại.

## §7. Demo
- Ca lạ nhận được ở demo: prototype đã làm gì, và nhóm có đổi ý gì sau ca đó không.
