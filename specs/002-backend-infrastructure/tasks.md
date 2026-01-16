# Nhiệm vụ: Backend Infrastructure cho Ứng dụng AI Chat với Kiến trúc A2A

**Tính năng**: Backend Infrastructure cho Ứng dụng AI Chat với Kiến trúc A2A  
**Tạo ngày**: 2026-01-16  
**Nhánh**: `002-backend-infrastructure`  
**Liên kết đặc tả**: [specs/002-backend-infrastructure/spec.md](spec.md)

## Chiến lược Triển khai

Việc triển khai này tuân theo phương pháp tiếp cận giao hàng tăng dần với phương pháp MVP-first. Việc triển khai được tổ chức thành các giai đoạn xây dựng dựa trên nhau, với mỗi câu chuyện người dùng có thể kiểm thử độc lập và triển khai được. Các yếu tố nền tảng được thiết lập trước tiên, sau đó là việc triển khai câu chuyện người dùng theo thứ tự ưu tiên (P1, P2, P3), và hoàn thiện với các mối quan tâm tổng thể.

### Phạm vi MVP
MVP sẽ tập trung vào Câu chuyện người dùng 1 (Khởi tạo Hệ thống và API khả dụng) và Câu chuyện người dùng 2 (Kết nối Cơ sở dữ liệu) để thiết lập cơ sở hạ tầng backend cốt lõi. Điều này cung cấp một backend tối thiểu nhưng chức năng có thể được kiểm thử độc lập.

### Phương pháp Giao hàng
- Giai đoạn 1: Các nhiệm vụ thiết lập (khởi tạo dự án và cấu hình cơ bản)
- Giai đoạn 2: Các nhiệm vụ nền tảng (cơ sở dữ liệu, xác thực, bảo mật - điều kiện tiên quyết chặn)
- Giai đoạn 3: Câu chuyện người dùng 1 (Khởi tạo Hệ thống - P1)
- Giai đoạn 4: Câu chuyện người dùng 2 (Kết nối Cơ sở dữ liệu - P1)
- Giai đoạn 5: Câu chuyện người dùng 3 (Xác thực - P2)
- Giai đoạn 6: Câu chuyện người dùng 4 (Quản lý Danh mục đầu tư - P2)
- Giai đoạn 7: Câu chuyện người dùng 5 (Ghi nhật ký và Xử lý Lỗi - P3)
- Giai đoạn 8: Hoàn thiện & Những mối quan tâm chéo (kiểm thử, tài liệu, triển khai)

## Phụ thuộc

### Thứ tự Hoàn thành Câu chuyện Người dùng
1. US1 (P1) - Khởi tạo Hệ thống: Cơ sở hạ tầng cho tất cả các câu chuyện khác
2. US2 (P1) - Kết nối Cơ sở dữ liệu: Điều kiện tiên quyết cho các câu chuyện phụ thuộc dữ liệu
3. US3 (P2) - Xác thực: Phụ thuộc vào cơ sở hạ tầng cơ bản và cơ sở dữ liệu
4. US4 (P2) - Quản lý Danh mục đầu tư: Phụ thuộc vào cơ sở dữ liệu và xác thực
5. US5 (P3) - Ghi nhật ký và Xử lý Lỗi: Có thể được thực hiện song song nhưng nâng cao tất cả các câu chuyện khác

### Đường dẫn Quan trọng
US1 → US2 → US3 → US4 (US5 chạy song song)

## Ví dụ Thực thi Song song

Mỗi câu chuyện người dùng chứa nhiều thành phần có thể được phát triển song song:
- US4 (Quản lý Danh mục đầu tư): Các thực thể, dịch vụ, điều khiển và DTO có thể được phát triển đồng thời
- US3 (Xác thực): Các thành phần đăng ký, đăng nhập và ủy quyền có thể được phát triển song song
- Kiểm thử: Các bài kiểm thử đơn vị có thể được viết song song với việc triển khai qua tất cả các câu chuyện

## Giai đoạn 1: Nhiệm vụ Thiết lập

