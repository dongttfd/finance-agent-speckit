# Data Model: Triển khai các A2A Agents cho phân tích tài chính

## Entity Definitions

### PortfolioAnalyzerResult
- **Description**: Represents the result of portfolio structure analysis
- **Fields**:
  - `id`: string (UUID) - Unique identifier for the analysis result
  - `userId`: string (UUID) - ID of the user who requested the analysis
  - `portfolioComposition`: object - Breakdown of portfolio by asset types, symbols, and currencies
    - `byAssetType`: object - Allocation percentages by asset type (stocks, bonds, crypto, cash, etc.)
    - `bySymbol`: object - Allocation percentages by specific asset symbols
    - `byCurrency`: object - Exposure percentages by currency
  - `analysisSummary`: string - Text summary of portfolio structure
  - `createdAt`: Date - Timestamp when analysis was performed
  - `requestId`: string - Correlation ID linking to original request

### RiskAnalysisResult
- **Description**: Represents the result of portfolio risk assessment
- **Fields**:
  - `id`: string (UUID) - Unique identifier for the risk analysis result
  - `userId`: string (UUID) - ID of the user who requested the analysis
  - `riskFactors`: object - Identified risk factors in the portfolio
    - `concentrationRisk`: object - Level of concentration in single assets/sectors
      - `level`: string (low, medium, high) - Risk level assessment
      - `details`: array of objects - Specific concentration concerns
    - `diversificationLevel`: string (poor, fair, good, excellent) - Overall diversification assessment
    - `currencyExposure`: object - Currency risk exposure breakdown
  - `riskSummary`: string - Text summary of risk assessment
  - `warnings`: array of strings - Specific risk warnings identified
  - `createdAt`: Date - Timestamp when analysis was performed
  - `requestId`: string - Correlation ID linking to original request

### CorrelationAnalysisResult
- **Description**: Represents the result of portfolio correlation/diversification analysis
- **Fields**:
  - `id`: string (UUID) - Unique identifier for the correlation analysis result
  - `userId`: string (UUID) - ID of the user who requested the analysis
  - `diversificationMetrics`: object - Metrics measuring portfolio diversification
    - `assetClassDiversity`: number (0-1) - Diversity score across asset classes
    - `sectorDiversity`: number (0-1) - Diversity score across sectors
    - `geographicDiversity`: number (0-1) - Geographic diversification score
  - `correlationFindings`: array of objects - Specific correlation observations
    - `assetGroup1`: string - First group in correlation
    - `assetGroup2`: string - Second group in correlation
    - `correlationCoefficient`: number - Correlation value (-1 to 1)
    - `significance`: string - Level of significance (low, medium, high)
  - `diversificationSummary`: string - Text summary of diversification assessment
  - `createdAt`: Date - Timestamp when analysis was performed
  - `requestId`: string - Correlation ID linking to original request

### BehaviorAnalysisResult
- **Description**: Represents the result of investor behavior pattern analysis
- **Fields**:
  - `id`: string (UUID) - Unique identifier for the behavior analysis result
  - `userId`: string (UUID) - ID of the user whose behavior was analyzed
  - `behavioralPatterns`: object - Identified behavioral patterns
    - `frequencyPattern`: string - Trading frequency patterns (day trader, swing trader, buy hold, etc.)
    - `timingPattern`: string - Common timing behaviors (market timing attempts, dollar cost averaging, etc.)
    - `riskPattern`: string - Risk-taking behavior patterns
    - `herdingIndicator`: number (0-1) - Tendency to follow market trends
  - `patternDescriptions`: array of strings - Descriptive summaries of identified patterns
  - `behaviorSummary`: string - Overall summary of behavioral observations
  - `createdAt`: Date - Timestamp when analysis was performed
  - `requestId`: string - Correlation ID linking to original request

### A2AMessage (Extended)
- **Description**: Enhanced A2A message entity to support financial agent communications
- **Additional Fields** (beyond base A2AMessage):
  - `analysisType`: string - Type of financial analysis requested (portfolio-analyzer, risk-assessment, etc.)
  - `portfolioData`: object - Portfolio data sent to agents for analysis
  - `analysisResult`: object - Result data returned by agents
  - `complianceFlagged`: boolean - Whether the response was flagged for compliance review
  - `responseQualityScore`: number (0-100) - Quality score of the agent response

