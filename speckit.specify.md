# Specification: Ứng dụng AI Chat ứng dụng Agent-to-Agent (A2A) trong lĩnh vực Finance B2C

## Tổng quan dự án

Dự án xây dựng một **AI Chat Application dựa trên kiến trúc Agent-to-Agent (A2A)**, đóng vai trò là **Trợ lý phân tích & diễn giải danh mục tài chính cá nhân (Decision Support System)**. Ứng dụng **không cung cấp tư vấn đầu tư**, mà tập trung vào việc giúp người dùng hiểu rõ hơn bức tranh tài chính của chính họ.

### Mục tiêu chính
- Thu thập và cấu trúc danh mục qua hội thoại
- Diễn giải cấu trúc danh mục
- Phân tích rủi ro ở mức mô tả (descriptive)
- Nhận diện các khuynh hướng hành vi phổ biến
- Cung cấp thông tin để người dùng tự ra quyết định

### Nguyên tắc thiết kế & An toàn pháp lý

#### Những gì hệ thống CÓ LÀM
- Mô tả và diễn giải danh mục tài chính
- So sánh cấu trúc danh mục với các phân bố phổ biến
- Cảnh báo rủi ro ở mức cấu trúc và hành vi
- Cung cấp thông tin để người dùng tự ra quyết định

#### Những gì hệ thống KHÔNG LÀM
- Không đưa ra khuyến nghị mua / bán
- Không dự đoán giá hoặc xu hướng thị trường
- Không đánh giá "đúng / sai" trong quyết định đầu tư
- Không cam kết lợi nhuận

> Mọi insight được trình bày dưới dạng **mô tả – so sánh – cảnh báo rủi ro**, tránh ngôn ngữ chỉ dẫn ("nên", "không nên").

## Công nghệ sử dụng

### Frontend
- **Framework**: React + Vite
- **Ngôn ngữ**: TypeScript
- **Styling**: Tailwind CSS
- **Thư viện UI**: assistant-ui
- **Testing**: Vitest
- **Trình quản lý package**: pnpm

### Backend
- **Framework**: NestJS
- **Runtime**: Node.js
- **Ngôn ngữ**: TypeScript
- **Giao thức**: HTTP (REST), Server-Sent Events (SSE) để stream phản hồi AI
- **Kiến trúc**: Modular, sử dụng Dependency Injection

### Database
- **MySQL** (hoặc SQLite cho MVP)
- **Môi trường phát triển**: Docker sử dụng docker-compose

### Agent System
- **Framework Agent**: LangChain
- **Nguyên tắc thiết kế Agent**:
  - Mỗi Agent chỉ đảm nhiệm một nhiệm vụ duy nhất, rõ ràng
  - Agent NÊN được triển khai dưới dạng provider của NestJS
  - Các Agent PHẢI giao tiếp với nhau thông qua A2A protocol dạng JSON

### Mô hình AI
- Nhà cung cấp AI PHẢI được cấu hình thông qua biến môi trường
- Hệ thống PHẢI hỗ trợ ít nhất một LLM bên ngoài (ví dụ: OpenAI hoặc Claude)
- Ứng dụng TUYỆT ĐỐI KHÔNG được đưa ra lời khuyên đầu tư

## Kiến trúc hệ thống

### Frontend
- **Web**: React + Vite
- **Desktop**: Electron (giai đoạn sau)
- Cung cấp:
  - Giao diện chat hội thoại
  - Nhập và chỉnh sửa danh mục qua chat
  - Hiển thị kết quả phân tích
  - Quan sát luồng xử lý Agent (A2A Trace)

### Backend & Agent System
- **Framework**: NestJS
- **Ngôn ngữ**: TypeScript
- **Database**: MySQL
- **Môi trường phát triển**: Docker
- **ADK Framework**
- Agent được tách theo trách nhiệm logic
- MVP triển khai logical A2A (chung service)
- Giai đoạn sau có thể mở rộng sang physical A2A qua HTTP/JSON

