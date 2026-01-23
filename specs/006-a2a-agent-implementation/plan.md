# Kế hoạch Triển khai: Triển khai các A2A Agents cho phân tích tài chính

**Nhánh**: `006-a2a-agent-implementation` | **Ngày**: 2026-01-22 | **Thông số kỹ thuật**: [specs/006-a2a-agent-implementation/spec.md](specs/006-a2a-agent-implementation/spec.md)

**Đầu vào**: Đặc tả tính năng từ `/specs/006-a2a-agent-implementation/spec.md`

**Lưu ý**: Mẫu này được điền bởi lệnh `/speckit.plan`. Xem `.specify/templates/commands/plan.md` để biết quy trình thực thi.

## Tóm tắt

Triển khai các agent phân tích tài chính A2A (Agent-to-Agent) như đã nêu trong đặc tả tính năng. Bao gồm bốn agent chuyên dụng cho phân tích danh mục đầu tư: Portfolio Analyzer, Risk Assessment, Correlation Analysis, và Behavior Analysis. Các agent sẽ tích hợp với hệ thống điều phối A2A hiện tại và tuân theo framework LangChain như đã nêu trong TECH.md. Giải pháp sẽ chủ yếu sử dụng Qwen 3 làm nhà cung cấp LLM như đã nêu trong yêu cầu kỹ thuật, với thiết kế mô-đun cho phép mở rộng dễ dàng sang các nhà cung cấp khác như OpenAI trong tương lai.

## Technical Context

<!--
  ACTION REQUIRED: Replace the content in this section with the technical details
  for the project. The structure here is presented in advisory capacity to guide
  the iteration process.
-->

**Ngôn ngữ/Phiên bản**: TypeScript/Node.js (theo TECH.md)
**Các phụ thuộc chính**: NestJS, LangChain, TypeORM, MySQL, Qwen 3 API client
**Lưu trữ**: Cơ sở dữ liệu MySQL với TypeORM làm lớp ORM (như đã nêu trong đặc tả)
**Kiểm thử**: Jest cho kiểm thử đơn vị và tích hợp (theo dự án hiện tại)
**Nền tảng đích**: Môi trường máy chủ Node.js (theo backend hiện tại)
**Loại dự án**: Dịch vụ backend với kiến trúc agent A2A (theo backend hiện tại)
**Mục tiêu hiệu suất**: Tất cả phân tích nên hoàn thành trong vòng 30 giây cho danh mục lên đến 100 tài sản (theo đặc tả)
**Ràng buộc**: Phải tuân thủ quy định tài chính - không tư vấn đầu tư, chỉ phân tích đọc, ghi nhật ký tuân thủ (theo đặc tả)
**Quy mô/Phạm vi**: Hỗ trợ yêu cầu phân tích đồng thời từ nhiều người dùng, xử lý đến 100 tài sản mỗi danh mục (theo đặc tả)
**Đặc điểm nổi bật**: A2A Orchestrator Agent thông minh sử dụng LLM để phân tích yêu cầu người dùng và ra quyết định điều phối các agent khác một cách thông minh (theo FR-015, FR-016, FR-017)

## Constitution Check

*GATE: Must pass before Phase 0 research. Re-check after Phase 1 design.*

### Pre-Design Check
- ✅ **Tiêu chuẩn chất lượng mã**: Triển khai sẽ tuân theo các phương pháp tốt nhất của TypeScript, xử lý lỗi đúng cách và quy ước đặt tên có ý nghĩa như yêu cầu trong hiến pháp
- ✅ **Yêu cầu kiểm thử**: Sẽ duy trì độ bao phủ kiểm thử trên 80% cho chức năng agent quan trọng và bao gồm kiểm thử đơn vị toàn diện cho logic kinh doanh
- ✅ **Tuân thủ tài chính**: Triển khai sẽ tuân thủ nghiêm ngặt các quy định tài chính - không tư vấn đầu tư, chỉ phân tích đọc, và ghi nhật ký tuân thủ đúng cách
- ✅ **Tích hợp LLM**: Sẽ cấu hình đúng Qwen 3 làm nhà cung cấp LLM chính như đã nêu trong TECH.md, với thiết kế mô-đun để mở rộng trong tương lai sang các nhà cung cấp khác như OpenAI
- ✅ **Kiến trúc A2A**: Sẽ tuân theo giao thức A2A (Agent-to-Agent) bắt buộc với tin nhắn JSON tiêu chuẩn như đã nêu trong TECH.md
- ✅ **Yêu cầu hiệu suất**: Sẽ đáp ứng yêu cầu hoàn thành phân tích trong 30 giây cho danh mục lên đến 100 tài sản như đã nêu trong đặc tả tính năng
- ✅ **Biện pháp bảo mật**: Sẽ triển khai xác thực, ủy quyền và ghi nhật ký kiểm toán đúng cách như yêu cầu cho ứng dụng tài chính

