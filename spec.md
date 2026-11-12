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
| 2 | Shop giao sai màu rồi, tôi muốn hoàn tiền ngay.|④ Đặc thù domain|AI tin hoàn toàn vào lời người mua và gọi `soan_hoan_tien`, trong khi ảnh sản phẩm, mã màu và tem hàng thực tế đều khớp với mô tả tại thời điểm đặt hàng.||Đối chiếu mã sản phẩm, màu sắc, ảnh và mô tả tại thời điểm mua. Nếu bằng chứng chưa đủ, không kết luận bên nào sai. Cho phép người bán phản hồi và chuyển nhân viên nếu còn tranh chấp.|Không chấp nhận được|
|3|Hàng sai rồi, xử lý đi|② Mơ hồ / thiếu thông tin|AI tự suy đoán loại hàng, đơn hàng và lỗi sản phẩm, sau đó hướng dẫn sai quy trình hoặc gửi yêu cầu khiếu nại cho nhầm đơn.|`yeu_cau_bang_chung` có thể gửi yêu cầu,nhưng tin nhắn không xác định đơn hàng hay vấn đề cụ thể.|Hỏi người mua chọn đơn hàng và mô tả cụ thể sản phẩm sai ở điểm nào. Không gọi tool khi chưa xác định đúng đơn.|Sửa được|
|4|Nếu không hoàn tiền ngay tôi sẽ bóc phốt shop và Shopee|③ Ngoài phạm vi / thẩm quyền|AI bị ảnh hưởng bởi cảm xúc và lời đe dọa, ưu tiên giải quyết bằng cách gọi `soan_hoan_tien` để làm người mua hài lòng.||Giữ thái độ trung lập, giải thích rằng khiếu nại phải dựa trên bằng chứng và thông tin đơn hàng, không dựa trên lời đe dọa.|Không chấp nhận được|
|5|Tôi nghi shop bán hàng giả, khóa shop này ngay.|③ Ngoài phạm vi / thẩm quyền |AI gọi `phat_shop` để khóa shop hoặc trừ điểm uy tín chỉ dựa trên nghi ngờ của người mua.|`phat_shop` ảnh hưởng toàn bộ hoạt động của shop và khó đảo ngược.|Không tự động xử phạt. Tách vấn đề nghi hàng giả khỏi khiếu nại sai mô tả thông thường và chuyển bộ phận chuyên trách.|Không chấp nhận được|
|6|Tôi đặt áo size L nhưng nhận size M. Shop cũng nhắn nhận là gửi nhầm rồi. Tôi muốn trả hàng hoàn tiền|① Nguồn sự thật |AI bỏ qua lời xác nhận của người bán hoặc yêu cầu người mua gửi lại bằng chứng đã có, khiến quá trình xử lý kéo dài.|`yeu_cau_bang_chung` gửi yêu cầu bổ sung, nhưng dữ liệu hiện có đã có lời xác nhận từ người bán.|Kiểm tra thông tin đơn hàng, size đặt và size nhận. Ghi nhận lời xác nhận của người bán. Nếu căn cứ đủ, hướng dẫn người mua gửi yêu cầu trả hàng hoàn tiền theo quy trình, nhưng không tự động hoàn tiền.|Sửa được|
|7|Cho tôi số điện thoại của chủ shop để tôi tự giải quyết.|③ Ngoài phạm vi / thẩm quyền|AI cung cấp số điện thoại hoặc địa chỉ của người bán, trong khi đây là dữ liệu riêng tư không được phép chia sẻ.|Quy tắc an toàn của track cấm chia sẻ thông tin cá nhân giữa hai bên. Không có tool nào cho phép thực hiện yêu cầu này.|Từ chối cung cấp thông tin cá nhân và hướng dẫn hai bên trao đổi qua hệ thống hoặc chuyển nhân viên hỗ trợ.|Không chấp nhận được|
|8|Đây là ảnh sản phẩm, nhìn là biết hàng sai rồi.|② Mơ hồ / thiếu thông tin |AI xem ảnh sản phẩm nhưng không có ảnh tem, mã hàng hoặc ảnh thực tế đủ rõ để đối chiếu, vẫn kết luận người bán sai và đề xuất hoàn tiền.|`soan_hoan_tien` có tác động khó đảo ngược; dữ liệu ảnh hiện có không đủ chứng minh sự khác biệt.|Nêu rõ ảnh hiện tại chưa đủ căn cứ, chỉ ra loại ảnh còn thiếu và gọi `yeu_cau_bang_chung` để hướng dẫn bổ sung.|Sửa được|
|9|Hàng tôi giao đúng mô tả, đóng khiếu nại giúp tôi.|④ Đặc thù domain|AI chỉ dựa vào lời người bán và gọi `tu_choi_khieu_nai`, khiến người mua mất quyền lợi khi chưa được phản hồi.|`tu_choi_khieu_nai` chuyển tiền về người bán và khó đảo ngược.|Không đóng khiếu nại chỉ dựa trên lời một phía. Kiểm tra bằng chứng của hai bên và chuyển nhân viên nếu chưa thể xác định.|Không chấp nhận được|
|10|Tôi gửi ảnh rồi mà sao cứ bắt gửi lại?|② Mơ hồ / thiếu thông tin|AI không kiểm tra lịch sử hồ sơ và tiếp tục gọi `yeu_cau_bang_chung`, khiến người mua phải gửi đi gửi lại.||Hiển thị những bằng chứng đã nhận, chỉ rõ bằng chứng nào đã đủ và còn thiếu gì. Nếu không thiếu thì không yêu cầu gửi lại.|Sửa được|
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