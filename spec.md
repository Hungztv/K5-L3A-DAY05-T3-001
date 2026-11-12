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