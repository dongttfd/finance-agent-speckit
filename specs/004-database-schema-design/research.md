# Research: Thiết kế lược đồ cơ sở dữ liệu

## Mục tiêu nghiên cứu
Nghiên cứu và xác định các yêu cầu kỹ thuật, mô hình dữ liệu và phương pháp triển khai cho thiết kế lược đồ cơ sở dữ liệu của hệ thống Finance Agent.

## Quyết định: Sử dụng TypeORM với MySQL
### Lý do chọn:
- Theo yêu cầu cụ thể trong spec.md (FR-011): "Hệ thống PHẢI sử dụng MySQL làm cơ sở dữ liệu chính với TypeORM làm ORM layer"
- TypeORM cung cấp hỗ trợ TypeScript tốt, phù hợp với stack NestJS/TypeScript
- Có hỗ trợ migrations, giúp quản lý schema evolution
- Hỗ trợ các pattern như Repository và Active Record
- Phù hợp với kiến trúc hiện tại của dự án

### Các lựa chọn thay thế đã cân nhắc:
- **Sequelize**: Một ORM khác cho Node.js, nhưng TypeORM có hỗ trợ TypeScript tốt hơn
- **Prisma**: Hiện đại và có nhiều tính năng, nhưng có thể phức tạp hơn mức cần thiết
- **Raw SQL queries**: Không sử dụng ORM, nhưng sẽ làm tăng độ phức tạp và giảm maintainability
- **MongoDB với Mongoose**: Không phù hợp vì spec yêu cầu MySQL (SQL database)

## Quyết định: Mô hình hóa dữ liệu theo các entity chính
### Lý do chọn:
- Dựa trên các Key Entities được xác định trong spec.md
- Tuân theo nguyên tắc thiết kế hướng đối tượng và mối quan hệ giữa các thực thể
- Đảm bảo tính toàn vẹn dữ liệu thông qua ràng buộc khóa ngoại (FR-010)

### Các entity chính:
1. **User**: Quản lý xác thực người dùng
2. **PortfolioItem**: Quản lý tài sản đầu tư cá nhân
3. **ChatSession**: Quản lý phiên hội thoại
4. **ChatMessage**: Quản lý tin nhắn trong phiên chat
5. **A2AMessage**: Quản lý giao tiếp giữa các agent
6. **AuthToken**: Quản lý token xác thực
7. **AgentRegistry**: Quản lý thông tin các agent
8. **PortfolioHistory**: Quản lý lịch sử giá trị tài sản

## Quyết định: Bảo mật dữ liệu
### Lý do chọn:
- Spec yêu cầu mã hóa thông tin nhạy cảm (FR-013)
- Cần đảm bảo phân quyền người dùng (FR-012)
- Tuân thủ các yêu cầu bảo mật tài chính

### Triển khai:
- Mật khẩu sẽ được hash sử dụng bcrypt hoặc tương đương
- Token xác thực sẽ được lưu trữ an toàn
- Các thông tin nhạy cảm sẽ được mã hóa tại tầng ứng dụng nếu cần

## Quyết định: Hiệu suất và mở rộng
### Lý do chọn:
- Spec yêu cầu hỗ trợ 10,000+ người dùng đồng thời
- Yêu cầu hiệu suất cụ thể: 95% truy vấn dưới 500ms

### Triển khai:
- Chỉ mục (indexes) sẽ được tạo cho các trường thường xuyên được tìm kiếm
- Sẽ sử dụng connection pooling
- Thiết kế schema tối ưu cho các truy vấn phổ biến
- Hỗ trợ phân trang cho dữ liệu lớn (FR-014)

## Quyết định: Quản lý schema và migrations
### Lý do chọn:
- Spec yêu cầu xử lý schema evolution
- Cần đảm bảo tính nhất quán giữa các môi trường
- Hỗ trợ phát triển team và deployment tự động

### Triển khai:
- Sử dụng hệ thống migrations của TypeORM
- Version control cho các thay đổi schema
- Backup và rollback procedures

## Những điểm cần lưu ý trong quá trình triển khai:
1. **Quản lý quan hệ giữa các entity**: Đặc biệt là giữa User và các entity khác để đảm bảo quyền riêng tư dữ liệu
2. **Tối ưu hiệu suất**: Đảm bảo các truy vấn phức tạp vẫn đạt được hiệu suất yêu cầu
3. **Bảo mật dữ liệu tài chính**: Tuân thủ các yêu cầu bảo mật trong ngành tài chính
4. **Tính toàn vẹn dữ liệu**: Áp dụng các ràng buộc phù hợp để đảm bảo dữ liệu hợp lệ
5. **Khả năng mở rộng**: Thiết kế schema có thể mở rộng trong tương lai