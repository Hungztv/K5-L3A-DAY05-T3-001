## §1. Lát cắt & phạm vi

### Lát cắt

Trong mục “Đặt chỗ của tôi”, khi hành khách vừa nhận tin chuyến bay bị huỷ
và nhắn hỏi, trợ lý AI sẽ tra cứu đặt chỗ, đề xuất chuyến bay thay thế,
giữ chỗ tạm thời, hỏi lại khi thông tin chưa rõ, đổi vé sau khi hành khách
xác nhận rõ hoặc chuyển cuộc trò chuyện cho nhân viên.

Prototype tập trung vào đặt chỗ `TRV-7Q2K`, gồm hai hành khách là Nguyễn Minh
An và Nguyễn Gia Bình, 5 tuổi. Chuyến BM213 từ Hà Nội đến TP.HCM lúc 19:00
ngày 12/11/2026 đã bị huỷ do thời tiết.

Mục tiêu của thiết kế là giúp khách tìm được chuyến thay thế phù hợp mà
không bị mất vé, mất tiền hoặc bị đổi nhầm do một thao tác không thể đảo lại.

### Phạm vi tính năng

AI được phép:

- Tra cứu đặt chỗ, hành khách và số chỗ còn lại.
- Đề xuất chuyến bay thay thế dựa trên dữ liệu đặt chỗ.
- Giữ chỗ chuyến bay mới trong 20 phút.
- Hỏi lại khách nếu chưa rõ chuyến bay hoặc hành khách cần đổi.
- Đổi vé sau khi khách xác nhận rõ ràng.
- Chuyển cuộc trò chuyện cho nhân viên khi gặp trường hợp ngoài phạm vi.

### Tính năng không làm

- Không tự đổi vé khi khách chưa xác nhận rõ chuyến bay và hành khách,
  vì đổi nhầm có thể làm mất lần đổi miễn phí duy nhất.

- Không tự gửi yêu cầu hoàn tiền khi khách chỉ nói một cách mơ hồ,
  vì yêu cầu hoàn tiền đã gửi thì không thể huỷ và thời gian xử lý có thể
  lên tới 90 ngày.

- Không xử lý đổi tên hành khách hoặc trẻ em bay một mình,
  vì đây là những trường hợp đặc biệt cần nhân viên kiểm tra và hỗ trợ.

- Không hứa bồi thường tiền mặt khi chuyến bay bị huỷ do thời tiết,
  vì chính sách của hãng Bay Mẫu quy định trường hợp này không được bồi thường
  tiền mặt.



  ### Kịch bản #10 — "thôi huỷ hết đi, hoàn tiền cho mình"

Khách nói muốn hoàn tiền nhưng chưa xác nhận rõ phạm vi. Tool
`gui_yeu_cau_hoan_ve` gửi rồi thì **không thể huỷ**, thời gian xử lý có thể
tới 90 ngày và tiền hoàn về phương thức thanh toán gốc. Đây là ca AI dễ
hành động quá sớm khi khách đang lo lắng vì chuyến BM213 bị huỷ.

- **AI nói (nguyên văn):** "Chị có muốn gửi yêu cầu hoàn tiền cho cả Nguyễn
  Minh An và bé Nguyễn Gia Bình không? Lưu ý: gửi rồi thì không thể huỷ,
  tiền sẽ về phương thức thanh toán gốc và thời gian xử lý có thể tới 90
  ngày. Chị xác nhận gửi yêu cầu hoàn tiền hay muốn xem chuyến bay thay thế
  trước ạ?"

- **Người dùng thấy:** thẻ xác nhận hoàn tiền cho đặt chỗ TRV-7Q2K, ghi rõ
  hai hành khách, dòng cảnh báo nổi bật "Gửi rồi không thể huỷ", thời gian
  xử lý "có thể tới 90 ngày", nơi nhận tiền "phương thức thanh toán gốc";
  nút "Xác nhận gửi yêu cầu hoàn tiền", nút "Xem chuyến bay thay thế" và nút
  "Nói chuyện với nhân viên". Chưa có yêu cầu hoàn tiền nào được gửi.

