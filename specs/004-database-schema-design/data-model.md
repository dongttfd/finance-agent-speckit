# Data Model: Thiết kế lược đồ cơ sở dữ liệu cho hệ thống Finance Agent

## Tổng quan
Tài liệu này mô tả chi tiết các entity, thuộc tính, mối quan hệ và ràng buộc của lược đồ cơ sở dữ liệu cho hệ thống Finance Agent dựa trên các yêu cầu trong spec.md.

## Entity: User
### Mô tả
Đại diện cho người dùng hệ thống, chứa thông tin xác thực và hồ sơ cá nhân.

### Thuộc tính
| Trường | Kiểu dữ liệu | Ràng buộc | Mô tả |
|--------|-------------|-----------|-------|
| id | UUID | PK, Not Null | ID duy nhất cho người dùng |
| email | VARCHAR(255) | Unique, Not Null | Email đăng ký tài khoản |
| passwordHash | VARCHAR(255) | Not Null | Mật khẩu đã được hash |
| firstName | VARCHAR(100) | Nullable | Tên người dùng |
| lastName | VARCHAR(100) | Nullable | Họ người dùng |
| isActive | BOOLEAN | Not Null, Default: true | Trạng thái hoạt động |
| createdAt | TIMESTAMP | Not Null | Thời gian tạo tài khoản |
| updatedAt | TIMESTAMP | Not Null | Thời gian cập nhật gần nhất |

### Ràng buộc
- Email phải là duy nhất (UNIQUE)
- Email phải hợp lệ theo định dạng email
- Mật khẩu phải được hash trước khi lưu trữ

## Entity: AuthToken
### Mô tả
Đại diện cho token xác thực được sử dụng để duy trì phiên làm việc.

### Thuộc tính
| Trường | Kiểu dữ liệu | Ràng buộc | Mô tả |
|--------|-------------|-----------|-------|
| id | UUID | PK, Not Null | ID duy nhất cho token |
| userId | UUID | FK, Not Null | ID người dùng sở hữu token |
| token | VARCHAR(500) | Unique, Not Null | Giá trị token |
| tokenType | ENUM('BEARER', 'REFRESH') | Not Null | Loại token |
| expiresAt | TIMESTAMP | Not Null | Thời gian hết hạn |
| isRevoked | BOOLEAN | Not Null, Default: false | Trạng thái hủy bỏ |
| createdAt | TIMESTAMP | Not Null | Thời gian tạo token |
| updatedAt | TIMESTAMP | Not Null | Thời gian cập nhật gần nhất |

### Ràng buộc
- Token phải là duy nhất (UNIQUE)
- Liên kết với User thông qua userId (FK)
- Token không thể được sử dụng sau khi bị revoke

## Entity: PortfolioItem
### Mô tả
Đại diện cho một tài sản đầu tư trong danh mục, bao gồm loại tài sản, mã, số lượng, giá gốc, giá trị hiện tại, đơn vị tiền tệ và ngày mua.

### Thuộc tính
| Trường | Kiểu dữ liệu | Ràng buộc | Mô tả |
|--------|-------------|-----------|-------|
| id | UUID | PK, Not Null | ID duy nhất cho tài sản |
| userId | UUID | FK, Not Null | ID người dùng sở hữu tài sản |
| assetType | ENUM('STOCK', 'BOND', 'CRYPTO', 'ETF', 'FUND', 'CASH', 'OTHER') | Not Null | Loại tài sản |
| symbol | VARCHAR(20) | Not Null | Mã chứng khoán/tài sản |
| name | VARCHAR(255) | Not Null | Tên đầy đủ của tài sản |
| quantity | DECIMAL(15,6) | Not Null | Số lượng tài sản |
| costBasis | DECIMAL(15,6) | Not Null | Giá gốc (giá mua trung bình) |
| currency | CHAR(3) | Not Null | Mã tiền tệ theo chuẩn ISO 4217 |
| purchaseDate | DATE | Nullable | Ngày mua tài sản |
| currentValue | DECIMAL(15,6) | Nullable | Giá trị hiện tại |
| currentPrice | DECIMAL(15,6) | Nullable | Giá hiện tại mỗi đơn vị |
| createdAt | TIMESTAMP | Not Null | Thời gian tạo bản ghi |
| updatedAt | TIMESTAMP | Not Null | Thời gian cập nhật gần nhất |

### Ràng buộc
- Liên kết với User thông qua userId (FK)
- Số lượng phải lớn hơn 0
- Giá gốc phải lớn hơn hoặc bằng 0
- Tiền tệ phải theo chuẩn ISO 4217

## Entity: PortfolioHistory
### Mô tả
Đại diện cho lịch sử giá trị của tài sản theo thời gian, bao gồm ID tài sản, giá trị, ngày ghi nhận và nguồn dữ liệu.