### Post-Design Check (After Phase 1)
- ✅ **Tiêu chuẩn chất lượng mã**: Thiết kế tích hợp các phương pháp tốt nhất của TypeScript với xử lý lỗi đúng cách và đặt tên có ý nghĩa trong triển khai agent
- ✅ **Yêu cầu kiểm thử**: Mô hình dữ liệu và hợp đồng API hỗ trợ khả năng kiểm thử toàn diện với provisions cho độ bao phủ trên 80%
- ✅ **Tuân thủ tài chính**: Tất cả phản hồi của agent được thiết kế chỉ để mô tả, không có khuyến nghị đầu tư như yêu cầu
- ✅ **Tích hợp LLM**: Kiến trúc trừu tượng hóa đúng cách các nhà cung cấp LLM với Qwen 3 là nhà cung cấp chính và thiết kế mô-đun hỗ trợ mở rộng trong tương lai sang các dịch vụ khác như OpenAI
- ✅ **Kiến trúc A2A**: Thiết kế mở rộng giao thức A2A hiện tại với khả năng phân tích tài chính chuyên biệt
- ✅ **Yêu cầu hiệu suất**: Kiến trúc hỗ trợ thời gian phản hồi yêu cầu với các provisions tối ưu hóa
- ✅ **Biện pháp bảo mật**: Thiết kế bao gồm xác thực, ủy quyền và ghi nhật ký kiểm toán đúng cách cho sự tuân thủ tài chính

## Project Structure

### Documentation (this feature)

```text
specs/006-a2a-agent-implementation/
├── plan.md              # This file (/speckit.plan command output)
├── research.md          # Phase 0 output (/speckit.plan command)
├── data-model.md        # Phase 1 output (/speckit.plan command)
├── quickstart.md        # Phase 1 output (/speckit.plan command)
├── contracts/           # Phase 1 output (/speckit.plan command)
└── tasks.md             # Phase 2 output (/speckit.tasks command - NOT created by /speckit.plan)
```

### Source Code (repository root)

```text
finance-agent-app/
└── backend/
    ├── src/
    │   ├── agents/                    # New: A2A agent implementations
    │   │   ├── a2a-orchestrator.agent.ts      # New: Agent thông minh sử dụng LLM để phân tích yêu cầu người dùng và ra quyết định điều phối
    │   │   ├── portfolio-analyzer.agent.ts
    │   │   ├── risk-assessment.agent.ts
    │   │   ├── correlation-analysis.agent.ts
    │   │   └── behavior-analysis.agent.ts
    │   ├── services/
    │   │   ├── a2a-orchestrator.service.ts    # Existing: Updated for new agents (có thể được tích hợp với a2a-orchestrator.agent.ts)
    │   │   ├── ai-service.ts                  # Existing: LLM integration
    │   │   └── portfolio-analysis.service.ts  # Existing: Updated for agent integration
    │   ├── api/
    │   │   └── a2a/                           # Existing: A2A API endpoints
    │   ├── models/
    │   │   ├── a2a-message.entity.ts          # Existing: A2A message model
    │   │   ├── agent-registry.entity.ts       # Existing: Agent registry model
    │   │   ├── portfolio-analyzer-result.entity.ts
    │   │   ├── risk-analysis-result.entity.ts
    │   │   ├── correlation-analysis-result.entity.ts
    │   │   └── behavior-analysis-result.entity.ts
    │   └── types/
    └── tests/
        ├── unit/
        │   ├── agents/
        │   └── services/
        └── integration/
            └── a2a/
```

**Structure Decision**: Following Option 2 (Web application) with backend-only implementation as per existing architecture. New A2A agents will be implemented in the `src/agents/` directory to align with the LangChain framework requirement in TECH.md. The agents will integrate with existing A2A orchestration services and leverage the existing AI service for LLM interactions.

## Phase 0: Outline & Research

### Research Summary
The research phase identified key implementation approaches for the A2A financial agents. The implementation will leverage the LangChain framework as specified in TECH.md, integrating with external LLM providers (OpenAI, Qwen 3, or Gemini). The solution maintains compatibility with the existing A2A orchestration system and follows the JSON-based messaging protocol.