- [x] T001 Tạo cấu trúc dự án theo kế hoạch triển khai
- [x] T002 Thiết lập cấu hình TypeScript cho backend
- [x] T003 Khởi tạo ứng dụng NestJS với các phụ thuộc cần thiết
- [x] T004 Cấu hình ESLint và Prettier cho định dạng mã nguồn
- [x] T005 Thiết lập quản lý cấu hình môi trường
- [x] T093 [P] Cập nhật các phụ thuộc trong package.json lên phiên bản mới nhất phù hợp (@nestjs/common 11.0.0+, typeorm 0.3.20+, mysql2 3.9.8+, bcrypt 5.1.1+)

## Giai đoạn 2: Nhiệm vụ Nền tảng

- [x] T006 Tạo cấu hình cơ sở dữ liệu với TypeORM và MySQL
- [x] T007 Tạo thực thể User với quy tắc xác thực theo mô hình dữ liệu
- [x] T008 Tạo thực thể Portfolio Item với quy tắc xác thực theo mô hình dữ liệu
- [x] T009 Tạo thực thể Chat Session với quy tắc xác thực theo mô hình dữ liệu
- [x] T010 Tạo thực thể Chat Message với quy tắc xác thực theo mô hình dữ liệu
- [x] T011 Tạo thực thể A2A Message với quy tắc xác thực theo mô hình dữ liệu
- [x] T012 Tạo thực thể Authentication Token với quy tắc xác thực theo mô hình dữ liệu
- [x] T013 Tạo thực thể Agent Registry với quy tắc xác thực theo mô hình dữ liệu
- [x] T014 Tạo thực thể Portfolio History với quy tắc xác thực theo mô hình dữ liệu
- [x] T015 Thiết lập di trú cơ sở dữ liệu cho tất cả các thực thể
- [x] T016 [P] Tạo endpoint kiểm tra sức khỏe (/health) theo hợp đồng API
- [x] T017 [P] Tạo module xác thực với chiến lược JWT theo quyết định nghiên cứu
- [x] T018 [P] Tạo các endpoint xác thực (/auth/register, /auth/login) theo hợp đồng API

## Giai đoạn 3: Câu chuyện Người dùng 1 - Khởi tạo Hệ thống và API Khả dụng (Ưu tiên: P1)

**Mục tiêu**: Người dùng có thể truy cập hệ thống backend thông qua các endpoint API ổn định và đáng tin cậy.

**Tiêu chí Kiểm thử Độc lập**: Có thể kiểm tra độc lập bằng cách gửi request đến các endpoint API cơ bản và xác nhận rằng hệ thống phản hồi đúng cách, không gặp lỗi kết nối hoặc crash.

**Kiểm thử (nếu được yêu cầu)**:
- [x] T019 [P] [US1] Tạo module và controller cơ bản theo kế hoạch
- [x] T020 [P] [US1] Triển khai endpoint kiểm tra sức khỏe với schema phản hồi đúng

## Giai đoạn 4: Câu chuyện Người dùng 2 - Kết nối và Quản lý Cơ sở dữ liệu (Ưu tiên: P1)

**Mục tiêu**: Hệ thống có thể kết nối đến cơ sở dữ liệu MySQL và thực hiện các thao tác lưu trữ dữ liệu tài chính cơ bản.

**Tiêu chí Kiểm thử Độc lập**: Có thể kiểm tra độc lập bằng cách thực hiện các thao tác CRUD (Create, Read, Update, Delete) lên một bảng dữ liệu mẫu và xác nhận dữ liệu được lưu trữ và truy xuất chính xác.

**Kiểm thử (nếu được yêu cầu)**:
- [x] T021 [P] [US2] Cấu hình kết nối cơ sở dữ liệu và nhóm kết nối
- [x] T022 [P] [US2] Tạo module dịch vụ cơ sở dữ liệu cho tích hợp TypeORM
- [x] T023 [P] [US2] Triển khai xác thực và mối quan hệ thực thể cơ sở dữ liệu

## Giai đoạn 5: Câu chuyện Người dùng 3 - Xác thực và Phân quyền Người dùng (Ưu tiên: P2)

**Mục tiêu**: Hệ thống có cơ chế xác thực người dùng cơ bản để bảo vệ dữ liệu tài chính cá nhân.

**Tiêu chí Kiểm thử Độc lập**: Có thể kiểm tra độc lập bằng cách thử truy cập các endpoint yêu cầu xác thực với token hợp lệ và không hợp lệ, xác nhận rằng chỉ có token hợp lệ mới được phép truy cập.