### Thuộc tính
| Trường | Kiểu dữ liệu | Ràng buộc | Mô tả |
|--------|-------------|-----------|-------|
| id | UUID | PK, Not Null | ID duy nhất cho bản ghi lịch sử |
| portfolioItemId | UUID | FK, Not Null | ID của tài sản liên quan |
| value | DECIMAL(15,6) | Not Null | Giá trị tài sản tại thời điểm ghi nhận |
| price | DECIMAL(15,6) | Not Null | Giá mỗi đơn vị tại thời điểm ghi nhận |
| dateRecorded | DATE | Not Null | Ngày ghi nhận giá trị |
| source | VARCHAR(100) | Nullable | Nguồn dữ liệu |
| createdAt | TIMESTAMP | Not Null | Thời gian tạo bản ghi |

### Ràng buộc
- Liên kết với PortfolioItem thông qua portfolioItemId (FK)
- Giá trị và giá phải lớn hơn hoặc bằng 0
- Ngày ghi nhận không thể trong tương lai

## Entity: ChatSession
### Mô tả
Đại diện cho một phiên hội thoại giữa người dùng và AI Assistant, bao gồm tiêu đề, trạng thái, thời gian tạo/cập nhật và danh sách tin nhắn.

### Thuộc tính
| Trường | Kiểu dữ liệu | Ràng buộc | Mô tả |
|--------|-------------|-----------|-------|
| id | UUID | PK, Not Null | ID duy nhất cho phiên chat |
| userId | UUID | FK, Not Null | ID người dùng sở hữu phiên chat |
| title | VARCHAR(255) | Nullable | Tiêu đề phiên chat |
| status | ENUM('ACTIVE', 'ARCHIVED', 'DELETED') | Not Null, Default: 'ACTIVE' | Trạng thái phiên chat |
| metadata | JSON | Nullable | Siêu dữ liệu bổ sung |
| createdAt | TIMESTAMP | Not Null | Thời gian tạo phiên chat |
| updatedAt | TIMESTAMP | Not Null | Thời gian cập nhật gần nhất |

### Ràng buộc
- Liên kết với User thông qua userId (FK)
- Trạng thái phải là một trong các giá trị được định nghĩa

## Entity: ChatMessage
### Mô tả
Đại diện cho một tin nhắn trong phiên hội thoại, bao gồm loại người gửi (người dùng, tác nhân, hệ thống), nội dung, siêu dữ liệu và thời gian tạo.

### Thuộc tính
| Trường | Kiểu dữ liệu | Ràng buộc | Mô tả |
|--------|-------------|-----------|-------|
| id | UUID | PK, Not Null | ID duy nhất cho tin nhắn |
| chatSessionId | UUID | FK, Not Null | ID phiên chat chứa tin nhắn |
| senderType | ENUM('USER', 'AGENT', 'SYSTEM') | Not Null | Loại người gửi |
| senderId | UUID | Nullable | ID của người gửi (nếu có) |
| content | TEXT | Not Null | Nội dung tin nhắn |
| messageType | ENUM('TEXT', 'COMMAND', 'NOTIFICATION', 'DATA') | Not Null | Loại tin nhắn |
| metadata | JSON | Nullable | Siêu dữ liệu bổ sung |
| createdAt | TIMESTAMP | Not Null | Thời gian tạo tin nhắn |

### Ràng buộc
- Liên kết với ChatSession thông qua chatSessionId (FK)
- Nội dung không được rỗng
- Nếu senderType là USER, senderId phải là ID của User hợp lệ
- Nếu senderType là AGENT, senderId phải là ID của Agent hợp lệ

## Entity: AgentRegistry
### Mô tả
Đại diện cho đăng ký các Agent đang hoạt động trong hệ thống, bao gồm tên, mô tả, điểm cuối API, trạng thái hoạt động và khả năng hỗ trợ.

### Thuộc tính
| Trường | Kiểu dữ liệu | Ràng buộc | Mô tả |
|--------|-------------|-----------|-------|
| id | UUID | PK, Not Null | ID duy nhất cho agent |
| name | VARCHAR(255) | Not Null | Tên của agent |
| description | TEXT | Nullable | Mô tả chức năng của agent |
| endpointUrl | VARCHAR(500) | Not Null | Đường dẫn API của agent |
| isActive | BOOLEAN | Not Null, Default: true | Trạng thái hoạt động |
| capabilities | JSON | Nullable | Danh sách khả năng của agent |
| lastHeartbeat | TIMESTAMP | Nullable | Thời gian heartbeat gần nhất |
| createdAt | TIMESTAMP | Not Null | Thời gian đăng ký agent |
| updatedAt | TIMESTAMP | Not Null | Thời gian cập nhật gần nhất |

### Ràng buộc
- Tên phải là duy nhất (UNIQUE)
- Endpoint URL phải là URL hợp lệ
- Agent có thể được kích hoạt hoặc vô hiệu hóa

## Entity: A2AMessage
### Mô tả
Đại diện cho một thông điệp giao tiếp giữa các Agent trong hệ thống A2A, bao gồm ID tác nhân nguồn/đích, loại thông điệp, hành động, tải trọng dữ liệu và trạng thái xử lý.

