# Milestone 12: Tích hợp AI và đảm bảo tuân thủ pháp lý

## Mục tiêu
Tích hợp nhà cung cấp AI bên ngoài và đảm bảo hệ thống tuân thủ các yêu cầu pháp lý trong lĩnh vực tài chính.

## Công việc cần thực hiện
- Tích hợp với nhà cung cấp AI (OpenAI hoặc Claude) thông qua biến môi trường
- Cấu hình LLM để chỉ cung cấp phân tích và thông tin, không đưa ra lời khuyên đầu tư
- Triển khai cơ chế hiển thị rõ ràng thông báo "Không cung cấp lời khuyên đầu tư"
- Đảm bảo tất cả phản hồi AI tuân thủ nguyên tắc thiết kế an toàn pháp lý
- Thiết lập cơ chế kiểm tra và xác nhận rằng không có chức năng giao dịch, dự đoán giá, hay phát tín hiệu mua/bán
- Tạo unit test để đảm bảo các ràng buộc pháp lý được tuân thủ

## Kết quả mong đợi
- Hệ thống AI hoạt động ổn định với nhà cung cấp bên ngoài
- Tất cả phản hồi đều tuân thủ nguyên tắc không tư vấn đầu tư
- Thông báo pháp lý hiển thị rõ ràng cho người dùng
- Hệ thống đảm bảo không vi phạm các ràng buộc pháp lý tài chính

## Công nghệ liên quan
- LangChain
- OpenAI/Claude API
- TypeScript
- Legal Compliance
- Unit Testing