# Feature Specification: Triển khai các A2A Agents cho phân tích tài chính

**Feature Branch**: `006-a2a-agent-implementation`
**Created**: 2026-01-22
**Status**: Draft
**Input**: User description: "Triển khai các A2A Agents cho phân tích tài chính (Portfolio Analyzer, Risk, Correlation, Behavior Analysis)"

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

### User Story 1 - Phân tích cấu trúc danh mục đầu tư (Priority: P1)

Người dùng có thể nhận được phân tích về cấu trúc danh mục đầu tư của họ, bao gồm tỷ trọng theo loại tài sản, mã tài sản, và tiền tệ. Agent sẽ phân tích dữ liệu danh mục và cung cấp thông tin mô tả giúp người dùng hiểu rõ bức tranh tài chính của họ.

**Why this priority**: Đây là chức năng cốt lõi của hệ thống - người dùng cần hiểu rõ cấu trúc danh mục hiện tại để đưa ra quyết định tài chính sáng suốt. Đây là nền tảng cho tất cả các phân tích khác như rủi ro và đa dạng hóa.

**Interaction Method**: Người dùng tương tác với hệ thống thông qua giao diện chat hiện có, gửi yêu cầu phân tích danh mục đầu tư và nhận kết quả qua hệ thống A2A messaging.

**Independent Test**: Có thể kiểm tra độc lập bằng cách cung cấp một danh mục đầu tư mẫu và xác nhận rằng hệ thống trả về phân tích cấu trúc chính xác, bao gồm tỷ trọng theo các tiêu chí khác nhau. Chức năng này mang lại giá trị là khả năng hiểu rõ cấu trúc danh mục đầu tư.

**Acceptance Scenarios**:

1. **Given** người dùng có danh mục đầu tư với nhiều loại tài sản khác nhau, **When** họ yêu cầu phân tích cấu trúc danh mục, **Then** hệ thống trả về tỷ trọng theo loại tài sản (cổ phiếu, quỹ, crypto, tiền mặt)
2. **Given** người dùng có danh mục đầu tư với nhiều mã tài sản, **When** họ yêu cầu phân tích cấu trúc danh mục, **Then** hệ thống trả về tỷ trọng theo mã tài sản

---

### User Story 2 - Phân tích rủi ro cấu trúc danh mục (Priority: P1)

Người dùng có thể nhận được đánh giá về rủi ro cấu trúc danh mục của họ, bao gồm mức độ tập trung, phân bổ theo loại tài sản và exposure theo tiền tệ. Agent sẽ phân tích cấu trúc danh mục và cảnh báo về các rủi ro tiềm ẩn ở mức mô tả.

**Why this priority**: Đây là chức năng cốt lõi thứ hai trong việc giúp người dùng hiểu rõ rủi ro trong danh mục của họ, không đưa ra khuyến nghị mua/bán mà chỉ mô tả và cảnh báo rủi ro.

**Interaction Method**: Người dùng tương tác với hệ thống thông qua giao diện chat hiện có, gửi yêu cầu phân tích rủi ro và nhận kết quả qua hệ thống A2A messaging sau khi đã xác thực người dùng.

**Independent Test**: Có thể kiểm tra độc lập bằng cách cung cấp một danh mục có mức độ tập trung cao và xác nhận rằng hệ thống nhận diện và cảnh báo về rủi ro tập trung. Chức năng này mang lại giá trị là nhận thức rõ hơn về rủi ro trong danh mục đầu tư.

**Acceptance Scenarios**:

1. **Given** người dùng có danh mục đầu tư tập trung cao vào một tài sản duy nhất, **When** họ yêu cầu phân tích rủi ro, **Then** hệ thống cảnh báo về mức độ tập trung và đề xuất cân nhắc đa dạng hóa
2. **Given** người dùng có danh mục đầu tư với nhiều loại tiền tệ khác nhau, **When** họ yêu cầu phân tích rủi ro tiền tệ, **Then** hệ thống mô tả mức độ exposure theo tiền tệ

---

### User Story 3 - Phân tích mức độ đa dạng hóa (Priority: P2)