### Thuộc tính
| Trường | Kiểu dữ liệu | Ràng buộc | Mô tả |
|--------|-------------|-----------|-------|
| id | UUID | PK, Not Null | ID duy nhất cho thông điệp |
| sourceAgentId | UUID | FK, Not Null | ID của agent nguồn |
| destinationAgentId | UUID | FK, Not Null | ID của agent đích |
| messageType | ENUM('REQUEST', 'RESPONSE', 'NOTIFICATION', 'HEARTBEAT') | Not Null | Loại thông điệp |
| action | VARCHAR(255) | Not Null | Hành động được yêu cầu |
| payload | JSON | Not Null | Dữ liệu tải trọng |
| status | ENUM('PENDING', 'PROCESSING', 'COMPLETED', 'FAILED') | Not Null, Default: 'PENDING' | Trạng thái xử lý |
| correlationId | UUID | Nullable | ID để theo dõi luồng yêu cầu |
| errorMessage | TEXT | Nullable | Thông báo lỗi nếu có |
| processedAt | TIMESTAMP | Nullable | Thời gian xử lý hoàn tất |
| createdAt | TIMESTAMP | Not Null | Thời gian tạo thông điệp |

### Ràng buộc
- Liên kết với AgentRegistry cho cả sourceAgentId và destinationAgentId (FK)
- Payload phải là JSON hợp lệ
- Nếu là RESPONSE, phải có correlationId tham chiếu đến REQUEST ban đầu

## Mối quan hệ giữa các Entity

### Quan hệ 1-N (One-to-Many)
1. **User → PortfolioItem**: Một người dùng có thể có nhiều tài sản đầu tư
2. **User → ChatSession**: Một người dùng có thể có nhiều phiên chat
3. **User → AuthToken**: Một người dùng có thể có nhiều token xác thực
4. **ChatSession → ChatMessage**: Một phiên chat có thể chứa nhiều tin nhắn
5. **PortfolioItem → PortfolioHistory**: Một tài sản có thể có nhiều bản ghi lịch sử
6. **AgentRegistry → A2AMessage (source)**: Một agent có thể gửi nhiều thông điệp
7. **AgentRegistry → A2AMessage (destination)**: Một agent có thể nhận nhiều thông điệp

### Quan hệ N-N (Many-to-Many)
Không có mối quan hệ N-N trực tiếp trong mô hình này, tất cả các mối quan hệ N-N sẽ được chuyển đổi sang các quan hệ 1-N thông qua bảng trung gian nếu cần.

## Ràng buộc toàn vẹn dữ liệu
1. **Referential Integrity**: Tất cả các khóa ngoại đều có ràng buộc tham chiếu đến bảng cha
2. **Data Validation**: Tất cả các trường đều có ràng buộc xác thực dữ liệu phù hợp
3. **Domain Constraints**: Các trường enum chỉ chấp nhận các giá trị được định nghĩa
4. **Business Rules**: Các ràng buộc nghiệp vụ được áp dụng thông qua các trigger hoặc kiểm tra ở tầng ứng dụng

## Chỉ mục (Indexes)
1. **Primary Indexes**: Tự động tạo cho các trường khóa chính
2. **Unique Indexes**: 
   - User.email
   - AuthToken.token
   - AgentRegistry.name
3. **Composite Indexes**:
   - PortfolioItem.userId + assetType (cho truy vấn danh mục theo loại tài sản)
   - ChatSession.userId + status (cho truy vấn phiên chat theo người dùng và trạng thái)
   - PortfolioHistory.portfolioItemId + dateRecorded (cho truy vấn lịch sử theo thời gian)
4. **Regular Indexes**:
   - User.createdAt (cho truy vấn theo thời gian tạo tài khoản)
   - ChatMessage.chatSessionId + createdAt (cho truy vấn tin nhắn theo phiên và thời gian)
   - A2AMessage.status + createdAt (cho truy vấn thông điệp theo trạng thái và thời gian)

## Ghi chú thiết kế
1. **UUIDs**: Tất cả các ID đều sử dụng UUID để đảm bảo tính duy nhất toàn cục và dễ dàng tích hợp trong môi trường phân tán
2. **Timestamps**: Tất cả các entity đều có createdAt và updatedAt để theo dõi lịch sử thay đổi
3. **Soft Deletes**: Thay vì xóa vật lý, các entity sử dụng trường trạng thái để đánh dấu đã xóa (ví dụ: User.isActive, ChatSession.status)
4. **JSON Fields**: Sử dụng JSON để lưu trữ dữ liệu linh hoạt như metadata, capabilities, payload mà không cần xác định cấu trúc cố định
5. **Security**: Thiết kế đảm bảo rằng người dùng chỉ có thể truy cập dữ liệu thuộc về họ thông qua các ràng buộc FK và kiểm tra ở tầng ứng dụng