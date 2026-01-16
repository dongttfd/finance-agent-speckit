# Milestone 13: Nâng cấp A2A và triển khai testing

## Mục tiêu
Bổ sung các thành phần còn thiếu trong hệ thống A2A và đảm bảo chất lượng code thông qua testing framework.

## Công việc cần thực hiện
- Triển khai đầy đủ 6 agents như trong thiết kế ban đầu:
  * Chat / Orchestrator Agent (phát hiện thông tin danh mục, đề xuất CRUD, điều phối)
  * Portfolio Analyzer Agent (đã có)
  * Risk Agent (đã có)
  * Correlation Agent (đã có)
  * Behavior Analysis Agent (phân tích hành vi đầu tư, nhận diện khuynh hướng)
  * Insight / Report Agent (đã có nhưng cần đảm bảo đúng chức năng mô tả)
- Triển khai A2A Configuration features:
  * Quản lý danh sách Agent
  * Cấu hình A2A
  * Theo dõi trạng thái Agent
- Tích hợp ADK Framework vào hệ thống A2A
- Thiết lập Vitest cho frontend testing theo yêu cầu TECH.md
- Cập nhật milestone-09 để phản ánh đúng 4 agents cốt lõi theo MVP (Portfolio, Risk, Correlation, và Insight) cộng với Behavior Analysis Agent
- Đảm bảo mỗi agent chỉ đảm nhiệm một nhiệm vụ duy nhất như yêu cầu
- Tạo endpoint A2A tương thích JSON cho mỗi agent

## Kết quả mong đợi
- Đầy đủ 6 agents hoạt động đúng như thiết kế ban đầu
- A2A Configuration interface hoạt động
- Testing framework được thiết lập và chạy thành công
- ADK Framework được tích hợp vào hệ thống
- Mỗi agent có endpoint A2A JSON riêng biệt
- Luồng xử lý A2A hoạt động đúng theo sơ đồ thiết kế

## Công nghệ liên quan
- LangChain
- ADK Framework
- Vitest
- NestJS
- TypeScript
- A2A Protocol