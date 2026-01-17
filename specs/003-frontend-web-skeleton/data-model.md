# Data Model: Thiết lập giao diện frontend Web cho ứng dụng AI Chat Agent-to-Agent trong lĩnh vực tài chính

## Mô tả tổng quan

Tài liệu này định nghĩa các mô hình dữ liệu phía frontend cho ứng dụng AI Chat Agent-to-Agent trong lĩnh vực tài chính. Dữ liệu frontend chủ yếu là các kiểu dữ liệu TypeScript dùng để định nghĩa cấu trúc cho các thành phần UI và các đối tượng được truyền giữa frontend và backend.

## Các thực thể chính

### 1. ChatMessage

**Mô tả**: Đại diện cho một tin nhắn trong cuộc trò chuyện giữa người dùng và hệ thống AI/A2A agents

**Trường dữ liệu**:
- id: string - Định danh duy nhất cho tin nhắn
- content: string - Nội dung của tin nhắn
- sender: 'user' | 'agent' | 'system' - Người gửi tin nhắn (người dùng, agent hoặc hệ thống)
- timestamp: Date - Thời điểm gửi tin nhắn
- status: 'sending' | 'sent' | 'delivered' | 'error' - Trạng thái của tin nhắn
- metadata?: object - Dữ liệu bổ sung về tin nhắn (tùy chọn)

**Validation rules**:
- id là bắt buộc và phải là chuỗi không rỗng
- content là bắt buộc và phải có độ dài từ 1 đến 10000 ký tự
- sender là bắt buộc và phải là một trong ba giá trị: 'user', 'agent', hoặc 'system'
- timestamp là bắt buộc và phải là một ngày hợp lệ
- status là bắt buộc và phải là một trong bốn giá trị được liệt kê

### 2. ChatSession

**Mô tả**: Đại diện cho một phiên trò chuyện giữa người dùng và hệ thống AI/A2A agents

**Trường dữ liệu**:
- id: string - Định danh duy nhất cho phiên chat
- userId: string - ID của người dùng sở hữu phiên chat
- title: string - Tiêu đề của phiên chat (tự động tạo từ nội dung tin nhắn đầu tiên hoặc do người dùng đặt)
- createdAt: Date - Thời điểm tạo phiên chat
- updatedAt: Date - Thời điểm cập nhật gần nhất
- isActive: boolean - Trạng thái hoạt động của phiên chat

**Validation rules**:
- id là bắt buộc và phải là chuỗi không rỗng
- userId là bắt buộc và phải là chuỗi không rỗng
- title là bắt buộc và phải có độ dài từ 1 đến 200 ký tự
- createdAt là bắt buộc và phải là một ngày hợp lệ
- updatedAt là bắt buộc và phải là một ngày hợp lệ
- isActive là bắt buộc và phải là boolean

### 3. User

**Mô tả**: Đại diện cho người dùng của ứng dụng

**Trường dữ liệu**:
- id: string - Định danh duy nhất cho người dùng
- username: string - Tên đăng nhập của người dùng
- email: string - Địa chỉ email của người dùng
- firstName?: string - Tên người dùng (tùy chọn)
- lastName?: string - Họ người dùng (tùy chọn)
- createdAt: Date - Thời điểm tạo tài khoản
- preferences: UserPreferences - Thiết lập tùy chọn của người dùng

**Validation rules**:
- id là bắt buộc và phải là chuỗi không rỗng
- username là bắt buộc và phải có độ dài từ 3 đến 50 ký tự
- email là bắt buộc và phải là địa chỉ email hợp lệ
- createdAt là bắt buộc và phải là một ngày hợp lệ
- preferences là bắt buộc và phải là một đối tượng UserPreferences hợp lệ

### 4. UserPreferences

**Mô tả**: Đại diện cho các tùy chọn người dùng

**Trường dữ liệu**:
- theme: 'light' | 'dark' | 'auto' - Chế độ hiển thị giao diện
- notificationsEnabled: boolean - Cho phép thông báo
- fontSize: 'small' | 'medium' | 'large' - Kích thước chữ
- enableTypingIndicator: boolean - Hiển thị dấu hiệu đánh máy