Người dùng có thể nhận được phân tích về mức độ đa dạng hóa trong danh mục đầu tư của họ dựa trên nhóm tài sản, ngành và loại hình. Agent sẽ đánh giá mức độ phân tán của danh mục và cung cấp thông tin mô tả về mức độ đa dạng hóa.

**Why this priority**: Sau phân tích cấu trúc và rủi ro, việc đánh giá mức độ đa dạng hóa là bước tiếp theo quan trọng để giúp người dùng hiểu rõ hơn về chiến lược đầu tư của họ.

**Interaction Method**: Người dùng tương tác với hệ thống thông qua giao diện chat hiện có, gửi yêu cầu phân tích đa dạng hóa và nhận kết quả qua hệ thống A2A messaging sau khi đã xác thực người dùng.

**Independent Test**: Có thể kiểm tra độc lập bằng cách cung cấp một danh mục đầu tư có các tài sản cùng ngành và xác nhận rằng hệ thống nhận diện mức độ tập trung ngành và mô tả mức độ đa dạng hóa. Chức năng này mang lại giá trị là hiểu rõ hơn về mức độ phân tán rủi ro.

**Acceptance Scenarios**:

1. **Given** người dùng có danh mục đầu tư tập trung trong cùng một ngành, **When** họ yêu cầu phân tích đa dạng hóa, **Then** hệ thống mô tả mức độ tập trung ngành và mức độ đa dạng hóa theo nhóm tài sản
2. **Given** người dùng có danh mục đầu tư đa dạng theo nhiều ngành khác nhau, **When** họ yêu cầu phân tích đa dạng hóa, **Then** hệ thống xác nhận mức độ đa dạng hóa và mô tả sự phân bố

---

### User Story 4 - Phân tích hành vi đầu tư (Priority: P2)

Người dùng có thể nhận được phân tích về các khuynh hướng hành vi phổ biến trong lịch sử giao dịch của họ. Agent sẽ nhận diện các mẫu hành vi như tập trung cao vào một tài sản, giao dịch dày trong thời gian ngắn và trình bày dưới dạng quan sát trung lập.

**Why this priority**: Việc nhận diện hành vi đầu tư giúp người dùng hiểu rõ hơn về xu hướng và thói quen đầu tư của họ, hỗ trợ họ đưa ra quyết định sáng suốt hơn trong tương lai.

**Interaction Method**: Người dùng tương tác với hệ thống thông qua giao diện chat hiện có, gửi yêu cầu phân tích hành vi đầu tư và nhận kết quả qua hệ thống A2A messaging sau khi đã xác thực người dùng.

**Independent Test**: Có thể kiểm tra độc lập bằng cách phân tích lịch sử giao dịch và xác nhận rằng hệ thống nhận diện các mẫu hành vi phổ biến và trình bày dưới dạng quan sát trung lập, không phán xét. Chức năng này mang lại giá trị là nhận thức rõ hơn về hành vi đầu tư cá nhân.

**Acceptance Scenarios**:

1. **Given** người dùng có lịch sử giao dịch thể hiện xu hướng tập trung vào một tài sản cụ thể, **When** họ yêu cầu phân tích hành vi, **Then** hệ thống nhận diện và mô tả khuynh hướng này dưới dạng quan sát trung lập
2. **Given** người dùng có lịch sử giao dịch thể hiện giao dịch dày đặc trong thời gian ngắn, **When** họ yêu cầu phân tích hành vi, **Then** hệ thống nhận diện và mô tả mẫu hành vi này

---

### User Story 5 - Tích hợp và điều phối A2A (Priority: P3)

Hệ thống có thể điều phối các Agent khác nhau trong quy trình A2A, đảm bảo luồng xử lý liền mạch từ phân tích cấu trúc đến phân tích rủi ro, đa dạng hóa và hành vi. Orchestrator sẽ đảm bảo các Agent hoạt động phối hợp hiệu quả.

**Why this priority**: Sau khi các Agent riêng lẻ hoạt động, việc tích hợp và điều phối chúng là cần thiết để tạo nên trải nghiệm phân tích toàn diện cho người dùng.

**Interaction Method**: Người dùng tương tác với hệ thống thông qua giao diện chat hiện có, gửi yêu cầu phân tích toàn diện và nhận kết quả được tổng hợp từ nhiều Agent qua hệ thống A2A messaging sau khi đã xác thực người dùng.

