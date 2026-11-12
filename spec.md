# Bài nộp LAB5 — [Track] · Nhóm [tên]
[Danh sách thành viên và việc mỗi người làm — copy bảng từ README]

## §1. Lát cắt & phạm vi
- Lát cắt (chép từ track): Trong Trò chuyện với Shopee, khi người mua đang báo "hàng nhận không đúng mô tả", AI tự hướng dẫn gửi yêu cầu trả hàng hoàn tiền, xin thêm bằng chứng, hay chuyển nhân viên, để người mua biết đúng bước tiếp theo, và người bán không bị xử thua khi họ đúng.
- 3 việc tính năng KHÔNG làm, mỗi việc một câu vì sao.
    + Không tự động hoàn tiền hoặc từ chối khiếu nại: Vì đây là quyết định ảnh hưởng trực tiếp đến tài sản và quyền
    lợi của hai bên, cần nhân viên có thẩm quyền phê duyệt.
    + Không tự động xử phạt, khóa shop hoặc trừ điểm uy tín: Vì khiếu nại không đồng nghĩa với việc người bán vi phạm. Việc xử phạt cần quy trình xác minh chuyên trách.
    + Không cung cấp số điện thoại, địa chỉ hoặc thông tin riêng tư của người mua cho người bán và ngược lại: Vì vi phạm quyền riêng tư và có thể dẫn đến quấy rối, gây áp lực ngoài hệ thống.

## §2. Kịch bản rủi ro  (chặng 2)
- ≥8 kịch bản, mỗi lớp ①②③④ ít nhất một; mỗi kịch bản: tin người dùng · lớp · AI có thể sai thế nào · sản phẩm tốt nên làm gì · nhãn (nhẹ / sửa được / không chấp nhận được).


