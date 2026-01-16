# ĐỀ TÀI

## Xây dựng ứng dụng AI Chat ứng dụng Agent-to-Agent (A2A) trong lĩnh vực Finance B2C

**(Decision Support – Non-Advisory System)**

---

## 1. Tổng quan ý tưởng

Trong bối cảnh nhà đầu tư cá nhân ngày càng tiếp cận nhiều loại tài sản tài chính (cổ phiếu, quỹ, crypto, tiền mặt…), phần lớn người dùng **không thiếu dữ liệu**, mà **thiếu khả năng hiểu và diễn giải danh mục tài chính của chính mình**.

Các công cụ hiện tại thường rơi vào hai thái cực:

* Quá đơn giản (chỉ hiển thị lãi/lỗ, NAV)
* Hoặc tiềm ẩn rủi ro pháp lý (đưa ra khuyến nghị mua/bán)

Dự án này đề xuất xây dựng một **AI Chat Application dựa trên kiến trúc Agent-to-Agent (A2A)**, đóng vai trò là:

> **Trợ lý phân tích & diễn giải danh mục tài chính cá nhân (Decision Support System)**

Ứng dụng **không cung cấp tư vấn đầu tư**, mà tập trung vào:

* Thu thập và cấu trúc danh mục qua hội thoại
* Diễn giải cấu trúc danh mục
* Phân tích rủi ro ở mức mô tả (descriptive)
* Nhận diện các khuynh hướng hành vi phổ biến
* Giúp người dùng **hiểu rõ hơn bức tranh tài chính của chính họ**

---

## 2. Nguyên tắc thiết kế & An toàn pháp lý

Hệ thống tuân thủ các nguyên tắc sau:

### Những gì hệ thống **CÓ LÀM**

* Mô tả và diễn giải danh mục tài chính
* So sánh cấu trúc danh mục với các phân bố phổ biến
* Cảnh báo rủi ro ở mức cấu trúc và hành vi
* Cung cấp thông tin để người dùng **tự ra quyết định**

### Những gì hệ thống **KHÔNG LÀM**

* Không đưa ra khuyến nghị mua / bán
* Không dự đoán giá hoặc xu hướng thị trường
* Không đánh giá “đúng / sai” trong quyết định đầu tư
* Không cam kết lợi nhuận

> Mọi insight được trình bày dưới dạng **mô tả – so sánh – cảnh báo rủi ro**, tránh ngôn ngữ chỉ dẫn (“nên”, “không nên”).

---

## 3. Đối tượng người dùng

* Nhà đầu tư cá nhân (retail investors)
* Người có danh mục nhỏ đến trung bình
* Người muốn:

  * Nhập và quản lý danh mục thông qua hội thoại
  * Hiểu cấu trúc và mức độ tập trung danh mục
  * Nhận diện rủi ro tiềm ẩn
  * Nhìn lại hành vi đầu tư của bản thân một cách trung lập

---

## 4. Phạm vi bài toán (Domain)

### 4.1 Bài toán chính

* Người dùng có thể **nhập, cập nhật, sửa hoặc xoá danh mục đầu tư thông qua chat**
* Hệ thống tự động:

  * Phát hiện thông tin liên quan đến danh mục trong nội dung chat
  * Đề xuất hành động (thêm / sửa / xoá)
  * Yêu cầu **xác nhận rõ ràng từ người dùng** trước khi thay đổi dữ liệu
* Phân tích và diễn giải danh mục thông qua hội thoại AI

---

### 4.2 Chuẩn hoá dữ liệu danh mục

Danh mục đầu tư được chuẩn hoá theo schema tối thiểu:

```json
{
  "asset_type": "stock | fund | crypto | cash",
  "symbol": "string",
  "quantity": "number",
  "cost_basis": "number",
  "currency": "string",
  "date": "optional"
}
```

Agent chỉ có quyền **đề xuất cập nhật**, mọi thay đổi đều cần người dùng xác nhận thông qua UI chat (button / chip).

---

## 5. Kiến trúc tổng thể hệ thống

### 5.1 Frontend

* **Web**: React + Vite
* **Desktop**: Electron (phase sau)
* **Ngôn ngữ & UI**:

  * TypeScript
  * Tailwind CSS
  * assistant-ui

Frontend cung cấp:

* Giao diện chat hội thoại
* Nhập và chỉnh sửa danh mục qua chat
* Hiển thị kết quả phân tích
* Quan sát luồng xử lý Agent (A2A Trace)

---