**Independent Test**: Có thể kiểm tra độc lập bằng cách gửi yêu cầu phân tích toàn diện và xác nhận rằng hệ thống kích hoạt đúng các Agent theo thứ tự hợp lý, tổng hợp kết quả và trả về cho người dùng. Chức năng này mang lại giá trị là trải nghiệm phân tích liền mạch và toàn diện.

**Acceptance Scenarios**:

1. **Given** người dùng yêu cầu phân tích toàn diện về danh mục đầu tư, **When** hệ thống nhận yêu cầu, **Then** hệ thống kích hoạt các Agent theo thứ tự hợp lý (cấu trúc -> rủi ro -> đa dạng hóa -> hành vi)
2. **Given** các Agent đang xử lý yêu cầu phân tích, **When** một Agent hoàn thành, **Then** hệ thống chuyển tiếp kết quả cho các Agent tiếp theo hoặc tổng hợp cuối cùng

---

### User Story 5.1 - A2A Orchestrator Agent thông minh (Priority: P3)
Hệ thống có một A2A Orchestrator Agent thực sự sử dụng trí tuệ nhân tạo để phân tích cuộc hội thoại của người dùng và ra quyết định điều phối thông minh các agent khác, thay vì chỉ là một dịch vụ điều phối logic đơn thuần. Agent này sẽ phân tích yêu cầu của người dùng và quyết định chiến lược điều phối các agent phù hợp.

**Why this priority**: Sau khi các Agent riêng lẻ hoạt động, việc có một orchestrator thực sự thông minh sử dụng LLM để phân tích cuộc hội thoại với người dùng và đưa ra quyết định điều phối là cần thiết để tạo nên hệ thống A2A đúng nghĩa với khả năng học hỏi và thích nghi.

**Interaction Method**: Người dùng tương tác với hệ thống thông qua giao diện chat hiện có, gửi yêu cầu phân tích toàn diện và A2A Orchestrator Agent sử dụng LLM để phân tích yêu cầu và kích hoạt các agent phù hợp theo chiến lược thông minh.

**Independent Test**: Có thể kiểm tra độc lập bằng cách gửi yêu cầu phân tích toàn diện và xác nhận rằng A2A Orchestrator Agent phân tích yêu cầu của người dùng và kích hoạt các agent phù hợp theo chiến lược thông minh. Chức năng này mang lại giá trị là khả năng điều phối thông minh giữa các agent dựa trên phân tích cuộc hội thoại với người dùng.

**Acceptance Scenarios**:
1. **Given** người dùng gửi yêu cầu phân tích toàn diện về danh mục đầu tư, **When** A2A Orchestrator Agent nhận yêu cầu, **Then** hệ thống sử dụng LLM để phân tích yêu cầu và kích hoạt các agent theo thứ tự hợp lý (cấu trúc -> rủi ro -> đa dạng hóa -> hành vi)
2. **Given** người dùng gửi yêu cầu cụ thể về rủi ro danh mục, **When** A2A Orchestrator Agent phân tích yêu cầu, **Then** hệ thống quyết định chỉ kích hoạt các agent liên quan đến phân tích rủi ro
### Edge Cases

- What happens when người dùng có danh mục đầu tư với tài sản không xác định hoặc thiếu thông tin phân loại? → System should provide partial analysis with clear indication of missing data
- How does system handle trường hợp danh mục đầu tư quá lớn khiến việc phân tích mất nhiều thời gian? → All analyses should complete within 30 seconds for portfolios up to 100 assets
- What happens when người dùng có tài sản với giá trị âm hoặc không hợp lệ? → System should flag these assets and exclude them from percentage calculations while noting the exception
- How does system handle trường hợp một hoặc nhiều Agent không phản hồi trong quá trình A2A? → System should provide available results with notification of which agents failed to respond
- What happens when dữ liệu danh mục đầu tư không đầy đủ cho phân tích rủi ro hoặc đa dạng hóa? → System should provide partial analysis indicating which aspects couldn't be calculated due to insufficient data
- How does the system handle khi tài sản trong danh mục có mã không tồn tại hoặc không thể xác định loại tài sản? → System should categorize unknown assets separately and note them in the analysis
- What occurs when multiple agents provide conflicting analysis results? → System should present both perspectives with confidence levels and note the discrepancy
- How does the system manage khi có sự thay đổi trong danh mục trong khi đang phân tích? → System should lock the portfolio data at the beginning of analysis and note the timestamp of the data snapshot used
- What happens when user is not authenticated properly? → System should require standard JWT-based authentication before processing any requests

