# Feature Specification: Thiết kế lược đồ cơ sở dữ liệu

**Feature Branch**: `004-database-schema-design`  
**Created**: 2026-01-17  
**Status**: Draft  
**Input**: User description: "Thiết kế lược đồ cơ sở dữ liệu cho hệ thống Finance Agent"

## User Scenarios & Testing *(mandatory)*

<!--
  IMPORTANT: User stories should be PRIORITIZED as user journeys ordered by importance.
  Each user story/journey must be INDEPENDENTLY TESTABLE - meaning if you implement just ONE of them,
  you should still have a viable MVP (Minimum Viable Product) that delivers value.
  
  Assign priorities (P1, P2, P3, etc.) to each story, where P1 is the most critical.
  Think of each story as a standalone slice of functionality that can be:
  - Developed independently
  - Tested independently
  - Deployed independently
  - Demonstrated to users independently
-->

### User Story 1 - Xác thực người dùng cơ bản (Priority: P1)

Người dùng có thể đăng ký tài khoản mới và đăng nhập vào hệ thống Finance Agent. Hệ thống xác thực người dùng và duy trì phiên làm việc an toàn.

**Why this priority**: Đây là chức năng cốt lõi, không có xác thực thì không thể phân biệt người dùng và bảo vệ dữ liệu cá nhân.

**Independent Test**: Có thể kiểm tra bằng cách đăng ký tài khoản mới, đăng nhập, và xác nhận rằng phiên làm việc được duy trì an toàn.

**Acceptance Scenarios**:

1. **Given** người dùng chưa có tài khoản, **When** họ đăng ký với email và mật khẩu hợp lệ, **Then** tài khoản được tạo thành công và họ có thể đăng nhập
2. **Given** người dùng có tài khoản hợp lệ, **When** họ đăng nhập với thông tin chính xác, **Then** hệ thống xác thực thành công và cấp token truy cập

---

### User Story 2 - Quản lý danh mục đầu tư cá nhân (Priority: P2)

Người dùng có thể thêm, xem, cập nhật và xóa các mục trong danh mục đầu tư cá nhân của họ, bao gồm cổ phiếu, quỹ, tiền điện tử và các loại tài sản khác.

**Why this priority**: Đây là chức năng cốt lõi của hệ thống Finance Agent - cho phép người dùng quản lý tài sản tài chính của họ.

**Independent Test**: Người dùng có thể thêm tài sản vào danh mục, xem danh sách tài sản, cập nhật thông tin và xóa tài sản khỏi danh mục.

**Acceptance Scenarios**:

1. **Given** người dùng đã đăng nhập, **When** họ thêm tài sản vào danh mục, **Then** tài sản được lưu trữ an toàn và chỉ hiển thị cho người dùng đó
2. **Given** người dùng có danh mục tài sản, **When** họ yêu cầu xem danh mục, **Then** hệ thống trả về chỉ những tài sản thuộc về họ

---

### User Story 3 - Lưu trữ lịch sử hội thoại (Priority: P3)

Hệ thống lưu trữ các cuộc hội thoại giữa người dùng và AI Assistant, cho phép người dùng quay lại các cuộc hội thoại trước đó.

**Why this priority**: Tăng trải nghiệm người dùng bằng cách duy trì bối cảnh hội thoại và cho phép truy xuất lịch sử.

**Independent Test**: Người dùng có thể bắt đầu cuộc hội thoại mới, gửi tin nhắn, và quay lại các cuộc hội thoại trước đó.

**Acceptance Scenarios**:

1. **Given** người dùng đang trong cuộc hội thoại, **When** họ gửi tin nhắn, **Then** tin nhắn được lưu trữ và hiển thị trong phiên hội thoại
2. **Given** người dùng có nhiều phiên hội thoại, **When** họ yêu cầu danh sách hội thoại, **Then** hệ thống trả về chỉ các phiên của người dùng đó

---

### User Story 4 - Hỗ trợ hệ thống A2A (Agent-to-Agent) (Priority: P4)

Hệ thống lưu trữ thông tin liên lạc giữa các Agent trong kiến trúc A2A, cho phép theo dõi và gỡ lỗi luồng xử lý Agent.

**Why this priority**: Hỗ trợ khả năng mở rộng hệ thống và debug trong kiến trúc A2A phức tạp.

**Independent Test**: Các Agent có thể giao tiếp với nhau và hệ thống lưu trữ thông tin giao tiếp để giám sát.

**Acceptance Scenarios**:

1. **Given** một Agent cần gửi yêu cầu đến Agent khác, **When** yêu cầu được gửi qua hệ thống A2A, **Then** thông điệp được ghi lại trong cơ sở dữ liệu
2. **Given** có các thông điệp A2A được lưu trữ, **When** người quản trị yêu cầu lịch sử giao tiếp, **Then** hệ thống trả về các thông điệp theo thời gian

---

### Edge Cases

- What happens when người dùng cố gắng truy cập dữ liệu của người dùng khác?
- How does system handle quá nhiều tài sản trong danh mục của một người dùng?
- What happens when người dùng có phiên làm việc hết hạn?
- How does system handle dữ liệu tài sản không hợp lệ hoặc bị thiếu?
- How does the system handle database connection failures during critical operations?
- What happens when the database reaches storage capacity limits?
- How does system handle concurrent access to the same data by multiple users?
- What occurs when database transactions fail or timeout?
- How does the system manage data consistency during schema migrations?
- What happens when foreign key constraints are violated?

## Requirements *(mandatory)*

<!--
  ACTION REQUIRED: The content in this section represents placeholders.
  Fill them out with the right functional requirements.
-->

### Functional Requirements

