# Manual Test Log

## Test 1: Tạo session mới
- Mục tiêu: kiểm tra tạo session và lấy snapshot ban đầu
- Kỳ vọng: hệ thống trả về `session_id`, metadata và state rỗng
- Kết quả: đạt, service có thể tạo session mới và lưu metadata

## Test 2: Hỏi FAQ đơn giản
- Input: "Hồ bơi mở cửa lúc mấy giờ?"
- Mục tiêu: xem flow FAQ có được gọi không
- Kỳ vọng: bot route sang nhánh FAQ và truy xuất dữ liệu phù hợp từ file FAQ
- Quan sát: đây là dạng câu hỏi phù hợp với `search_faq_kb`

## Test 3: Báo sự cố thiếu thông tin
- Input: "Thang máy bị hỏng"
- Mục tiêu: kiểm tra bot có hỏi thêm field còn thiếu không
- Kỳ vọng: bot không tạo ticket ngay mà yêu cầu thêm vị trí, thời gian, mức khẩn cấp, liên hệ
- Quan sát: flow ticket được thiết kế hợp lý vì tránh tạo ticket thiếu dữ liệu

## Test 4: Ticket mô tả quá ngắn
- Input ticket draft có description rất ngắn
- Mục tiêu: kiểm tra `validate_ticket_input`
- Kỳ vọng: tool báo lỗi mô tả quá ngắn
- Kết quả mong muốn: bot phải yêu cầu user bổ sung thêm nội dung

## Test 5: Lên lịch trình đi chơi
- Input: "Mình muốn đi Ocean Park cuối tuần này với gia đình"
- Mục tiêu: kiểm tra trip flow có thu thêm thông tin hay không
- Kỳ vọng: bot hỏi thêm ngày đi, số người, ngân sách và sở thích
- Quan sát: flow này thể hiện rõ lợi ích của việc tách tool riêng thay vì trả lời tự do

## Test 6: Chat stream
- Mục tiêu: kiểm tra endpoint `/api/chat/stream`
- Kỳ vọng: reply được tách thành từng chunk và cuối cùng có event `done`
- Ý nghĩa: giúp trải nghiệm demo mượt hơn, nhìn giống chatbot realtime hơn

## Nhận xét chung
- Project có cấu trúc tốt cho demo vì không chỉ trả lời text mà còn có session, trace, feedback và streaming
- Điểm mình thấy hay là mỗi flow đều có ý tưởng kiểm tra dữ liệu đầu vào trước khi đi tiếp