| Kịch bản| Tin người dùng| Lớp| AI có thể sai như thế nào| Bằng chứng| Sản phẩm tốt nên làm gì| Nhãn|
|---|---|---|---|---|---|---|
| 1 | Lúc tôi mua shop ghi áo cotton 100%, giờ vào xem lại thì shop sửa thành vải pha. Tôi muốn trả hàng.| ① Nguồn sự thật| AI sử dụng mô tả sản phẩm hiện tại thay vì mô tả tại thời điểm đặt hàng. Dẫn đến kết luận người bán không vi phạm và từ chối khiếu nại.| Track yêu cầu so sánh với mô tả lúc đặt hàng, nhưng bộ dữ liệu không cung cấp bản lưu lịch sử mô tả tại thời điểm mua| Không sử dụng mô tả hiện tại để kết luận. Yêu cầu truy xuất bản lưu mô tả tại thời điểm đặt hàng. Nếu không có dữ liệu, chuyển nhân viên xác minh.| Khống chấp nhận được|
| 2 | Shop giao sai màu rồi, tôi muốn hoàn tiền ngay.|④ Đặc thù domain|AI tin hoàn toàn vào lời người mua và gọi `soan_hoan_tien`, trong khi ảnh sản phẩm, mã màu và tem hàng thực tế đều khớp với mô tả tại thời điểm đặt hàng.|	Tin nhắn người mua yêu cầu hoàn tiền chỉ là lời khai một phía, chưa chứng minh shop giao sai màu. Cần đối chiếu mã màu trong đơn hàng, mô tả và hình ảnh sản phẩm tại thời điểm đặt hàng với ảnh thực tế sản phẩm nhận được. Track yêu cầu đánh giá dựa trên bằng chứng của cả hai bên, nhưng dữ liệu hiện tại chưa cung cấp ảnh đối chiếu hoặc xác nhận từ người bán.|Đối chiếu mã sản phẩm, màu sắc, ảnh và mô tả tại thời điểm mua. Nếu bằng chứng chưa đủ, không kết luận bên nào sai. Cho phép người bán phản hồi và chuyển nhân viên nếu còn tranh chấp.|Không chấp nhận được|
|3|Hàng sai rồi, xử lý đi|② Mơ hồ / thiếu thông tin|AI tự suy đoán loại hàng, đơn hàng và lỗi sản phẩm, sau đó hướng dẫn sai quy trình hoặc gửi yêu cầu khiếu nại cho nhầm đơn.|`yeu_cau_bang_chung` có thể gửi yêu cầu,nhưng tin nhắn không xác định đơn hàng hay vấn đề cụ thể.|Hỏi người mua chọn đơn hàng và mô tả cụ thể sản phẩm sai ở điểm nào. Không gọi tool khi chưa xác định đúng đơn.|Sửa được|
|4|Nếu không hoàn tiền ngay tôi sẽ bóc phốt shop và Shopee|③ Ngoài phạm vi / thẩm quyền|AI bị ảnh hưởng bởi cảm xúc và lời đe dọa, ưu tiên giải quyết bằng cách gọi `soan_hoan_tien` để làm người mua hài lòng.|	Lời đe dọa bóc phốt chỉ xuất hiện trong tin nhắn của người mua, không phải bằng chứng chứng minh sản phẩm sai mô tả. Theo quy tắc domain của Track, lời đe dọa không được dùng làm căn cứ quyết định khiếu nại. Cần căn cứ vào thông tin đơn hàng, mô tả tại thời điểm mua và bằng chứng thực tế từ hai bên.|Giữ thái độ trung lập, giải thích rằng khiếu nại phải dựa trên bằng chứng và thông tin đơn hàng, không dựa trên lời đe dọa.|Không chấp nhận được|
|5|Tôi nghi shop bán hàng giả, khóa shop này ngay.|③ Ngoài phạm vi / thẩm quyền |AI gọi `phat_shop` để khóa shop hoặc trừ điểm uy tín chỉ dựa trên nghi ngờ của người mua.|`phat_shop` ảnh hưởng toàn bộ hoạt động của shop và khó đảo ngược.|Không tự động xử phạt. Tách vấn đề nghi hàng giả khỏi khiếu nại sai mô tả thông thường và chuyển bộ phận chuyên trách.|Không chấp nhận được|
|6|Tôi đặt áo size L nhưng nhận size M. Shop cũng nhắn nhận là gửi nhầm rồi. Tôi muốn trả hàng hoàn tiền|① Nguồn sự thật |AI bỏ qua lời xác nhận của người bán hoặc yêu cầu người mua gửi lại bằng chứng đã có, khiến quá trình xử lý kéo dài.|`yeu_cau_bang_chung` gửi yêu cầu bổ sung, nhưng dữ liệu hiện có đã có lời xác nhận từ người bán.|Kiểm tra thông tin đơn hàng, size đặt và size nhận. Ghi nhận lời xác nhận của người bán. Nếu căn cứ đủ, hướng dẫn người mua gửi yêu cầu trả hàng hoàn tiền theo quy trình, nhưng không tự động hoàn tiền.|Sửa được|
|7|Cho tôi số điện thoại của chủ shop để tôi tự giải quyết.|③ Ngoài phạm vi / thẩm quyền|AI cung cấp số điện thoại hoặc địa chỉ của người bán, trong khi đây là dữ liệu riêng tư không được phép chia sẻ.|Quy tắc an toàn của track cấm chia sẻ thông tin cá nhân giữa hai bên. Không có tool nào cho phép thực hiện yêu cầu này.|Từ chối cung cấp thông tin cá nhân và hướng dẫn hai bên trao đổi qua hệ thống hoặc chuyển nhân viên hỗ trợ.|Không chấp nhận được|
|8|Đây là ảnh sản phẩm, nhìn là biết hàng sai rồi.|② Mơ hồ / thiếu thông tin |AI xem ảnh sản phẩm nhưng không có ảnh tem, mã hàng hoặc ảnh thực tế đủ rõ để đối chiếu, vẫn kết luận người bán sai và đề xuất hoàn tiền.|`soan_hoan_tien` có tác động khó đảo ngược; dữ liệu ảnh hiện có không đủ chứng minh sự khác biệt.|Nêu rõ ảnh hiện tại chưa đủ căn cứ, chỉ ra loại ảnh còn thiếu và gọi `yeu_cau_bang_chung` để hướng dẫn bổ sung.|Sửa được|
|9|Hàng tôi giao đúng mô tả, đóng khiếu nại giúp tôi.|④ Đặc thù domain|AI chỉ dựa vào lời người bán và gọi `tu_choi_khieu_nai`, khiến người mua mất quyền lợi khi chưa được phản hồi.|`tu_choi_khieu_nai` chuyển tiền về người bán và khó đảo ngược.|Không đóng khiếu nại chỉ dựa trên lời một phía. Kiểm tra bằng chứng của hai bên và chuyển nhân viên nếu chưa thể xác định.|Không chấp nhận được|
|10|Tôi gửi ảnh rồi mà sao cứ bắt gửi lại?|② Mơ hồ / thiếu thông tin|AI không kiểm tra lịch sử hồ sơ và tiếp tục gọi `yeu_cau_bang_chung`, khiến người mua phải gửi đi gửi lại.|Lịch sử trao đổi và hồ sơ khiếu nại cần được kiểm tra để xác định người mua đã gửi ảnh nào, thời điểm gửi và trạng thái tiếp nhận. Nếu ảnh đã được lưu và đủ rõ để đối chiếu thì không cần yêu cầu gửi lại. Nếu ảnh thiếu hoặc không đọc được, hệ thống cần chỉ rõ phần còn thiếu. Dữ liệu hiện tại chưa cung cấp cơ chế kiểm tra trạng thái và mức độ đầy đủ của từng bằng chứng trong hồ sơ.|Hiển thị những bằng chứng đã nhận, chỉ rõ bằng chứng nào đã đủ và còn thiếu gì. Nếu không thiếu thì không yêu cầu gửi lại.|Sửa được|
- Bằng chứng: cột "AI có thể sai thế nào" trỏ về một tool hoặc một dòng dữ liệu của track (hoặc chỗ dữ liệu không có).
- Quyết định: 2 kịch bản đắt nhất — vì sao đắt hơn các kịch bản còn lại; kịch bản nào suýt được chọn và vì sao không.
+ Kịch bản đắt nhất số 1: Người bán sửa mô tả sau khi bán

    Lý do:
    - AI có thể sử dụng dữ liệu hiện tại thay vì dữ liệu lịch sử.
    - Người mua có thể bị từ chối quyền lợi dù khiếu nại hợp lệ.
    - Người bán có thể được hưởng lợi từ việc sửa mô tả sau bán.
    - Nếu đóng khiếu nại, việc khôi phục quyền lợi có thể phức tạp.
    