**Kiểm thử (nếu được yêu cầu)**:
- [x] T024 [P] [US3] Tạo guard xác thực cho các tuyến đường được bảo vệ
- [x] T025 [P] [US3] Triển khai xác thực JWT với xử lý token đúng cách
- [x] T026 [P] [US3] Thêm hashing mật khẩu với bcrypt cho bảo mật người dùng
- [x] T027 [P] [US3] Triển khai quản lý phiên cho xác thực người dùng

## Giai đoạn 6: Câu chuyện Người dùng 4 - Module Quản lý Danh mục Đầu tư (Ưu tiên: P2)

**Mục tiêu**: Hệ thống có module chuyên biệt để quản lý dữ liệu danh mục đầu tư theo schema chuẩn hóa.

**Tiêu chí Kiểm thử Độc lập**: Có thể kiểm tra độc lập bằng cách gửi dữ liệu danh mục đầu tư theo schema chuẩn hóa và xác nhận rằng hệ thống có thể lưu trữ, truy xuất và xử lý dữ liệu chính xác.

**Kiểm thử (nếu được yêu cầu)**:
- [x] T028 [P] [US4] Tạo cấu trúc module danh mục đầu tư theo kế hoạch
- [x] T029 [P] [US4] Triển khai các thao tác CRUD cho danh mục đầu tư theo hợp đồng API
- [x] T030 [P] [US4] Thêm xác thực quyền sở hữu người dùng cho các mục danh mục
- [x] T031 [P] [US4] Tạo dịch vụ phân tích danh mục đầu tư theo kế hoạch
- [x] T067 [P] [US4] Thêm tiện ích chuyển đổi tiền tệ cho quản lý danh mục đầu tư
- [x] T068 [P] [US4] Triển khai chức năng nhập/xuất dữ liệu cho danh mục đầu tư

## Giai đoạn 7: Câu chuyện Người dùng 5 - Ghi nhật ký và Xử lý Lỗi (Ưu tiên: P3)

**Mục tiêu**: Hệ thống ghi nhật ký (logging) các sự kiện quan trọng và xử lý lỗi một cách nhất quán.

**Tiêu chí Kiểm thử Độc lập**: Có thể kiểm tra độc lập bằng cách tạo các tình huống lỗi và xác nhận rằng hệ thống ghi log đúng cách và trả về thông báo lỗi phù hợp.

**Kiểm thử (nếu được yêu cầu)**:
- [x] T032 [P] [US5] Tạo bộ lọc ngoại lệ toàn cục cho xử lý lỗi nhất quán
- [x] T033 [P] [US5] Triển khai ghi nhật ký có cấu trúc cho các bản ghi kiểm toán
- [x] T034 [P] [US5] Thêm middleware ghi nhật ký để theo dõi request/response
- [x] T035 [P] [US5] Chuẩn hóa các phản hồi lỗi theo hợp đồng API
- [x] T065 [P] [US5] Thêm ghi nhật ký tuân thủ cho dữ liệu tài chính
- [x] T066 [P] [US5] Triển khai theo dõi và báo cáo lỗi

## Giai đoạn 8: Các Nhiệm vụ Triển khai Bổ sung

### Kiến trúc A2A (Agent-to-Agent)
- [x] T036 [P] Tạo dịch vụ điều phối A2A theo quyết định nghiên cứu
- [x] T037 [P] Tạo định dạng thông điệp chuẩn hóa cho giao tiếp tác nhân
- [x] T038 [P] Triển khai endpoint giao tiếp A2A theo hợp đồng API
- [x] T039 [P] Tạo sổ đăng ký tác nhân để theo dõi các tác nhân khả dụng
- [x] T040 [P] Triển khai ID tương quan để theo dõi luồng tin nhắn
- [x] T041 [P] Thêm xử lý lỗi cho sự cố giao tiếp tác nhân

### Triển khai Dịch vụ Chat
- [x] T042 [P] Tạo cấu trúc module chat theo kế hoạch
- [x] T043 [P] Triển khai quản lý phiên chat
- [x] T044 [P] Tạo chức năng lưu trữ và truy xuất tin nhắn
- [x] T045 [P] Triển khai Sự kiện được gửi từ máy chủ (SSE) cho truyền tải luồng dữ liệu theo quyết định nghiên cứu
- [x] T046 [P] Tạo các endpoint chat (/chat/send, /chat/stream) theo hợp đồng API
- [x] T047 [P] Tích hợp với nhà cung cấp AI cho phản hồi chat
- [x] T048 [P] Triển khai theo dõi lịch sử tin nhắn
- [x] T049 [P] Tạo chức năng duy trì phiên

