# Data Model: Backend Infrastructure for AI Chat Application with A2A Architecture

## Entity: User
**Description**: Đại diện cho người dùng hệ thống, có thông tin xác thực và quyền truy cập dữ liệu danh mục của chính họ

### Fields:
- `id`: UUID - Unique identifier cho người dùng
- `email`: String - Email của người dùng (unique)
- `passwordHash`: String - Hash của mật khẩu người dùng
- `firstName`: String - Tên của người dùng
- `lastName`: String - Họ của người dùng
- `createdAt`: DateTime - Thời điểm tạo tài khoản
- `updatedAt`: DateTime - Thời điểm cập nhật tài khoản lần cuối
- `lastLoginAt`: DateTime - Thời điểm đăng nhập lần cuối
- `isActive`: Boolean - Trạng thái hoạt động của người dùng

### Relationships:
- One User has many Portfolio Items
- One User has many Chat Sessions
- One User has many Authentication Tokens

### Validation Rules:
- Email must be valid email format and unique
- Password must meet security requirements (min length, complexity)
- First name and last name must not be empty
- isActive defaults to true

---

## Entity: Portfolio Item
**Description**: Đại diện cho một khoản đầu tư trong danh mục tài chính cá nhân

### Fields:
- `id`: UUID - Unique identifier cho khoản đầu tư
- `userId`: UUID - Foreign key liên kết đến người dùng sở hữu
- `assetType`: Enum('stock', 'fund', 'crypto', 'cash', 'bond', 'reit') - Loại tài sản
- `symbol`: String - Mã biểu tượng của tài sản (e.g., AAPL, BTCUSD)
- `name`: String - Tên đầy đủ của tài sản
- `quantity`: Decimal - Số lượng tài sản
- `costBasis`: Decimal - Giá gốc của tài sản (đơn vị tiền tệ)
- `currentValue`: Decimal - Giá trị hiện tại của tài sản (đơn vị tiền tệ)
- `currency`: String - Đơn vị tiền tệ (e.g., USD, EUR, VND)
- `purchaseDate`: Date - Ngày mua tài sản
- `createdAt`: DateTime - Thời điểm tạo bản ghi
- `updatedAt`: DateTime - Thời điểm cập nhật bản ghi lần cuối

### Relationships:
- One Portfolio Item belongs to one User
- One Portfolio Item has many Portfolio History Records

### Validation Rules:
- assetType must be one of the defined enum values
- quantity must be greater than 0
- costBasis and currentValue must be non-negative
- currency must be valid ISO 4217 currency code
- userId must reference an existing User

---

## Entity: Chat Session
**Description**: Đại diện cho một phiên trò chuyện giữa người dùng và AI chat agent

### Fields:
- `id`: UUID - Unique identifier cho phiên chat
- `userId`: UUID - Foreign key liên kết đến người dùng
- `title`: String - Tiêu đề của phiên chat (tự động tạo từ nội dung đầu tiên)
- `status`: Enum('active', 'completed', 'archived') - Trạng thái của phiên chat
- `createdAt`: DateTime - Thời điểm tạo phiên chat
- `updatedAt`: DateTime - Thời điểm cập nhật phiên chat lần cuối
- `lastMessageAt`: DateTime - Thời điểm gửi tin nhắn cuối cùng

### Relationships:
- One Chat Session belongs to one User
- One Chat Session has many Chat Messages

### Validation Rules:
- userId must reference an existing User
- status must be one of the defined enum values
- title must not be empty

---

## Entity: Chat Message
**Description**: Đại diện cho một tin nhắn trong phiên trò chuyện

### Fields:
- `id`: UUID - Unique identifier cho tin nhắn
- `sessionId`: UUID - Foreign key liên kết đến phiên chat
- `senderType`: Enum('user', 'agent', 'system') - Loại người gửi
- `senderId`: UUID - ID của người gửi (user id hoặc agent id)
- `content`: Text - Nội dung tin nhắn
- `metadata`: JSON - Metadata bổ sung (role, tokens sử dụng, v.v.)
- `createdAt`: DateTime - Thời điểm tạo tin nhắn