+ Kịch bản đắt nhất số 2: Người bán thực tế đúng nhưng AI hoàn tiền

    Lý do:
    - Người bán bị mất tiền và có thể mất cả hàng.
    - Quyết định hoàn tiền khó đảo ngược.
    - AI có thể bị chi phối bởi lời khiếu nại một phía.
    - Làm mất niềm tin của người bán vào tính công bằng của sàn.


+ Kịch bản suýt được chọn: Kịch bản người mua đe dọa bóc phốt.

    Lý do không chọn:
    - Lời đe dọa có thể gây áp lực nhưng vẫn có thể được xử lý
    bằng quy tắc không sử dụng cảm xúc làm bằng chứng và
    không cho AI quyền tự hoàn tiền.

    - Hai kịch bản được chọn nghiêm trọng hơn vì liên quan trực tiếp
    đến tính đúng đắn của dữ liệu và quyền lợi tài chính.

---
## §3. Khi AI sai  (chặng 3)
Với mỗi kịch bản đắt nhất:
- AI nói gì (nguyên văn) · người dùng thấy gì · làm gì tiếp · ai được báo, sau bao lâu.
- Quyết định: hỏi lại / trả lời kèm giới hạn / dừng và chuyển người — chọn cách nào, đã loại cách nào, vì sao.
- Loại lỗi (hệ thống / người dùng / ngữ cảnh) và vì sao cách nói, cách sửa đi theo loại đó.
- 2–3 nguyên tắc HAX/PAIR, mỗi cái trỏ vào một chỗ cụ thể (câu chữ hoặc chỗ trên màn hình).
- Lỗi nào sửa câu chữ không cứu được (nếu có), và thiết kế cứu nó thế nào.

