# Design Sketch Notes

## 1. Mục tiêu của sketch
Mình vẽ sketch để giải thích luồng tổng thể của project `vinhomes_agent`, vì nếu chỉ nhìn từng file riêng lẻ thì khá khó thấy chatbot đang vận hành theo vòng nào. Sketch giúp mình và các bạn trong nhóm thống nhất hệ thống được chia thành 3 lớp:

- `web.py`: nhận request từ frontend hoặc API client
- `service.py`: giữ session, state, trace, summary và điều phối chat
- `tools.py`: thực thi logic nghiệp vụ cụ thể như FAQ, ticket, trip planning

## 2. Flow tổng thể mình hình dung
1. User gửi message từ giao diện chat
2. `web.py` nhận request qua endpoint `/api/chat` hoặc `/api/chat/stream`
3. `service.py` lấy `session_id`, nạp state cũ hoặc tạo state mới
4. Message mới được thêm vào history
5. Agent xử lý và chọn flow phù hợp
6. Khi cần, agent gọi tool trong `tools.py`
7. Kết quả được ghi lại vào state, trace, summary
8. Response cuối cùng được trả lại cho user

## 3. 3 nhánh chính trong sketch

### FAQ flow
- Dùng khi user hỏi thông tin như nội quy, tiện ích, phí hoặc các câu thường gặp
- Tool chính là `search_faq_kb`
- Nếu dữ liệu nội bộ chưa đủ, có thể fallback sang `search_faq_web`
- Mục tiêu là trả lời nhanh và có nguồn bám theo dữ liệu FAQ

### Ticket flow
- Dùng khi user muốn báo sự cố
- Các bước quan trọng là lấy schema, kiểm tra field thiếu, tóm tắt ticket nháp và tạo ticket mock
- Các tool liên quan gồm `get_ticket_schema`, `validate_ticket_input`, `summarize_ticket_draft`, `create_ticket`

### Trip flow
- Dùng khi user muốn lên kế hoạch đi chơi ở Vinhomes Ocean Park
- Flow này dài hơn vì phải thu constraint rồi mới build itinerary
- Các tool chính là `get_trip_requirements_helper`, `get_trip_context`, `build_itinerary`

## 4. Điều mình rút ra từ sketch
- Nếu không chia riêng `web`, `service`, `tools` thì hệ thống sẽ khó đọc và khó demo
- `service.py` là phần rất quan trọng vì nó giữ state và session, giúp chatbot không bị “mất trí nhớ” giữa các lượt chat
- Phần sketch giúp mình giải thích project dễ hơn khi trình bày, vì người nghe nhìn flow sẽ hiểu nhanh hơn đọc code