### Relationships:
- One Chat Message belongs to one Chat Session
- One Chat Message was sent by one User or Agent

### Validation Rules:
- sessionId must reference an existing Chat Session
- senderType must be one of the defined enum values
- content must not be empty
- createdAt must be greater than or equal to session creation time

---

## Entity: A2A Message
**Description**: Đại diện cho một tin nhắn trong giao tiếp Agent-to-Agent

### Fields:
- `id`: UUID - Unique identifier cho tin nhắn A2A
- `sourceAgentId`: String - ID của agent gửi
- `targetAgentId`: String - ID của agent nhận
- `messageType`: String - Loại tin nhắn (request, response, notification)
- `action`: String - Hành động cần thực hiện
- `payload`: JSON - Payload của tin nhắn
- `correlationId`: String - ID để theo dõi luồng tin nhắn
- `status`: Enum('pending', 'processing', 'completed', 'failed') - Trạng thái xử lý
- `createdAt`: DateTime - Thời điểm tạo tin nhắn
- `processedAt`: DateTime - Thời điểm xử lý xong

### Relationships:
- One A2A Message may belong to one Chat Session (for traceability)

### Validation Rules:
- sourceAgentId and targetAgentId must be valid
- messageType must be one of allowed values
- correlationId should follow UUID format
- status must be one of the defined enum values

---

## Entity: Authentication Token
**Description**: Đại diện cho phiên xác thực của người dùng, cho phép truy cập các tài nguyên được bảo vệ

### Fields:
- `id`: UUID - Unique identifier cho token
- `userId`: UUID - Foreign key liên kết đến người dùng
- `token`: String - JWT token string
- `type`: Enum('access', 'refresh') - Loại token
- `expiresAt`: DateTime - Thời điểm token hết hạn
- `createdAt`: DateTime - Thời điểm tạo token
- `revokedAt`: DateTime - Thời điểm token bị hủy (nếu có)
- `isRevoked`: Boolean - Trạng thái hủy token

### Relationships:
- One Authentication Token belongs to one User

### Validation Rules:
- userId must reference an existing User
- token must be valid JWT format
- type must be one of the defined enum values
- expiresAt must be in the future
- isRevoked defaults to false

---

## Entity: Agent Registry
**Description**: Đại diện cho registry của các A2A agents trong hệ thống

### Fields:
- `id`: UUID - Unique identifier cho agent
- `name`: String - Tên của agent
- `description`: Text - Mô tả chức năng của agent
- `endpoint`: String - URL endpoint của agent
- `status`: Enum('active', 'inactive', 'maintenance') - Trạng thái hoạt động
- `capabilities`: Array<String> - Danh sách khả năng của agent
- `createdAt`: DateTime - Thời điểm đăng ký agent
- `updatedAt`: DateTime - Thời điểm cập nhật thông tin lần cuối

### Relationships:
- One Agent Registry may participate in many A2A Messages

### Validation Rules:
- name must be unique
- endpoint must be valid URL format
- status must be one of the defined enum values
- capabilities must be non-empty array

---

## Entity: Portfolio History
**Description**: Đại diện cho lịch sử thay đổi giá trị của một khoản đầu tư

### Fields:
- `id`: UUID - Unique identifier cho bản ghi lịch sử
- `portfolioItemId`: UUID - Foreign key liên kết đến khoản đầu tư
- `value`: Decimal - Giá trị tại thời điểm ghi nhận
- `date`: Date - Ngày ghi nhận giá trị
- `source`: String - Nguồn dữ liệu (manual, api, calculation)
- `createdAt`: DateTime - Thời điểm tạo bản ghi

### Relationships:
- One Portfolio History belongs to one Portfolio Item

### Validation Rules:
- portfolioItemId must reference an existing Portfolio Item
- value must be non-negative
- date must be valid date format