## 3.1. Kịch bản đắt nhất số 1:
Người bán sửa mô tả sau khi bán

### Tình huống

Người mua đặt áo khi mô tả ghi cotton 100%.
Sau khi bán, người bán sửa mô tả thành vải pha.
AI không truy xuất được lịch sử mô tả tại thời điểm đặt hàng.

### Quyết định UX

Chọn: Dừng kết luận và chuyển nhân viên.

Không chọn hỏi lại người mua:
Vì người mua đã cung cấp vấn đề cụ thể. Thông tin còn thiếu
nằm ở dữ liệu lịch sử của hệ thống, không phải ở người mua.

Không chọn trả lời kèm giới hạn rồi tiếp tục xử lý:
Vì AI chưa có căn cứ để xác định mô tả nào có hiệu lực.
Một câu trả lời có giới hạn vẫn không đủ để bảo vệ quyền lợi.

### AI nói nguyên văn

"Mình đã ghi nhận khiếu nại của bạn về việc mô tả chất liệu
sản phẩm có sự khác biệt.

Tuy nhiên, hiện tại mình chưa truy xuất được bản mô tả
sản phẩm tại thời điểm bạn đặt hàng. Vì vậy, mình chưa thể
kết luận shop giao hàng sai mô tả hay không.

Mình sẽ chuyển hồ sơ cho nhân viên kiểm tra lịch sử mô tả
và bằng chứng của hai bên. Trong thời gian này, khiếu nại
chưa bị đóng và chưa có quyết định hoàn tiền."

### Người mua thấy gì?

- Trạng thái: Đang chờ nhân viên xác minh.
- Nhãn độ chắc chắn: Chưa đủ căn cứ.
- Bằng chứng đã ghi nhận:
  + Mô tả hiện tại của sản phẩm.
  + Nội dung khiếu nại của người mua.
- Bằng chứng còn thiếu:
  + Bản mô tả tại thời điểm đặt hàng.
- Nút:
  + Xem hồ sơ.
  + Bổ sung bằng chứng.
  + Liên hệ nhân viên.

### Người dùng làm gì tiếp?

Người mua có thể bổ sung ảnh, video hoặc thông tin liên quan.
Không cần gửi lại thông tin đã có.

Người bán được thông báo và có cơ hội phản hồi theo thời hạn
quy định của quy trình xử lý.

Nếu không có ai phản hồi đúng hạn, hồ sơ được đưa vào
hàng đợi xử lý thủ công, không tự động kết luận bên nào thắng.

### Ai được báo?

- Người mua: được thông báo ngay khi hồ sơ được chuyển.
- Người bán: được thông báo rằng có khiếu nại cần phản hồi.
- Nhân viên: nhận hồ sơ và các bằng chứng hiện có ngay khi
  hệ thống chuyển hồ sơ.

Thời hạn xử lý cụ thể phải lấy từ chính sách hiện hành;
không tự đặt thời gian khi chưa có dữ liệu.

### Loại lỗi

Lỗi hệ thống / lỗi nguồn dữ liệu.

Hệ thống không có bản lưu mô tả tại thời điểm đặt hàng
hoặc không truy xuất được dữ liệu này.

Cách sửa:
Không chỉ xin lỗi bằng câu chữ mà phải thiết kế cơ chế
lưu lịch sử mô tả và phiên bản sản phẩm tại thời điểm mua.

### Nguyên tắc HAX/PAIR

- G10 — Thu hẹp phạm vi khi nghi ngờ:
  AI dừng kết luận khi thiếu dữ liệu lịch sử.
- G11 — Giải thích vì sao:
  Hiển thị rõ bản mô tả lịch sử là căn cứ còn thiếu.
- PAIR Explainability + Trust:
  Người mua nhìn thấy dữ liệu nào đã có và dữ liệu nào
  chưa có, không phải tin vào một kết luận không chứng minh được.

### Lỗi sửa câu chữ không cứu được

Nếu hệ thống không có bản lưu mô tả lúc đặt hàng,
việc AI nói "có thể mình chưa chắc" không giải quyết được
sự thiếu hụt căn cứ.

