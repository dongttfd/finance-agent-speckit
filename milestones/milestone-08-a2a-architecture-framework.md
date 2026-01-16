# Milestone 8: Xây dựng framework cho hệ thống A2A

## Mục tiêu
Thiết lập nền tảng cho hệ thống Agent-to-Agent (A2A) theo kiến trúc đã định nghĩa.

## Công việc cần thực hiện
- Thiết kế A2A protocol dạng JSON cho giao tiếp giữa các agents
- Tạo base class/interface cho các agents
- Triển khai Orchestrator Agent để điều phối luồng xử lý
- Thiết kế cơ chế đăng ký và quản lý các agents
- Tạo cơ chế A2A trace để theo dõi luồng xử lý
- Thiết lập dependency injection cho các agents trong NestJS

## Kết quả mong đợi
- A2A framework hoạt động ổn định
- Cơ chế giao tiếp giữa các agents theo chuẩn JSON
- Orchestrator có thể điều phối các agents
- Cơ chế trace giúp theo dõi luồng xử lý

## Công nghệ liên quan
- NestJS
- LangChain
- Dependency Injection
- TypeScript
- JSON Protocol