### A2AOrchestrationPlan
- **Description**: Đại diện cho kế hoạch điều phối được tạo bởi A2A Orchestrator Agent thông minh
- **Fields**:
  - `id`: string (UUID) - Mã định danh duy nhất cho kế hoạch điều phối
  - `userId`: string (UUID) - ID của người dùng đã khởi tạo yêu cầu
  - `requestMessage`: string - Tin nhắn yêu cầu gốc của người dùng đã kích hoạt điều phối
  - `selectedAgents`: array of objects - Danh sách các agent được chọn để thực thi
    - `agentId`: string - ID của agent cần gọi
    - `action`: string - Hành động cụ thể mà agent cần thực hiện
    - `parameters`: object - Tham số truyền cho agent
  - `executionOrder`: array of strings - Danh sách có thứ tự các ID agent xác định trình tự thực thi
  - `rationale`: string - Giải thích lý do chọn kế hoạch điều phối này
  - `createdAt`: Date - Thời điểm kế hoạch được tạo
  - `executedAt`: Date - Thời điểm thực thi kế hoạch bắt đầu (tùy chọn)
  - `completedAt`: Date - Thời điểm thực thi kế hoạch hoàn thành (tùy chọn)
  - `status`: string - Trạng thái hiện tại (PLANNED, EXECUTING, COMPLETED, FAILED)
  - `correlationId`: string - ID liên kết kế hoạch này với các tin nhắn và kết quả liên quan

## Relationships

### User → Analysis Results
- One-to-many relationship between User and each analysis result type
- Each analysis result belongs to a single user
- Foreign key: `userId` in each analysis result entity

### A2AMessage → Analysis Results
- One-to-one relationship between A2AMessage and analysis result entities
- Links A2A communication to its resulting analysis
- Foreign key: `requestId` in analysis result entities matches `id` in A2AMessage

### User → A2AOrchestrationPlans
- One-to-many relationship between User and A2AOrchestrationPlan entities
- Each orchestration plan belongs to a single user
- Foreign key: `userId` in A2AOrchestrationPlan entity

### A2AOrchestrationPlan → Analysis Results
- Mối quan hệ một-nhiều giữa A2AOrchestrationPlan và các thực thể kết quả phân tích
- Liên kết kế hoạch điều phối với kết quả phân tích từ các agent khác nhau
- Khóa ngoại: `correlationId` trong các thực thể kết quả phân tích khớp với `correlationId` trong A2AOrchestrationPlan

## Validation Rules

### PortfolioAnalyzerResult
- `userId` must be a valid UUID format
- `portfolioComposition.byAssetType` values must sum to approximately 100% (±0.1%)
- `analysisSummary` must be non-empty and less than 2000 characters
- `createdAt` must be a valid timestamp

### RiskAnalysisResult
- `userId` must be a valid UUID format
- `riskFactors.concentrationRisk.level` must be one of: low, medium, high
- `riskFactors.diversificationLevel` must be one of: poor, fair, good, excellent
- `warnings` array must contain at least one warning or be empty
- `riskSummary` must be non-empty and less than 2000 characters

### CorrelationAnalysisResult
- `userId` must be a valid UUID format
- `diversificationMetrics.assetClassDiversity` must be between 0 and 1
- `diversificationMetrics.sectorDiversity` must be between 0 and 1
- `diversificationMetrics.geographicDiversity` must be between 0 and 1
- `correlationFindings[].correlationCoefficient` must be between -1 and 1

### BehaviorAnalysisResult
- `userId` must be a valid UUID format
- `behavioralPatterns.herdingIndicator` must be between 0 and 1
- `behaviorSummary` must be non-empty and less than 2000 characters
- `patternDescriptions` array must not exceed 10 items

### A2AOrchestrationPlan
- `userId` phải là định dạng UUID hợp lệ
- Mảng `selectedAgents` phải chứa ít nhất một agent
- Mảng `executionOrder` phải trùng với độ dài của các agent được chọn
- `status` phải là một trong các giá trị: PLANNED, EXECUTING, COMPLETED, FAILED
- `createdAt` phải là timestamp hợp lệ
- `rationale` phải không rỗng và nhỏ hơn 5000 ký tự

## State Transitions

### A2AMessage
- `PENDING` → `PROCESSING` → `COMPLETED` or `FAILED` (standard A2A flow)
- New `COMPLIANCE_REVIEW` state when response needs compliance check
- New `COMPLIANCE_APPROVED` state when response passes compliance
- New `COMPLIANCE_REJECTED` state when response violates compliance rules

## Indexes

### PortfolioAnalyzerResult
- Index on `userId` for efficient user-specific queries
- Index on `createdAt` for chronological analysis retrieval

### RiskAnalysisResult
- Index on `userId` for efficient user-specific queries
- Index on `createdAt` for chronological analysis retrieval

### CorrelationAnalysisResult
- Index on `userId` for efficient user-specific queries
- Index on `createdAt` for chronological analysis retrieval

### BehaviorAnalysisResult
- Index on `userId` for efficient user-specific queries
- Index on `createdAt` for chronological analysis retrieval

### A2AMessage
- Index on `analysisType` for efficient agent-specific queries
- Index on `status` for monitoring purposes
- Index on `complianceFlagged` for compliance monitoring

### A2AOrchestrationPlan
- Chỉ mục trên `userId` cho truy vấn hiệu quả theo người dùng
- Chỉ mục trên `status` để giám sát tiến độ điều phối
- Chỉ mục trên `createdAt` để truy xuất kế hoạch điều phối theo thứ tự thời gian
- Chỉ mục trên `correlationId` để liên kết với các tin nhắn và kết quả liên quan