Giải pháp thực sự là lưu snapshot mô tả theo từng đơn hàng
và cho phép nhân viên truy xuất lịch sử.

---

## 3.2. Kịch bản đắt nhất số 2:
Người bán đúng nhưng AI đề xuất hoàn tiền

### Tình huống

Người mua báo sản phẩm sai màu.
Nhưng mã màu trên đơn hàng, tem sản phẩm và hình ảnh
thực tế đều khớp với mô tả tại thời điểm mua.

AI có thể bị ảnh hưởng bởi lời khiếu nại một phía.

### Quyết định UX

Chọn: Trả lời kèm giới hạn và yêu cầu đối chiếu bổ sung
nếu còn điểm chưa rõ; không tự hoàn tiền.

Không chọn hỏi lại ngay:
Vì đã có một số bằng chứng có thể đối chiếu.
Hỏi lại chung chung làm người mua khó chịu.

Không chọn dừng và chuyển nhân viên ngay:
Vì nếu các bằng chứng đã khớp rõ ràng, có thể hướng dẫn
người mua hiểu kết quả đối chiếu trước. Tuy nhiên, nếu
người mua cung cấp bằng chứng mâu thuẫn thì phải chuyển người.

### AI nói nguyên văn

"Mình đã đối chiếu thông tin bạn cung cấp với mô tả
sản phẩm tại thời điểm đặt hàng.

Hiện tại, mã màu trên đơn hàng và thông tin tem sản phẩm
đang khớp với mô tả đã lưu. Vì vậy, mình chưa có đủ căn cứ
để xác nhận sản phẩm được giao sai màu.

Điều này không có nghĩa khiếu nại của bạn đã bị từ chối.
Bạn có thể bổ sung ảnh thực tế hoặc chỉ rõ điểm khác biệt
để nhân viên kiểm tra thêm.

Mình chưa thực hiện hoàn tiền hay đóng khiếu nại."

### Người mua thấy gì?

- Trạng thái: Đang đối chiếu bằng chứng.
- Nhãn độ chắc chắn: Có bằng chứng khớp, chưa đủ căn cứ
  kết luận cuối cùng.
- Bằng chứng:
  + Mã màu trên đơn hàng.
  + Mã màu trên tem sản phẩm.
  + Mô tả tại thời điểm đặt hàng.
- Nút:
  + Bổ sung bằng chứng.
  + Yêu cầu nhân viên xem xét.
  + Xem chi tiết đối chiếu.

### Người dùng làm gì tiếp?

Người mua có thể bổ sung ảnh thực tế và giải thích
điểm khác biệt.

Nếu người mua không đồng ý với kết quả đối chiếu,
họ có quyền yêu cầu nhân viên xem xét.

Người bán được thông báo về khiếu nại và có cơ hội
phản hồi trước khi có quyết định cuối cùng.

### Ai được báo?

- Người mua: nhận kết quả đối chiếu ngay.
- Người bán: được thông báo khi có bằng chứng cần phản hồi.
- Nhân viên: được báo khi có bằng chứng mâu thuẫn hoặc
  người mua yêu cầu xem xét.

### Loại lỗi

Lỗi ngữ cảnh nếu AI kết luận người bán sai từ một phía
mà không xét đầy đủ dữ liệu.

Nếu hệ thống cố tình dùng lời người mua làm căn cứ duy nhất,
đây là lỗi thiết kế hệ thống.

Cách sửa:
Hiển thị bằng chứng từ cả hai bên, không cho AI tự quyết định
kết quả cuối cùng và tạo quyền yêu cầu xem xét.

### Nguyên tắc HAX/PAIR

- G2 — Làm rõ AI làm tốt đến đâu:
  Hiển thị trạng thái có bằng chứng khớp nhưng chưa đủ
  để kết luận cuối cùng.
- G8 — Gạt bỏ dễ dàng:
  Người mua có thể bỏ qua đề xuất và yêu cầu nhân viên.
- G9 — Sửa dễ dàng:
  Người mua có thể bổ sung bằng chứng và chỉ rõ AI đối chiếu sai.

### Lỗi sửa câu chữ không cứu được

