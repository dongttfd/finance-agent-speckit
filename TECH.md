---

## Yêu cầu kỹ thuật & Ràng buộc (Phiên bản tiếng Việt)

### 1. Cấu trúc thư mục

* **Toàn bộ mã nguồn BẮT BUỘC phải nằm trong thư mục `finance-agent-app`.** và được quản lý trên 1 thư mục bằng git submodule
* Mã nguồn frontend, backend và agent **PHẢI được tách rõ ràng** trong các thư mục con của `finance-agent-app`.

---

### 2. Technology Stack

#### 2.1 Frontend – Web

* **Framework:** React + Vite
* **Ngôn ngữ:** TypeScript
* **Styling:** Tailwind CSS
* **Thư viện UI:** assistant-ui
* **Testing:** Vitest
* **Trình quản lý package:** pnpm

---

#### 2.2 Frontend – Desktop

* **Framework:** Electron
* **Ngôn ngữ:** TypeScript
* **UI Layer:** React (dùng chung codebase với Web)
* **Build Tool:** Vite
* **Mục tiêu:**

  * Tái sử dụng tối đa UI và logic từ frontend Web
  * Cung cấp trải nghiệm Desktop cho AI Chat
  * Hoạt động như client thuần, **KHÔNG chứa logic AI**
  * Giao tiếp với Backend NestJS thông qua HTTP / SSE

---

#### 2.3 Backend

* **Framework:** NestJS
* **Runtime:** Node.js
* **Ngôn ngữ:** TypeScript
* **Giao thức:**

  * HTTP (REST)
  * Server-Sent Events (SSE) để stream phản hồi AI
* **Kiến trúc:** Modular, sử dụng Dependency Injection

---

### 3. Kiến trúc Agent & A2A

* **Framework Agent:** LangChain
* **Nguyên tắc thiết kế Agent:**

  * Mỗi Agent **chỉ đảm nhiệm một nhiệm vụ duy nhất**, rõ ràng.
  * Agent **NÊN** được triển khai dưới dạng provider của NestJS.
  * **Giao tiếp A2A (Agent-to-Agent):**

  * Các Agent **PHẢI** giao tiếp với nhau thông qua **A2A protocol dạng JSON**.
  * Mỗi Agent **PHẢI** cung cấp endpoint tương thích A2A (nội bộ hoặc public).
  * **Agent điều phối (Orchestrator Agent):**

  * BẮT BUỘC có một Agent trung tâm để:

    * Nhận yêu cầu từ người dùng
    * Điều phối luồng Agent-to-Agent
    * Tổng hợp kết quả trả về

---

### 4. Mô hình AI & Cấu hình

* Nhà cung cấp AI **PHẢI** được cấu hình thông qua biến môi trường.
* Hệ thống **PHẢI** hỗ trợ ít nhất một LLM bên ngoài (ví dụ: OpenAI hoặc Claude).
* Ứng dụng **TUYỆT ĐỐI KHÔNG** được đưa ra lời khuyên đầu tư; mọi phản hồi AI **chỉ mang tính phân tích và cung cấp thông tin**.

---

### 5. Xử lý dữ liệu

* **Lưu trữ dữ liệu (MVP):**

  * In-memory hoặc database nhẹ (ví dụ: SQLite).
* Backend **PHẢI** hỗ trợ:

  * Quản lý trạng thái hội thoại
  * Lưu snapshot danh mục tài chính người dùng
* Lưu trữ lâu dài **KHÔNG BẮT BUỘC** trong giai đoạn MVP.

---

### 6. Chính sách kiểm thử

* **KHÔNG bắt buộc** viết unit test cho toàn bộ component frontend.
* **TOÀN BỘ unit test hiện có PHẢI chạy pass** trước khi bắt đầu bất kỳ task mới nào.
* Các service backend và logic Agent **NÊN** có khả năng test độc lập (sử dụng Jest – mặc định của NestJS).

---

### 7. Ràng buộc bảo mật & tuân thủ (Finance)

* Ứng dụng **PHẢI** hiển thị rõ ràng thông báo:
  **“Không cung cấp lời khuyên đầu tư”**.
* Dữ liệu tài chính của người dùng **CHỈ được phân tích ở chế độ đọc (read-only)**.
* **CẤM** mọi chức năng:

  * Giao dịch
  * Dự đoán giá
  * Phát tín hiệu mua/bán

---

### 8. Thao tác với file

* Sau mỗi thao tác **tạo file**, **PHẢI xác minh file đã được tạo thành công**.

---

### 9. Các mục tiêu KHÔNG thực hiện (Non-Goals)

* Thu thập dữ liệu giá thị trường theo thời gian thực.
* Giao dịch tự động.
* Đưa ra khuyến nghị đầu tư cá nhân hóa.

---

### 10. Nguyên tắc kiến trúc tổng thể

* Frontend Web và Desktop **chỉ đóng vai trò client**
* Backend NestJS là **single source of truth**
* Toàn bộ logic AI & A2A **CHỈ tồn tại ở Backend**

---