- **FR-001**: Hệ thống PHẢI hỗ trợ xác thực người dùng bằng email và mật khẩu được hash
- **FR-002**: Hệ thống PHẢI xác thực rằng mỗi người dùng chỉ có thể truy cập dữ liệu của chính họ
- **FR-003**: Người dùng PHẢI có thể đăng ký tài khoản mới với email duy nhất
- **FR-004**: Hệ thống PHẢI lưu trữ thông tin danh mục đầu tư theo định dạng chuẩn hóa (asset_type, symbol, quantity, cost_basis, currency)
- **FR-005**: Hệ thống PHẢI lưu trữ lịch sử hội thoại giữa người dùng và AI Assistant
- **FR-006**: Hệ thống PHẢI lưu trữ thông điệp giao tiếp giữa các Agent trong kiến trúc A2A
- **FR-007**: Người dùng PHẢI có thể thêm, đọc, cập nhật và xóa tài sản trong danh mục của họ
- **FR-008**: Hệ thống PHẢI duy trì lịch sử giá trị tài sản theo thời gian
- **FR-009**: Hệ thống PHẢI lưu trữ thông tin các Agent đang hoạt động trong hệ thống
- **FR-010**: Hệ thống PHẢI đảm bảo tính toàn vẹn dữ liệu thông qua ràng buộc khóa ngoại
- **FR-011**: Hệ thống PHẢI sử dụng MySQL làm cơ sở dữ liệu chính với TypeORM làm ORM layer
- **FR-012**: Hệ thống PHẢI hỗ trợ cơ chế phân quyền người dùng với các vai trò khác nhau
- **FR-013**: Hệ thống PHẢI mã hóa các thông tin nhạy cảm như mật khẩu và token xác thực
- **FR-014**: Hệ thống PHẢI hỗ trợ cơ chế phân trang cho các danh sách dữ liệu lớn
- **FR-015**: Hệ thống PHẢI ghi nhật ký các hoạt động quan trọng để phục vụ giám sát và kiểm toán

### Key Entities *(include if feature involves data)*

- **User**: Đại diện cho người dùng hệ thống, chứa thông tin xác thực và hồ sơ cá nhân, bao gồm email, mật khẩu đã hash, tên, trạng thái hoạt động và thời gian tạo/cập nhật
- **PortfolioItem**: Đại diện cho một tài sản đầu tư trong danh mục, bao gồm loại tài sản, mã, số lượng, giá gốc, giá trị hiện tại, đơn vị tiền tệ và ngày mua
- **ChatSession**: Đại diện cho một phiên hội thoại giữa người dùng và AI Assistant, bao gồm tiêu đề, trạng thái, thời gian tạo/cập nhật và danh sách tin nhắn
- **ChatMessage**: Đại diện cho một tin nhắn trong phiên hội thoại, bao gồm loại người gửi (người dùng, tác nhân, hệ thống), nội dung, siêu dữ liệu và thời gian tạo
- **A2AMessage**: Đại diện cho một thông điệp giao tiếp giữa các Agent trong hệ thống A2A, bao gồm ID tác nhân nguồn/đích, loại thông điệp, hành động, tải trọng dữ liệu và trạng thái xử lý
- **AuthToken**: Đại diện cho token xác thực được sử dụng để duy trì phiên làm việc, bao gồm loại token, thời gian hết hạn, trạng thái hủy bỏ và thời gian tạo
- **AgentRegistry**: Đại diện cho đăng ký các Agent đang hoạt động trong hệ thống, bao gồm tên, mô tả, điểm cuối API, trạng thái hoạt động và khả năng hỗ trợ
- **PortfolioHistory**: Đại diện cho lịch sử giá trị của tài sản theo thời gian, bao gồm ID tài sản, giá trị, ngày ghi nhận và nguồn dữ liệu

## Success Criteria *(mandatory)*

<!--
  ACTION REQUIRED: Define measurable success criteria.
  These must be technology-agnostic and measurable.
-->

### Measurable Outcomes

- **SC-001**: Người dùng có thể đăng ký và đăng nhập thành công với thời gian xử lý dưới 3 giây
- **SC-002**: Hệ thống hỗ trợ ít nhất 10,000 người dùng đồng thời mà không gặp sự cố về hiệu suất
- **SC-003**: Người dùng có thể thêm tài sản vào danh mục với thời gian xử lý trung bình dưới 2 giây
- **SC-004**: Hệ thống duy trì tính toàn vẹn dữ liệu 100% trong các trường hợp kiểm thử stress
- **SC-005**: 95% các phiên làm việc được duy trì thành công trong thời gian tối thiểu 1 giờ
- **SC-006**: Hệ thống có thể truy xuất danh mục tài sản của người dùng với thời gian dưới 1 giây
- **SC-007**: Cơ sở dữ liệu duy trì thời gian hoạt động 99.9% trong thời gian hoạt động bình thường
- **SC-008**: Hệ thống phục hồi từ sự cố mất điện hoặc lỗi phần cứng trong vòng 5 phút
- **SC-009**: 95% các truy vấn cơ sở dữ liệu được thực hiện trong thời gian dưới 500ms
- **SC-010**: Hệ thống có thể mở rộng để hỗ trợ 1 triệu người dùng mà không cần thay đổi lược đồ cơ bản

## Clarifications

### Session 2026-01-17

- Q: What database technology should be used for the system? → A: MySQL as primary database with TypeORM as the ORM layer
- Q: What are the specific performance requirements for database operations? → A: Query response time under 500ms for 95% of requests, support for 1000+ concurrent connections
- Q: How should the system handle database migrations and schema evolution? → A: Use TypeORM migration system with version control and backup procedures
- Q: What security measures should be implemented at the database level? → A: Encrypted connections, role-based access control, audit logging for sensitive operations
- Q: What are the backup and recovery requirements for the database? → A: Daily backups with point-in-time recovery capability and encrypted storage