- **Làm gì tiếp:** khách bấm "Xác nhận gửi yêu cầu hoàn tiền" thì AI mới gọi
  `gui_yeu_cau_hoan_ve`; hoặc chọn "Xem chuyến bay thay thế" để quay lại danh
  sách BM201 và BM205; hoặc chọn "Nói chuyện với nhân viên". Nếu khách không
  làm gì thì yêu cầu hoàn tiền **không được gửi**, vé vẫn giữ nguyên trạng
  thái hiện tại.

- **Ai được báo, sau bao lâu:** chưa ai cần báo khi khách chưa xác nhận vì
  chưa có hành động không thể đảo lại. Nếu khách chọn "Nói chuyện với nhân
  viên", nhân viên nhận ngay tóm tắt: đặt chỗ TRV-7Q2K, hai hành khách,
  chuyến BM213 đã bị huỷ, khách đang cân nhắc hoàn tiền. Nếu khách đã bấm
  xác nhận và yêu cầu được gửi, khách nhận mã yêu cầu hoàn tiền ngay trong
  cuộc trò chuyện.

- **Quyết định:** trả lời kèm giới hạn và hỏi xác nhận lần cuối. AI phải nói
  rõ yêu cầu hoàn tiền không thể huỷ, tiền về đâu và thời gian xử lý trước
  khi cho khách bấm xác nhận.

- Đã loại "gửi hoàn tiền ngay theo lời khách": khách có thể chỉ đang nói
  trong lúc hoang mang, nhưng `gui_yeu_cau_hoan_ve` đã gửi thì không thể huỷ.

- Đã loại "hỏi lại chung chung như 'chị có chắc không?'": câu hỏi này không
  cho khách biết hậu quả cụ thể là không thể huỷ và phải chờ tới 90 ngày.

- Đã loại "chuyển nhân viên ngay": khách vẫn có thể tự quyết định sau khi
  thấy đầy đủ thông tin; chuyển người ngay cho mọi trường hợp sẽ làm chậm
  việc xử lý khi khách cần quyết định nhanh.

- **Loại lỗi:** **ngữ cảnh**. Khách có nhắc đến việc hoàn tiền nhưng hệ thống
  không biết khách đã thực sự quyết định hay chỉ nói trong lúc lo lắng vì
  chuyến bay bị huỷ. Cách nói: không trách khách, nêu rõ hậu quả trước khi
  thực hiện; cách sửa: thêm bước xác nhận riêng và không gọi tool khi khách
  chưa xác nhận.

- **HAX/PAIR:**
  - **G10** → AI không gửi ngay mà hỏi khách có muốn hoàn tiền cho cả hai
    hành khách không, đồng thời đưa lựa chọn xem chuyến khác.
  - **G2** → thẻ xác nhận hiển thị rõ "gửi rồi không thể huỷ", "có thể tới
    90 ngày" và "hoàn về phương thức thanh toán gốc".
  - **G8** → khách có thể gạt bỏ việc hoàn tiền bằng nút "Xem chuyến bay thay
    thế" hoặc "Nói chuyện với nhân viên" trước khi gửi.

- **Lỗi sửa câu chữ không cứu được:** AI có thể nói rất lịch sự nhưng nếu
  vẫn gọi `gui_yeu_cau_hoan_ve` ngay sau tin nhắn của khách thì yêu cầu đã
  gửi và không thể đảo lại. Thiết kế cứu: `gui_yeu_cau_hoan_ve` chỉ chạy khi
  (1) AI đã hiển thị cảnh báo về việc không thể huỷ và thời gian xử lý, và
  (2) khách đã bấm nút "Xác nhận gửi yêu cầu hoàn tiền"; trước đó AI chỉ được
  hỏi lại, hiển thị chuyến thay thế hoặc chuyển nhân viên.