## Requirements *(mandatory)*

<!--
  ACTION REQUIRED: The content in this section represents placeholders.
  Fill them out with the right functional requirements.
-->

### Functional Requirements

- **FR-001**: Hệ thống PHẢI triển khai Portfolio Analyzer Agent để phân tích cấu trúc danh mục theo loại tài sản, mã tài sản và tiền tệ
- **FR-002**: Hệ thống PHẢI triển khai Risk Agent để đánh giá rủi ro cấu trúc danh mục bao gồm mức độ tập trung và phân bổ theo loại tài sản
- **FR-003**: Hệ thống PHẢI triển khai Correlation Agent để phân tích mức độ đa dạng hóa theo nhóm tài sản, ngành và loại hình
- **FR-004**: Hệ thống PHẢI triển khai Behavior Analysis Agent để nhận diện các khuynh hướng hành vi phổ biến trong lịch sử giao dịch
- **FR-005**: Hệ thống PHẢI đảm bảo các Agent tuân thủ nguyên tắc "không tư vấn đầu tư" mà chỉ mô tả, so sánh và cảnh báo rủi ro
- **FR-006**: Người dùng PHẢI nhận được kết quả phân tích dưới dạng mô tả trung lập, không có ngôn ngữ chỉ dẫn ("nên", "không nên")
- **FR-007**: Hệ thống PHẢI tích hợp các Agent với hệ thống A2A hiện có thông qua A2AOrchestratorService
- **FR-008**: Hệ thống PHẢI xử lý lỗi khi một hoặc nhiều Agent không phản hồi và vẫn cung cấp kết quả khả dụng
- **FR-009**: Hệ thống PHẢI đảm bảo hiệu suất phân tích trong thời gian chấp nhận được (dưới 30 giây cho danh mục tối đa 100 tài sản)
- **FR-013**: Hệ thống PHẢI yêu cầu xác thực người dùng thông qua JWT token trước khi thực hiện bất kỳ phân tích nào
- **FR-014**: Hệ thống PHẢI hiển thị rõ ràng phần nào của phân tích không thể thực hiện do dữ liệu danh mục bị thiếu hoặc không hợp lệ
- **FR-010**: Hệ thống PHẢI duy trì tính nhất quán dữ liệu trong quá trình phân tích nhiều Agent
- **FR-011**: Hệ thống PHẢI ghi nhật ký quá trình xử lý của từng Agent để phục vụ debug và kiểm toán
- **FR-012**: Hệ thống PHẢI xử lý các trường hợp dữ liệu danh mục không đầy đủ hoặc không hợp lệ một cách an toàn
- **FR-015**: Hệ thống PHẢI triển khai A2A Orchestrator Agent sử dụng LLM để phân tích cuộc hội thoại với người dùng và ra quyết định điều phối các agent khác một cách thông minh
- **FR-016**: A2A Orchestrator Agent PHẢI phân tích hành vi người dùng và đưa ra quyết định điều phối dựa trên ngữ cảnh yêu cầu cụ thể
- **FR-017**: Hệ thống PHẢI đảm bảo A2A Orchestrator Agent hoạt động như một agent bình thường khác trong hệ thống A2A, có khả năng tự phân tích giao tiếp của người dùng để điều phối các agent khác một cách thông minh

### Key Entities *(include if feature involves data)*