**Validation rules**:
- theme là bắt buộc và phải là một trong ba giá trị được liệt kê
- notificationsEnabled là bắt buộc và phải là boolean
- fontSize là bắt buộc và phải là một trong ba giá trị được liệt kê
- enableTypingIndicator là bắt buộc và phải là boolean

### 5. AgentStatus

**Mô tả**: Đại diện cho trạng thái của các A2A agents

**Trường dữ liệu**:
- id: string - Định danh duy nhất cho agent
- name: string - Tên của agent
- status: 'idle' | 'busy' | 'offline' - Trạng thái hoạt động của agent
- lastActive: Date - Thời điểm hoạt động gần nhất
- capabilities: string[] - Danh sách khả năng của agent

**Validation rules**:
- id là bắt buộc và phải là chuỗi không rỗng
- name là bắt buộc và phải có độ dài từ 1 đến 100 ký tự
- status là bắt buộc và phải là một trong ba giá trị được liệt kê
- lastActive là bắt buộc và phải là một ngày hợp lệ
- capabilities là bắt buộc và phải là một mảng chuỗi

### 6. PortfolioItem

**Mô tả**: Đại diện cho một mục trong danh mục đầu tư của người dùng

**Trường dữ liệu**:
- id: string - Định danh duy nhất cho mục đầu tư
- userId: string - ID của người dùng sở hữu mục đầu tư
- symbol: string - Mã chứng khoán/tài sản
- name: string - Tên của tài sản
- quantity: number - Số lượng tài sản
- purchasePrice: number - Giá mua trung bình
- currentPrice: number - Giá hiện tại
- value: number - Giá trị hiện tại (quantity * currentPrice)
- changePercent: number - Phần trăm thay đổi so với giá mua
- assetType: 'stock' | 'bond' | 'etf' | 'mutual_fund' | 'crypto' | 'other' - Loại tài sản

**Validation rules**:
- id là bắt buộc và phải là chuỗi không rỗng
- userId là bắt buộc và phải là chuỗi không rỗng
- symbol là bắt buộc và phải có độ dài từ 1 đến 10 ký tự
- name là bắt buộc và phải có độ dài từ 1 đến 100 ký tự
- quantity là bắt buộc và phải là số dương
- purchasePrice là bắt buộc và phải là số không âm
- currentPrice là bắt buộc và phải là số không âm
- value là bắt buộc và phải là số không âm
- changePercent là bắt buộc và phải là số
- assetType là bắt buộc và phải là một trong sáu giá trị được liệt kê

## Mối quan hệ giữa các thực thể

1. **User - ChatSession**: Một người dùng có thể có nhiều phiên chat (1:N)
2. **ChatSession - ChatMessage**: Một phiên chat có thể chứa nhiều tin nhắn (1:N)
3. **User - PortfolioItem**: Một người dùng có thể có nhiều mục đầu tư (1:N)
4. **ChatMessage - AgentStatus**: Một tin nhắn có thể liên quan đến một hoặc nhiều agent (N:M thông qua metadata)

## Trạng thái và chuyển đổi

### Trạng thái tin nhắn (ChatMessage.status)
- sending → sent: Tin nhắn đã được gửi đi
- sent → delivered: Tin nhắn đã được nhận bởi server
- sent → error: Gặp lỗi khi gửi tin nhắn
- delivered: Tin nhắn đã được gửi và nhận thành công

### Trạng thái agent (AgentStatus.status)
- idle ↔ busy: Agent chuyển giữa trạng thái rảnh và bận khi nhận nhiệm vụ
- offline: Agent không khả dụng

## Ghi chú đặc biệt

- Tất cả các model đều sử dụng TypeScript interfaces để đảm bảo type safety
- Dữ liệu được xác thực cả phía frontend và backend
- Các ID nên sử dụng UUID để đảm bảo tính duy nhất toàn cục
- Các trường ngày tháng nên sử dụng định dạng ISO 8601 khi truyền qua API