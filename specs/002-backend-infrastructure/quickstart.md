# Quickstart Guide: Backend Infrastructure for AI Chat Application with A2A Architecture

## Mục tiêu

Tài liệu này hướng dẫn nhanh cách thiết lập, cấu hình và chạy backend infrastructure cho ứng dụng AI Chat với kiến trúc A2A.

## Yêu cầu hệ thống

- Node.js v20+ 
- pnpm (hoặc npm/yarn nếu không dùng pnpm)
- MySQL 8.0+
- Docker và Docker Compose (tùy chọn nhưng được khuyến nghị)

## Cài đặt nhanh

### 1. Clone repository và cài đặt dependencies

```bash
# Di chuyển vào thư mục backend
cd finance-agent-app/backend

# Cài đặt dependencies với pnpm
pnpm install

# Nếu không có pnpm, cài đặt với npm
npm install
```

### 2. Cấu hình môi trường

Copy file mẫu `.env.example` và cập nhật các biến môi trường:

```bash
cp .env.example .env
```

Sau đó chỉnh sửa file `.env` với các giá trị phù hợp cho môi trường của bạn:

```env
NODE_ENV=development
PORT=3000

# Database configuration
DATABASE_HOST=localhost
DATABASE_PORT=3306
DATABASE_USER=your_username
DATABASE_PASSWORD=your_password
DATABASE_NAME=finance_agent_dev

# JWT configuration
JWT_SECRET=your_jwt_secret_key
JWT_EXPIRES_IN=24h

# AI Provider configuration
AI_PROVIDER=openai  # or anthropic
AI_API_KEY=your_ai_api_key

# CORS configuration
ALLOWED_ORIGINS=http://localhost:5173,http://localhost:3000
```

### 3. Khởi tạo cơ sở dữ liệu

Chạy migration để tạo các bảng cần thiết:

```bash
# Build project trước (nếu cần)
pnpm build

# Chạy migration
pnpm run typeorm migration:run
```

### 4. Chạy ứng dụng

#### Chạy trực tiếp với Node.js:

```bash
# Chạy ở chế độ development (có hot reload)
pnpm run start:dev

# Chạy ở chế độ production
pnpm run start:prod
```

#### Chạy với Docker:

```bash
# Di chuyển đến thư mục gốc của dự án
cd ../..  # từ backend trở về finance-agent-app

# Build và chạy dịch vụ với Docker Compose
docker-compose up --build
```

## Kiến trúc chính

### Modules

Dự án sử dụng kiến trúc module của NestJS với các module chính:

- **AppModule**: Module chính khởi tạo ứng dụng
- **AuthModule**: Xử lý xác thực người dùng
- **ChatModule**: Xử lý các tính năng chat và streaming
- **PortfolioModule**: Xử lý quản lý danh mục đầu tư
- **A2AModule**: Xử lý giao tiếp Agent-to-Agent

### Services chính

- **A2AOrchestratorService**: Dịch vụ điều phối giao tiếp giữa các agents
- **AiAgentService**: Dịch vụ tương tác với AI providers
- **PortfolioAnalysisService**: Dịch vụ phân tích danh mục đầu tư
- **DatabaseService**: Dịch vụ quản lý kết nối và truy vấn cơ sở dữ liệu

### API Endpoints chính

- `GET /health`: Kiểm tra trạng thái hệ thống
- `POST /auth/login`: Đăng nhập người dùng
- `POST /auth/register`: Đăng ký người dùng mới
- `GET /chat/stream`: Streaming AI responses qua SSE
- `POST /chat/send`: Gửi tin nhắn chat
- `GET /portfolio`: Lấy danh mục đầu tư của người dùng
- `POST /portfolio`: Thêm hoặc cập nhật khoản đầu tư
- `POST /a2a/message`: Gửi tin nhắn A2A giữa các agents

## Kiểm thử nhanh

Chạy các lệnh sau để kiểm tra ứng dụng hoạt động đúng:

```bash
# Chạy unit tests
pnpm run test

# Chạy e2e tests
pnpm run test:e2e

# Kiểm tra linting
pnpm run lint
```

## Môi trường phát triển

Dự án đã được cấu hình với:

- **ESLint**: Kiểm tra lỗi cú pháp và quy tắc lập trình
- **Prettier**: Định dạng code tự động
- **Jest**: Framework testing
- **TypeScript**: Static type checking

## Debugging

Để debug ứng dụng:

1. Chạy ứng dụng với chế độ debug:

```bash
pnpm run start:debug
```

2. Kết nối debugger từ IDE của bạn vào port 9229

## Triển khai

Để chuẩn bị cho môi trường production:

1. Đảm bảo các biến môi trường production đã được cấu hình
2. Chạy build:

```bash
pnpm run build
```

3. Chạy ứng dụng production:

```bash
NODE_ENV=production node dist/main
```

## Ghi chú quan trọng

- Ứng dụng tuyệt đối **không được đưa ra lời khuyên đầu tư** - tất cả phản hồi AI chỉ mang tính phân tích và cung cấp thông tin
- Dữ liệu tài chính của người dùng **chỉ được phân tích ở chế độ đọc (read-only)**, không có chức năng giao dịch
- Cấu hình bảo mật đã được thiết lập để tuân thủ các yêu cầu tài chính