### Triển khai Bảo mật
- [x] T050 [P] Thêm xác thực và làm sạch đầu vào cho bảo mật
- [x] T051 [P] Cấu hình giới hạn tốc độ cho các endpoint API
- [x] T052 [P] Triển khai các biện pháp bảo mật (ngăn chặn SQL injection, bảo vệ XSS)
- [x] T053 [P] Thêm cấu hình header bảo mật

### Tài liệu
- [x] T054 [P] Tạo tài liệu API với OpenAPI/Swagger
- [x] T055 [P] Tạo hướng dẫn bắt đầu nhanh cho cơ sở hạ tầng backend
- [x] T056 [P] Tài liệu mô hình dữ liệu với định nghĩa trường và mối quan hệ
- [x] T057 [P] Tạo tài liệu kiến trúc cho các module và dịch vụ
- [x] T058 [P] Tạo hướng dẫn thiết lập môi trường theo yêu cầu bắt đầu nhanh
- [x] T059 [P] Tạo tài liệu triển khai

### Cấu hình Triển khai
- [x] T060 [P] Cấu hình các tệp Docker cho dịch vụ backend
- [x] T061 [P] Tạo script xây dựng sản phẩm
- [x] T062 [P] Tạo script di trú cơ sở dữ liệu cho triển khai
- [x] T063 [P] Thêm endpoint kiểm tra sức khỏe cho điều phối
- [x] T064 [P] Cấu hình SSL/TLS cho môi trường sản xuất
### Hiệu suất và Giám sát
- [x] T069 [P] [US5] Triển khai điểm chuẩn và giám sát hiệu suất
- [x] T070 [P] Tối ưu hóa truy vấn cơ sở dữ liệu cho hiệu suất tốt hơn
- [x] T071 [P] Thêm giám sát thời gian phản hồi API
- [x] T072 [P] Triển khai giám sát sử dụng bộ nhớ và CPU
- [x] T073 [P] Thêm giám sát kết nối cơ sở dữ liệu
- [x] T074 [P] Triển khai bộ nhớ đệm nơi thích hợp cho hiệu suất


### Đánh giá Tuân thủ và Bảo mật
- [x] T075 [P] Thực hiện xác minh tuân thủ SOC 2
- [x] T076 [P] Xác minh tuân thủ xử lý dữ liệu tài chính
- [x] T077 [P] Xác nhận thực thi không có lời khuyên đầu tư
- [x] T078 [P] Thực thi xử lý dữ liệu tài chính chỉ đọc
- [x] T079 [P] Xác minh tuân thủ quyền riêng tư dữ liệu
- [x] T080 [P] Xác minh triển khai bản ghi kiểm toán

## Giai đoạn 9: Kiểm thử & Đảm bảo Chất lượng

- [x] T081 [P] Tạo các bài kiểm thử đơn vị cho tất cả các dịch vụ (độ bao phủ 80%+)
- [x] T082 [P] Tạo các bài kiểm thử tích hợp cho các endpoint API
- [x] T083 [P] Tạo các bài kiểm thử E2E cho các luồng người dùng quan trọng
- [x] T084 [P] Tạo các bài kiểm thử tích hợp cơ sở dữ liệu
- [x] T085 [P] Tạo các bài kiểm thử giao tiếp A2A
- [x] T086 [P] Tạo các bài kiểm thử điều kiện lỗi

## Giai đoạn 10: Xác minh Cuối cùng

- [x] T087 [P] Xác minh tất cả các bài kiểm thử đơn vị đều vượt qua
- [x] T088 [P] Xác minh tất cả các bài kiểm thử tích hợp đều vượt qua
- [x] T089 [P] Xác minh tuân thủ hợp đồng API
- [x] T090 [P] Xác minh các yêu cầu hiệu suất được đáp ứng
- [x] T091 [P] Chạy quét bảo mật và xác minh nó vượt qua
- [x] T092 [P] Hoàn thành quy trình xem xét mã nguồn