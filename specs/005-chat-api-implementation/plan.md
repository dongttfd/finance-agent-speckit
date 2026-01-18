# Kế hoạch Triển khai: API Chat cho hệ thống Finance Agent

**Nhánh**: `005-chat-api-implementation` | **Ngày**: 2026-01-18 | **Tài liệu đặc tả**: [specs/005-chat-api-implementation/spec.md](specs/005-chat-api-implementation/spec.md)
**Đầu vào**: Tài liệu đặc tả tính năng từ `/specs/005-chat-api-implementation/spec.md`

**Note**: This template is filled in by the `/speckit.plan` command. See `.specify/templates/commands/plan.md` for the execution workflow.

## Summary

Tổng quan: Triển khai API cho hệ thống chat giữa người dùng và AI, hỗ trợ streaming response và tích hợp với hệ thống A2A. Dựa trên đặc tả yêu cầu và nghiên cứu đã thực hiện, hệ thống cung cấp khả năng gửi nhận tin nhắn cơ bản thông qua API REST, hỗ trợ streaming phản hồi qua Server-Sent Events (SSE), tích hợp với hệ thống A2A để xử lý các yêu cầu liên quan đến danh mục đầu tư, quản lý phiên chat với xác thực người dùng. Thiết kế tuân thủ các yêu cầu bảo mật tài chính, hiệu năng và khả năng mở rộng theo đặc tả.

## Ngữ cảnh Kỹ thuật

**Ngôn ngữ/Phiên bản**: TypeScript 5.5.3, Node.js v20+
**Các thư viện chính**: NestJS v11.1.12, TypeORM, MySQL, Redis, OpenAI API
**Lưu trữ**: MySQL database với TypeORM làm ORM layer, Redis cho caching
**Testing**: Jest cho unit và integration tests, với yêu cầu coverage 80%+
**Nền tảng mục tiêu**: Web server backend (Linux/Windows/macOS)
**Loại dự án**: Ứng dụng web với sự tách biệt giữa frontend và backend
**Mục tiêu hiệu suất**: Xử lý tới 10,000 phiên chat đồng thời, thời gian phản hồi dưới 3 giây cho 95% yêu cầu
**Ràng buộc**: <200ms p95 cho các API calls nội bộ, xử lý dữ liệu tài chính an toàn, tuân thủ SOC 2
**Quy mô/Phạm vi**: Hỗ trợ tới 10,000 người dùng đồng thời với lưu trữ lịch sử chat 2 năm

## Constitution Check

*GATE: Must pass before Phase 0 research. Re-check after Phase 1 design.*

**Pre-Phase 1 Design:**
**Code Quality Gate**: ✅ PASS - Tuân thủ các tiêu chuẩn clean code, xử lý lỗi đúng cách, và dễ đọc
**Testing Gate**: ✅ PASS - Duy trì mức độ test coverage 80%+ cho backend, đặc biệt là các unit tests cho backend business logic
**UX Consistency Gate**: ✅ PASS - Giao diện người dùng nhất quán, tuân thủ hệ thống thiết kế
**Performance Gate**: ✅ PASS - Tối ưu hóa cho API response times dưới 500ms và hỗ trợ 1000 requests đồng thời
**Execution Gate**: ✅ PASS - Tuân thủ Task Confirmation Protocol, Pre-Completion Testing, và Task Completion Definition
**Compliance Gate**: ✅ PASS - Tuân thủ các yêu cầu về bảo mật tài chính (SOC 2), không cung cấp lời khuyên đầu tư, dữ liệu tài chính chỉ đọc

**Post-Phase 1 Design Re-check:**
**Code Quality Gate**: ✅ PASS - Kiến trúc module NestJS được thiết kế rõ ràng, tuân thủ nguyên tắc SOLID và separation of concerns
**Testing Gate**: ✅ PASS - Các module được thiết kế với khả năng test độc lập, hỗ trợ mocking và dependency injection
**UX Consistency Gate**: ✅ PASS - Giao diện chat nhất quán với hệ thống thiết kế chung
**Performance Gate**: ✅ PASS - Thiết kế hỗ trợ hiệu suất yêu cầu với TypeORM tối ưu và API endpoints được cấu trúc hiệu quả
**Execution Gate**: ✅ PASS - Các thành phần được thiết kế để dễ dàng tích hợp và triển khai
**Compliance Gate**: ✅ PASS - Thiết kế hỗ trợ các yêu cầu tài chính với audit logging và dữ liệu tài chính chỉ đọc

## Cấu trúc Dự án

### Documentation (this feature)

```text
specs/[###-feature]/
├── plan.md              # This file (/speckit.plan command output)
├── research.md          # Phase 0 output (/speckit.plan command)
├── data-model.md        # Phase 1 output (/speckit.plan command)
├── quickstart.md        # Phase 1 output (/speckit.plan command)
├── contracts/           # Phase 1 output (/speckit.plan command)
└── tasks.md             # Phase 2 output (/speckit.tasks command - NOT created by /speckit.plan)
```

### Source Code (finance-agent-app/backend)

```
finance-agent-app/backend/
├── src/
│   ├── app.module.ts
│   ├── main.ts
│   ├── api/
│   │   ├── chat/
│   │   │   ├── chat.controller.ts
│   │   │   ├── chat.service.ts
│   │   │   ├── chat.module.ts
│   │   │   └── dto/
│   │   │       └── send-message.dto.ts
│   │   ├── auth/
│   │   │   ├── auth.controller.ts
│   │   │   ├── auth.service.ts
│   │   │   └── auth.module.ts
│   │   ├── portfolio/
│   │   │   ├── portfolio.controller.ts
│   │   │   ├── portfolio.service.ts
│   │   │   └── portfolio.module.ts
│   │   └── a2a/
│   │       ├── a2a.controller.ts
│   │       ├── a2a.service.ts
│   │       └── a2a.module.ts
│   ├── models/
│   │   ├── chat-session.entity.ts
│   │   ├── chat-message.entity.ts
│   │   ├── user.entity.ts
│   │   ├── portfolio-item.entity.ts
│   │   └── a2a-message.entity.ts
│   ├── services/
│   │   ├── ai-service.ts
│   │   ├── cache.service.ts
│   │   ├── a2a-orchestrator.service.ts
│   │   └── chat-processing.service.ts
│   ├── config/
│   │   ├── database.config.ts
│   │   ├── security.config.ts
│   │   └── swagger.config.ts
│   ├── middleware/
│   │   └── security.middleware.ts
│   ├── filters/
│   │   └── global-exception.filter.ts
│   └── pipes/
│       └── validation.pipe.ts
├── test/
│   ├── app.e2e-spec.ts
│   └── jest-e2e.json
├── tests/
│   └── unit/
│       ├── chat/
│       ├── portfolio/
│       └── a2a/
├── .env.example
├── .gitignore
├── nest-cli.json
├── package.json
├── tsconfig.json
├── tsconfig.build.json
└── README.md
```

**Quyết định Cấu trúc**: Chọn cấu trúc NestJS modular với các module riêng biệt cho chat, portfolio, auth và a2a. Các entity models được lưu trong thư mục models, các services chính được lưu trong thư mục services. Sử dụng TypeORM cho MySQL integration. Có cấu hình đầy đủ cho môi trường, bảo mật, và Swagger documentation.

## Theo dõi Độ phức tạp

> **Fill ONLY if Constitution Check has violations that must be justified**

| Violation | Why Needed | Simpler Alternative Rejected Because |
|-----------|------------|-------------------------------------|
| Complex A2A integration | Financial analysis requires specialized agents | Simple monolithic approach would not scale or provide the transparency needed for financial compliance |