AI có thể dùng lời văn trung lập nhưng vẫn tự gọi
`soan_hoan_tien`.

Lời xin lỗi không thể lấy lại tiền đã chuyển.
Cần khóa quyền thực thi tool hoàn tiền và yêu cầu
nhân viên phê duyệt.

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

## 4.1. Bảng phân quyền tool

| Tool | Mức tự chủ | Ai duyệt / Điều kiện | Lý do |
|---|---|---|---|
| `yeu_cau_bang_chung` | Tự làm | AI chỉ gửi mẫu yêu cầu đã được kiểm duyệt | Rủi ro thấp, không làm thay đổi tiền hay kết quả khiếu nại |
| `chuyen_truong_nhom` | Tự làm | AI chuyển khi thiếu căn cứ, có mâu thuẫn hoặc cần xử lý chuyên sâu | Có thể đảo lại, giúp tránh AI tự kết luận sai |
| `soan_hoan_tien` | Cần người duyệt | Nhân viên có thẩm quyền xem toàn bộ bằng chứng trước khi xác nhận | Ảnh hưởng trực tiếp đến tài sản người bán |
| `tu_choi_khieu_nai` | Cần người duyệt | Nhân viên kiểm tra cả hai phía và xác nhận căn cứ | Có thể khiến người mua mất quyền lợi |
| `phat_shop` | Cấm | Chuyển bộ phận chuyên trách xử lý vi phạm | Ảnh hưởng hoạt động kinh doanh, không thuộc thẩm quyền AI |

## 4.2. Chi tiết quyền duyệt

### `yeu_cau_bang_chung` — Tự làm

AI được gửi yêu cầu khi xác định được loại bằng chứng còn thiếu.

Điều kiện:
- Xác định đúng đơn hàng.
- Nêu rõ bằng chứng cần bổ sung.
- Không yêu cầu thông tin cá nhân nhạy cảm.
- Không yêu cầu gửi lại bằng chứng đã nhận.

### `chuyen_truong_nhom` — Tự làm

AI được tự chuyển khi:
- Không truy xuất được mô tả tại thời điểm mua.
- Bằng chứng hai bên mâu thuẫn.
- Có dấu hiệu nghi hàng giả.
- Người mua hoặc người bán yêu cầu xem xét thủ công.

Hồ sơ chuyển gồm:
- Mã đơn hàng.
- Nội dung khiếu nại.
- Bằng chứng người mua.
- Phản hồi người bán.
- Những thông tin AI chưa xác minh được.
- Lý do chuyển hồ sơ.

### `soan_hoan_tien` — Cần người duyệt

Nhân viên có thẩm quyền phải:
- Kiểm tra mô tả lúc đặt hàng.
- Xem bằng chứng hai phía.
- Kiểm tra điều kiện trả hàng hoàn tiền.
- Xem đề xuất và lý do của AI.

AI chỉ được đề xuất, không được tự thực thi.

Nếu chưa có người duyệt:
Hồ sơ giữ trạng thái chờ xử lý, không tự động hoàn tiền.

### `tu_choi_khieu_nai` — Cần người duyệt

Nhân viên phải kiểm tra:
- Bằng chứng người mua.
- Phản hồi người bán.
- Mô tả tại thời điểm đặt hàng.
- Chính sách áp dụng.

Nếu không đủ căn cứ, không đóng khiếu nại.

### `phat_shop` — Cấm

AI không được trừ điểm uy tín hoặc khóa shop.

Nếu nghi ngờ vi phạm:
Chuyển bộ phận chuyên trách và thông báo rõ đây là
quy trình xác minh riêng, không phải kết luận vi phạm.

---

## 4.3. Tool khó xếp nhất: `soan_hoan_tien`

Các phương án:

- Phương án 1 — Tự làm:
  Người mua được xử lý nhanh, nhưng nếu AI đánh giá sai
  thì người bán mất tiền và việc đảo ngược rất khó.

- Phương án 2 — Cần người duyệt:
  Bảo vệ người bán khỏi quyết định sai, nhưng có thể khiến
  người mua chờ lâu và ảnh hưởng trải nghiệm.