### API Layer (Backend – Frontend Interface)
Backend cung cấp một lớp API làm ranh giới giao tiếp với Frontend, bao gồm:
- Chat API:
  - Nhận message từ người dùng
  - Trả về phản hồi AI (hỗ trợ streaming)
- Portfolio API:
  - Truy xuất trạng thái danh mục hiện tại
  - Nhận yêu cầu xác nhận thêm / sửa / xoá danh mục
- A2A Trace API:
  - Cung cấp thông tin luồng xử lý Agent
  - Phục vụ hiển thị và debug trên giao diện người dùng

Frontend **không giao tiếp trực tiếp với các Agent**, mà chỉ tương tác thông qua API này.

## Hệ thống Agent-to-Agent (A2A)

### Định nghĩa A2A trong dự án
Agent-to-Agent (A2A) là mô hình trong đó:
- Mỗi Agent chịu trách nhiệm một khía cạnh phân tích
- Orchestrator Agent điều phối luồng xử lý
- Kết quả được tổng hợp và diễn giải lại cho người dùng

Mục tiêu chính của A2A:
- Dễ mở rộng domain
- Dễ debug
- Minh bạch hoá quá trình phân tích

### Các Agent chính

#### 1. Chat / Orchestrator Agent
- Giao tiếp với người dùng
- Phát hiện thông tin danh mục trong hội thoại
- Đề xuất CRUD danh mục (yêu cầu xác nhận)
- Điều phối các Agent phân tích

#### 2. Portfolio Analyzer Agent
- Phân tích cấu trúc danh mục
- Tính tỷ trọng theo:
  - Loại tài sản
  - Mã tài sản
  - Tiền tệ

#### 3. Risk Agent
- Đánh giá rủi ro ở mức **cấu trúc danh mục**
- Ví dụ:
  - Mức độ tập trung
  - Phân bổ theo loại tài sản
  - Exposure theo tiền tệ
- (Phân tích thống kê nâng cao sẽ được triển khai khi có dữ liệu thị trường)

#### 4. Correlation Agent
- Phân tích mức độ đa dạng hoá
- Dựa trên:
  - Nhóm tài sản
  - Ngành / loại hình
- Không giả định dự báo giá trong MVP

#### 5. Behavior Analysis Agent
- Nhận diện các **khuynh hướng hành vi phổ biến**
- Ví dụ:
  - Tập trung cao vào một tài sản
  - Giao dịch dày trong thời gian ngắn
- Trình bày dưới dạng **quan sát trung lập**, không phán xét

#### 6. Insight / Report Agent
- Chuyển phân tích kỹ thuật thành ngôn ngữ dễ hiểu
- Trình bày theo dạng hội thoại
- Nhấn mạnh: "đây là mô tả, không phải khuyến nghị"

## Luồng A2A mẫu

```
Người dùng chat
        ↓
Portfolio Detect (Orchestrator)
        ↓
Xác nhận với người dùng thêm / sửa / xoá danh mục
        ↓
Portfolio Analyzer
        ↓
Risk Agent
        ↓
Correlation Agent
        ↓
Behavior Analysis Agent
        ↓
Insight Agent
        ↓
Người dùng
```

## Các tính năng chính của ứng dụng

### 8.1 Cấu hình A2A
- Quản lý danh sách Agent
- Cấu hình A2A
- Theo dõi trạng thái Agent

### 8.2 Chat với A2A Agent
- Chat hội thoại với AI
- Streaming response
- Hiển thị agent đang xử lý

### 8.3 Quan sát luồng A2A (Trace)
- Theo dõi từng bước Agent xử lý
- Debug và minh bạch hoá phân tích

## MVP đề xuất

### Phạm vi MVP
- Nhập danh mục thủ công qua chat
- Phân tích danh mục
- 4 Agent cốt lõi:
  - Portfolio
  - Risk (structure-based)
  - Correlation (category-based)
  - Insight

## Mô hình kinh doanh
- B2C Subscription
- Giá dự kiến: 5–15 USD / tháng
- Free tier giới hạn số lần phân tích