### Các quyết định chính được đưa ra
1. **Tích hợp LangChain**: Sử dụng framework LangChain cho phát triển agent để cung cấp khả năng tương tác LLM mạnh mẽ
2. **Nhà cung cấp Qwen 3 chính**: Sử dụng Qwen 3 làm nhà cung cấp LLM chính như đã nêu trong TECH.md, với thiết kế mô-đun cho việc mở rộng trong tương lai sang các nhà cung cấp khác như OpenAI
3. **Tuân thủ giao thức A2A**: Mở rộng hệ thống nhắn tin A2A hiện tại để hỗ trợ các yêu cầu phân tích tài chính chuyên biệt
4. **Thiết kế ưu tiên tuân thủ**: Đảm bảo tất cả các agent chỉ cung cấp phân tích mô tả, không có khuyến nghị đầu tư
5. **A2A Orchestrator Agent thông minh**: Triển khai A2A Orchestrator như một agent thông minh sử dụng LLM để phân tích yêu cầu người dùng và ra quyết định điều phối các agent khác một cách thông minh, thay vì chỉ là dịch vụ điều phối logic đơn thuần

### Unknowns Resolved
- All technical requirements from the feature specification have been clarified
- Integration approach with existing A2A system is defined
- Performance requirements are achievable with proper optimization
- Compliance requirements are clearly understood and implementable

## Phase 1: Design & Contracts

### Data Model Design
Created comprehensive data model for financial analysis results including PortfolioAnalyzerResult, RiskAnalysisResult, CorrelationAnalysisResult, and BehaviorAnalysisResult entities. The model includes proper validation rules, relationships, and compliance considerations.

### API Contract Design
Developed complete API contract for all four financial analysis agents with detailed request/response schemas, error handling, and compliance requirements. The contract specifies endpoints for Portfolio Analyzer, Risk Assessment, Correlation Analysis, and Behavior Analysis agents.

### Implementation Architecture
Thiết kế kiến trúc mở rộng cơ sở hạ tầng A2A hiện có với các triển khai agent mới trong khi duy trì khả năng tương thích ngược. Thiết kế tách biệt logic agent khỏi các dịch vụ điều phối và đảm bảo kiểm soát bảo mật và tuân thủ đúng cách. Một sự chú ý đặc biệt được dành cho A2A Orchestrator Agent, được triển khai như một agent thông minh sử dụng LLM để phân tích yêu cầu người dùng và ra quyết định điều phối thông minh, thay vì chỉ là một dịch vụ định tuyến đơn giản. Agent này sẽ lựa chọn thông minh các agent khác cần gọi dựa trên yêu cầu cụ thể của từng yêu cầu người dùng.

## Complexity Tracking

> **Fill ONLY if Constitution Check has violations that must be justified**

| Violation | Why Needed | Simpler Alternative Rejected Because |
|-----------|------------|-------------------------------------|
| [e.g., 4th project] | [current need] | [why 3 projects insufficient] |
| [e.g., Repository pattern] | [specific problem] | [why direct DB access insufficient] |

## Update A2AOrchestratorService Role

Sau khi thực hiện các task, cần cập nhật để làm rõ vai trò của A2AOrchestratorService:

A2AOrchestratorService không chỉ là một service, mà là một Agent bình thường khác trong hệ thống A2A. Nhiệm vụ chính của nó là đóng vai trò trung tâm chuyển tiếp các yêu cầu giữa các agent. Ngoài ra, nó còn có khả năng tự phân tích giao tiếp của người dùng để điều phối các agent khác một cách thông minh, giúp tối ưu hóa quy trình xử lý yêu cầu.

## Cập nhật triển khai A2A Orchestrator Agent

Dựa trên các cập nhật đặc tả mới nhất, A2A Orchestrator Agent đòi hỏi sự chú ý đặc biệt vì nó không chỉ là một dịch vụ điều phối đơn giản mà là một agent thông minh sử dụng LLM để phân tích yêu cầu người dùng và ra quyết định điều phối thông minh. Agent này nên được triển khai như một thành phần tinh vi có thể hiểu được ý định của người dùng và quyết định agent nào cần gọi dựa trên ngữ cảnh.

### Những thay đổi triển khai chính cho A2A Orchestrator Agent

1. **Phân tích ý định sử dụng LLM**: A2A Orchestrator Agent phải sử dụng LLM để phân tích yêu cầu người dùng và hiểu được ý định đằng sau chúng, thay vì dựa vào định tuyến dựa trên quy tắc đơn giản.

2. **Lựa chọn agent thông minh**: Thay vì logic định tuyến được xác định trước, bộ điều phối nên lựa chọn thông minh các agent cần gọi dựa trên yêu cầu cụ thể của người dùng.

3. **Điều phối nhận thức ngữ cảnh**: Bộ điều phối nên duy trì và sử dụng ngữ cảnh từ các tương tác trước đó để đưa ra quyết định điều phối sáng suốt hơn.

4. **Lập kế hoạch thực thi thích ứng**: Bộ điều phối nên có thể tạo các kế hoạch thực thi động dựa trên độ phức tạp và yêu cầu của từng yêu cầu, có thể nối chuỗi nhiều agent theo trình tự tối ưu.