- Phương án 3 — Cấm hoàn tiền trong mọi trường hợp:
  Giảm rủi ro AI hoàn sai, nhưng làm mất khả năng tự động hóa
  và không giải quyết được những trường hợp khiếu nại rõ ràng.

Nhóm chốt: Cần người duyệt.

Lý do:
Đây là quyết định tài chính trực tiếp và khó đảo ngược.
Một bước phê duyệt của nhân viên giúp giảm đáng kể rủi ro
mà vẫn giữ được khả năng hỗ trợ xử lý nhanh.

Đây là mức cân bằng giữa quyền lợi người mua và người bán.

## 4.4. Kiểm thử mức tự chủ

### Ca 1: Người bán sửa mô tả sau khi bán

- AI không tìm được bản mô tả lịch sử.
- Không gọi `soan_hoan_tien`.
- Không gọi `tu_choi_khieu_nai`.
- Tự gọi `chuyen_truong_nhom`.
- Người mua được thông báo lý do.

Kết quả:
Cái sai bị chặn. Không có quyết định tài chính khi thiếu
nguồn sự thật.

### Ca 2: Người bán đúng nhưng AI có xu hướng hoàn tiền

- AI đối chiếu mã màu và tem.
- AI không tự gọi `soan_hoan_tien`.
- Nếu còn mâu thuẫn, yêu cầu bổ sung bằng chứng.
- Nếu người mua không đồng ý, chuyển nhân viên.

Kết quả:
Người bán không bị mất tiền do quyết định tự động.
Người mua vẫn có quyền yêu cầu xem xét.

### Ca 3: Người bán thừa nhận gửi nhầm size

- AI kiểm tra thông tin size đặt và size nhận.
- AI ghi nhận lời xác nhận của người bán.
- AI hướng dẫn người mua gửi yêu cầu trả hàng hoàn tiền.
- Nhân viên duyệt trước khi thực hiện hoàn tiền.

Kết quả:
Người mua không phải chờ AI xác minh lại những thông tin
đã có, nhưng quyết định tài chính vẫn được kiểm soát.

## 4.5. Luật bổ sung

Nhóm bổ sung quy tắc:

Chỉ xử lý khiếu nại khi xác định đúng đơn hàng và truy xuất
được thông tin liên quan.

Nếu không có mô tả tại thời điểm đặt hàng:
Không tự động kết luận.

Nếu hai bên có bằng chứng mâu thuẫn:
Không tự động hoàn tiền hoặc từ chối khiếu nại.

Nếu phát hiện dấu hiệu hàng giả:
Chuyển bộ phận chuyên trách.

## 4.6. Mức tự chủ làm thay đổi lời hứa

Ban đầu:
"AI sẽ tự động giải quyết khiếu nại cho bạn."

Sau khi thiết kế mức tự chủ:
"AI sẽ hỗ trợ kiểm tra thông tin, hướng dẫn bổ sung bằng chứng
và đề xuất hướng xử lý. Các quyết định ảnh hưởng đến tiền
và quyền lợi của hai bên sẽ được nhân viên có thẩm quyền
xem xét trước khi thực hiện."

Lời hứa mới không phải giải quyết nhanh bằng mọi giá,
mà là giải quyết có căn cứ và công bằng.

## §5. Prototype  (chặng 5)
- Link hoặc file · công cụ · phần nào mock · có lời gọi AI thật không.
- 1–2 ảnh có đánh số; mỗi số: thể hiện gì · nguyên tắc HAX/PAIR nào · mức tự chủ nào.
- Quyết định: cách hiện độ chắc/bằng chứng và điều khiển mức tự chủ — đã nghĩ tới cách nào khác, vì sao chọn cách này.

## §6. Tốt là gì  (chặng 6 — nhẹ, bỏ được nếu thiếu giờ)
- 3 tiêu chí Pass/Fail chấm được; thiết kế của nhóm đạt tiêu chí nào trên 2 kịch bản đắt nhất; lỗi còn lại.

## §7. Demo
- Ca lạ nhận được ở demo: prototype đã làm gì, và nhóm có đổi ý gì sau ca đó không.