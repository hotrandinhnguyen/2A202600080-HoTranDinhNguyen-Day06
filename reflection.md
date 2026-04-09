# Individual reflection — [Hồ Trần Đình Nguyên] ([2A202600080])

## 1. Role
Flow designer + backend support. Phụ trách phác thảo luồng hoạt động của chatbot, vẽ sơ đồ sketch cho agent, và hỗ trợ làm rõ logic giữa các file `tools.py`, `service.py` và `web.py` trong project `vinhomes_agent`.

## 2. Đóng góp cụ thể
- Vẽ sơ đồ sketch để mô tả chatbot nhận message, định tuyến intent và đi vào các nhánh xử lý khác nhau thay vì trả lời một cách tuyến tính.
- Hỗ trợ nhóm làm rõ 3 flow chính đang có trong code: tra cứu FAQ từ file CSV, tạo ticket sự cố với các trường bắt buộc, và lên lịch trình đi chơi tại Vinhomes Ocean Park.
- Hoàn thiện phần `tools.py`, trong đó mỗi tool có nhiệm vụ khá rõ: `search_faq_kb`, `search_faq_web`, `get_ticket_schema`, `validate_ticket_input`, `summarize_ticket_draft`, `create_ticket`, `get_trip_requirements_helper`, `get_trip_context`, `build_itinerary`.
- Hoàn thiện phần `service.py` để quản lý session, lưu state, tạo snapshot/summary, build trace và xử lý vòng đời chat thay vì chỉ trả lời một lượt đơn lẻ.
- Hoàn thiện phần `web.py` để nối backend với giao diện qua các API như tạo session, chat thường, chat stream, xem session, xóa session, clear history và gửi feedback.
- Góp ý cách tổ chức conversation flow để bot hỏi thêm khi thiếu dữ liệu, thay vì tự đoán. Điều này đặc biệt quan trọng ở flow tạo ticket và flow planner.
- Hỗ trợ kiểm tra trải nghiệm end-to-end từ phía người dùng: tạo session mới, chat, nhận reply, xem trace/summary và cơ chế feedback like/dislike.

## 3. SPEC mạnh/yếu
- Mạnh nhất: phần code được chia tương đối đúng trách nhiệm. `tools.py` lo xử lý nghiệp vụ và dữ liệu, `service.py` lo state/session/trace, còn `web.py` lo expose API và streaming. Cách tách này giúp hệ thống dễ đọc và dễ demo hơn.
- Yếu nhất: phần orchestration trung tâm của agent chưa lộ ra đầy đủ trong repo hiện tại, nên khi đọc code vẫn phải suy luận thêm luồng tổng thể từ `service.py`, các tool và phần sketch mình vẽ.

## 4. Đóng góp khác
- Vẽ thêm sơ đồ sketch để cả nhóm dễ thống nhất cách `web -> service -> tools` kết nối với nhau, nhất là phần từ user message sang session, rồi sang xử lý và trả response.
- Hỗ trợ nhóm trình bày phần trip planning vì flow này có nhiều bước hơn FAQ thông thường: thu constraint, lấy context, tính budget và build itinerary.
- Góp ý cách mô tả phần trace trong `service.py` để khi demo có thể giải thích bot đã route gì, gọi tool nào và đang ở trạng thái nào.
- Hỗ trợ kiểm tra logic của endpoint chat stream trong `web.py`, vì đây là phần giúp trải nghiệm demo mượt hơn so với chỉ trả response một lần.

## 5. Điều học được
Khi làm project này, mình hiểu rõ hơn rằng với AI agent, phần quan trọng không chỉ là model mà còn là cách chia lớp trong hệ thống. Từ `vinhomes_agent`, mình học được rằng nếu tách `tools`, `service` và `web` rõ ràng thì việc phát triển và debug sẽ dễ hơn rất nhiều. Một chatbot tốt không phải chỉ trả lời được, mà còn phải biết khi nào cần hỏi tiếp, khi nào cần tạo draft, khi nào cần stream phản hồi, và khi nào cần ghi log để theo dõi sau này.

## 6. Nếu làm lại
Mình sẽ vẽ sơ đồ sketch sớm hơn và chi tiết hơn ngay từ đầu, rồi đối chiếu trực tiếp với code trong lúc làm. Như vậy nhóm sẽ dễ thống nhất hơn giữa phần backend, phần `tool logic`, phần `service` quản lý state và phần `web` expose API. Ngoài ra mình cũng sẽ bổ sung sơ đồ riêng cho session lifecycle, feedback flow và chat stream vì đây là những phần khá hay của project nhưng dễ bị bỏ qua khi thuyết trình.

## 7. AI giúp gì / AI sai gì
- **Giúp:** AI hỗ trợ brainstorm cách chia flow, cách đặt câu hỏi follow-up cho bot, và cách tách trách nhiệm giữa `tools`, `service` và `web`. Nó cũng giúp mình nghĩ nhanh nhiều phương án biểu diễn sơ đồ sketch trước khi chốt bản dễ hiểu nhất cho nhóm.
- **Sai/mislead:** AI đôi khi gợi ý flow quá phức tạp so với code hiện tại, thêm nhiều lớp agent hoặc nhiều endpoint/tool chưa cần thiết. Nếu làm theo hết thì dễ làm bài nặng hơn scope thật của project. Bài học mình rút ra là AI hữu ích để mở ý tưởng, nhưng mình vẫn phải bám sát code đang có trong `vinhomes_agent`.
