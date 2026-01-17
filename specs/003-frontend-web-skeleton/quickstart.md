# Quickstart Guide: Thiết lập giao diện frontend Web cho ứng dụng AI Chat Agent-to-Agent trong lĩnh vực tài chính

## Mục tiêu

Tài liệu này cung cấp hướng dẫn nhanh để thiết lập và chạy frontend web cho ứng dụng AI Chat Agent-to-Agent trong lĩnh vực tài chính.

## Yêu cầu hệ thống

- Node.js v20+ (được cài đặt trên hệ thống)
- pnpm (được cài đặt trên hệ thống)
- Truy cập internet để tải các gói phụ thuộc
- Backend server đang chạy (được cung cấp bởi milestone 02)

## Thiết lập dự án

### 1. Clone repository

```bash
git clone <repository-url>
cd finance-agent-app/frontend
```

### 2. Cài đặt phụ thuộc

```bash
cd finance-agent-app/frontend
pnpm install
```

### 3. Cấu hình biến môi trường

Tạo file `.env` từ file mẫu:

```bash
cp .env.example .env
```

Cập nhật các biến môi trường trong file `.env` nếu cần thiết:

```env
VITE_API_BASE_URL=http://localhost:3000/api
VITE_WS_BASE_URL=ws://localhost:3000
```

### 4. Chạy ứng dụng ở chế độ phát triển

```bash
pnpm dev
```

Ứng dụng sẽ chạy tại `http://localhost:5173`

### 5. Build ứng dụng cho production

```bash
pnpm build
```

Build output sẽ được lưu trong thư mục `dist/`.

### 6. Chạy ứng dụng đã build

```bash
pnpm preview
```

## Cấu trúc thư mục chính

```
finance-agent-app/frontend/
├── index.html
├── package.json
├── tsconfig.json
├── vite.config.ts
├── src/
│   ├── main.tsx
│   ├── App.tsx
│   ├── index.css
│   ├── components/
│   │   ├── ChatInterface/
│   │   └── Layout/
│   ├── pages/
│   ├── services/
│   └── types/
├── public/
└── tests/
```

## Các thành phần chính

### 1. ChatInterface Components

Các thành phần UI chính cho giao diện chat:
- `ChatArea`: Vùng hiển thị lịch sử trò chuyện
- `Message`: Thành phần hiển thị một tin nhắn
- `InputArea`: Vùng nhập tin nhắn
- `MessageList`: Danh sách các tin nhắn

### 2. Layout Components

Các thành phần bố cục:
- `Header`: Thanh tiêu đề ứng dụng
- `Sidebar`: Thanh bên chứa danh sách phiên chat
- `MainContent`: Vùng nội dung chính

### 3. Pages

Các trang chính của ứng dụng:
- `ChatPage`: Trang chat chính
- `PortfolioPage`: Trang quản lý danh mục đầu tư
- `SettingsPage`: Trang cài đặt người dùng

## Tích hợp với backend

Frontend giao tiếp với backend thông qua:
- REST APIs cho các hành động đồng bộ
- Server-Sent Events (SSE) cho các cập nhật thời gian thực
- WebSocket cho các tương tác hai chiều (nếu cần)

Các endpoints chính:
- `POST /api/auth/login` - Đăng nhập
- `GET /api/chat/messages` - Lấy tin nhắn
- `POST /api/chat/send` - Gửi tin nhắn
- `GET /api/portfolio` - Lấy danh mục đầu tư
- `POST /api/portfolio` - Cập nhật danh mục đầu tư

## Testing

Chạy unit tests:
```bash
pnpm test
```

Chạy tests với chế độ watch:
```bash
pnpm test:watch
```

## Troubleshooting

### Common Issues

1. **Module not found errors**: Chạy `pnpm install` để đảm bảo tất cả phụ thuộc được cài đặt

2. **API calls failing**: Kiểm tra lại biến `VITE_API_BASE_URL` trong file `.env`

3. **TypeScript errors**: Chạy `pnpm tsc --noEmit` để kiểm tra lỗi kiểu dữ liệu

4. **Hot reload không hoạt động**: Khởi động lại server phát triển

## Các bước tiếp theo

Sau khi thiết lập frontend web skeleton thành công, bạn có thể:
1. Phát triển các tính năng chat nâng cao
2. Tích hợp với hệ thống A2A agents
3. Thêm các component UI cho việc hiển thị thông tin tài chính
4. Cải thiện UX/UI cho trải nghiệm người dùng tốt hơn