- **PortfolioAnalyzerResult**: Đại diện cho kết quả phân tích cấu trúc danh mục, bao gồm tỷ trọng theo loại tài sản, mã tài sản và tiền tệ. Không chứa khuyến nghị đầu tư mà chỉ mô tả cấu trúc hiện tại.
- **RiskAnalysisResult**: Đại diện cho kết quả phân tích rủi ro cấu trúc danh mục, bao gồm mức độ tập trung, phân bổ theo loại tài sản và exposure tiền tệ. Trình bày dưới dạng cảnh báo mô tả chứ không phải khuyến nghị.
- **CorrelationAnalysisResult**: Đại diện cho kết quả phân tích mức độ đa dạng hóa, bao gồm phân tích theo nhóm tài sản, ngành và loại hình. Không giả định dự báo giá mà chỉ đánh giá mức độ phân tán.
- **BehaviorAnalysisResult**: Đại diện cho kết quả phân tích hành vi đầu tư, bao gồm các khuynh hướng hành vi phổ biến được nhận diện. Trình bày dưới dạng quan sát trung lập, không phán xét.
- **A2AMessage**: Đại diện cho thông điệp giao tiếp giữa các Agent trong hệ thống A2A, bao gồm ID tác nhân nguồn/đích, loại thông điệp, hành động, tải trọng dữ liệu và trạng thái xử lý.
- **A2AOrchestrationFlow**: Đại diện cho luồng điều phối xử lý giữa các Agent, đảm bảo thứ tự xử lý hợp lý và tổng hợp kết quả cuối cùng.
- **A2AOrchestrationPlan**: Đại diện cho kế hoạch điều phối được tạo bởi A2A Orchestrator Agent, bao gồm các agent được chọn, thứ tự thực thi và lý do lựa chọn, dựa trên phân tích yêu cầu của người dùng sử dụng LLM.

## Success Criteria *(mandatory)*

<!--
  ACTION REQUIRED: Define measurable success criteria.
  These must be technology-agnostic and measurable.
-->

### Measurable Outcomes

- **SC-001**: Người dùng có thể nhận được phân tích cấu trúc danh mục trong vòng 10 giây cho danh mục có dưới 50 tài sản
- **SC-002**: Hệ thống phân tích chính xác tỷ trọng theo loại tài sản, mã tài sản và tiền tệ với độ chính xác trên 95%
- **SC-003**: Người dùng nhận được phân tích rủi ro cấu trúc danh mục trong vòng 15 giây, bao gồm cảnh báo về mức độ tập trung
- **SC-004**: Hệ thống có thể phân tích mức độ đa dạng hóa theo nhóm tài sản và ngành với độ chính xác trên 90%
- **SC-005**: Người dùng nhận được phân tích hành vi đầu tư dưới dạng quan sát trung lập, không có khuyến nghị đầu tư
- **SC-006**: Hệ thống duy trì hiệu suất khi xử lý đồng thời 100 yêu cầu phân tích từ người dùng khác nhau
- **SC-007**: 95% các yêu cầu phân tích A2A hoàn thành thành công mà không gặp lỗi hệ thống
- **SC-008**: Người dùng đánh giá mức độ hài lòng với kết quả phân tích trên mức 4/5 điểm
- **SC-009**: Tất cả các phân tích hoàn thành trong vòng 30 giây cho danh mục lên đến 100 tài sản
- **SC-010**: A2A Orchestrator Agent có thể phân tích yêu cầu người dùng và đưa ra kế hoạch điều phối phù hợp trong vòng 5 giây
- **SC-011**: 90% các yêu cầu phân tích phức tạp được A2A Orchestrator Agent điều phối chính xác đến các agent phù hợp
- **SC-012**: Người dùng nhận được trải nghiệm phân tích liền mạch với ít nhất 85% yêu cầu được xử lý mà không cần tương tác bổ sung

## Clarifications

### Session 2026-01-22
- Q: How should users interact with the A2A financial agents? → A: Users interact via chat interface through existing A2A messaging system
- Q: What level of security and authentication is required for accessing financial analysis? → A: Standard JWT-based authentication required as per existing system
- Q: How should the system handle incomplete or missing portfolio data? → A: System should provide partial analysis with clear indication of missing data
- Q: What are the specific performance requirements for each type of analysis? → A: All analyses should complete within 30 seconds for portfolios up to 100 assets
- Q: Should the agents provide any form of investment recommendations? → A: Agents should only provide descriptive analysis, not investment recommendations
- Q: How should the A2A Orchestrator Agent differ from a regular orchestration service? → A: A2A Orchestrator Agent should be an intelligent agent using LLM to analyze user requests and make smart orchestration decisions, not just a simple routing service
