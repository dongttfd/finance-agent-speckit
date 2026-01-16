# Milestone 9: Triển khai các agents cốt lõi

## Mục tiêu
Xây dựng 5 agents cốt lõi theo thiết kế trong IDEA.md: Chat/Orchestrator, Portfolio, Risk, Correlation, Behavior Analysis, và Insight agents.

## Công việc cần thực hiện
- Triển khai Chat / Orchestrator Agent (giao tiếp với người dùng, phát hiện thông tin danh mục, đề xuất CRUD, điều phối các agent khác)
- Triển khai Portfolio Analyzer Agent (phân tích cấu trúc danh mục)
- Triển khai Risk Agent (đánh giá rủi ro cấu trúc danh mục)
- Triển khai Correlation Agent (phân tích mức độ đa dạng hóa)
- Triển khai Behavior Analysis Agent (nhận diện khuynh hướng hành vi phổ biến)
- Triển khai Insight Agent (chuyển phân tích thành ngôn ngữ dễ hiểu)
- Tích hợp các agents với Orchestrator
- Tạo cơ chế truyền dữ liệu giữa các agents qua A2A protocol

## Kết quả mong đợi
- 6 agents cốt lõi hoạt động đúng chức năng
- Các agents có thể giao tiếp với nhau qua A2A protocol
- Kết quả phân tích được chuyển tiếp đúng cách qua luồng A2A
- Insight Agent cung cấp thông tin dễ hiểu, không tư vấn đầu tư
- Behavior Analysis Agent nhận diện khuynh hướng hành vi trung lập, không phán xét

## Công nghệ liên quan
- LangChain
- NestJS
- TypeScript
- A2A Protocol
- Financial Analysis Logic