### 5.2 Backend & Agent System
* **Framework**: NestJs
* **Ngôn ngữ**: TypeScript
* **Database**: MySQL
* **Môi trường phát triển**: Docker
* ADK Framework
* Agent được tách theo **trách nhiệm logic**
* MVP triển khai **logical A2A** (chung service)
* Phase sau có thể mở rộng sang **physical A2A qua HTTP/JSON**

#### API Layer (Backend – Frontend Interface)

Backend cung cấp một lớp API làm ranh giới giao tiếp với Frontend, bao gồm:

* Chat API:
  * Nhận message từ người dùng
  * Trả về phản hồi AI (hỗ trợ streaming)
* Portfolio API:
  * Truy xuất trạng thái danh mục hiện tại
  * Nhận yêu cầu xác nhận thêm / sửa / xoá danh mục
* A2A Trace API:
  * Cung cấp thông tin luồng xử lý Agent
  * Phục vụ hiển thị và debug trên giao diện người dùng

Frontend **không giao tiếp trực tiếp với các Agent**, mà chỉ tương tác thông qua API này.


##### Kiến trúc API
* **Framework**: NestJs
* **Ngôn ngữ**: TypeScript
* **Database**: MySQL
* **Môi trường phát triển**: Docker sử dụng docker-compose để xây dựng môi trường

---

## 6. Hệ thống Agent-to-Agent (A2A)

### 6.1 Định nghĩa A2A trong dự án

Agent-to-Agent (A2A) là mô hình trong đó:

* Mỗi Agent chịu trách nhiệm một khía cạnh phân tích
* Orchestrator Agent điều phối luồng xử lý
* Kết quả được tổng hợp và diễn giải lại cho người dùng

Mục tiêu chính của A2A:

* Dễ mở rộng domain
* Dễ debug
* Minh bạch hoá quá trình phân tích

---

### 6.2 Các Agent chính

#### 1. Chat / Orchestrator Agent

* Giao tiếp với người dùng
* Phát hiện thông tin danh mục trong hội thoại
* Đề xuất CRUD danh mục (yêu cầu xác nhận)
* Điều phối các Agent phân tích

---

#### 2. Portfolio Analyzer Agent

* Phân tích cấu trúc danh mục
* Tính tỷ trọng theo:

  * Loại tài sản
  * Mã tài sản
  * Tiền tệ

---

#### 3. Risk Agent

* Đánh giá rủi ro ở mức **cấu trúc danh mục**
* Ví dụ:

  * Mức độ tập trung
  * Phân bổ theo loại tài sản
  * Exposure theo tiền tệ
* (Phân tích thống kê nâng cao sẽ được triển khai khi có dữ liệu thị trường)

---

#### 4. Correlation Agent

* Phân tích mức độ đa dạng hoá
* Dựa trên:

  * Nhóm tài sản
  * Ngành / loại hình
* Không giả định dự báo giá trong MVP

---

#### 5. Behavior Analysis Agent

* Nhận diện các **khuynh hướng hành vi phổ biến**
* Ví dụ:

  * Tập trung cao vào một tài sản
  * Giao dịch dày trong thời gian ngắn
* Trình bày dưới dạng **quan sát trung lập**, không phán xét

---

#### 6. Insight / Report Agent

* Chuyển phân tích kỹ thuật thành ngôn ngữ dễ hiểu
* Trình bày theo dạng hội thoại
* Nhấn mạnh: “đây là mô tả, không phải khuyến nghị”

---

## 7. Luồng A2A mẫu

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

---

## 8. Các tính năng chính của ứng dụng

### 8.1 Cấu hình A2A

* Quản lý danh sách Agent
* Cấu hình A2A
* Theo dõi trạng thái Agent

### 8.2 Chat với A2A Agent

* Chat hội thoại với AI
* Streaming response
* Hiển thị agent đang xử lý

### 8.3 Quan sát luồng A2A (Trace)

* Theo dõi từng bước Agent xử lý
* Debug và minh bạch hoá phân tích

---

## 9. MVP đề xuất

### Phạm vi MVP

* Nhập danh mục thủ công qua chat
* Phân tích danh mục
* 4 Agent cốt lõi:

  * Portfolio
  * Risk (structure-based)
  * Correlation (category-based)
  * Insight

---

## 10. Mô hình kinh doanh

* B2C Subscription
* Giá dự kiến: 5–15 USD / tháng
* Free tier giới hạn số lần phân tích

---

## 11. Kết luận

Dự án:

* Ứng dụng mô hình **AI Chat + A2A** vào bài toán tài chính cá nhân thực tế
* Tập trung vào **hiểu – diễn giải – cảnh báo**, không tư vấn
* Đảm bảo an toàn pháp lý
* Có khả năng mở rộng thành sản phẩm thương mại

---
