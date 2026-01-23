# Hướng dẫn nhanh: Triển khai các A2A Agents cho phân tích tài chính

## Tổng quan

Hướng dẫn này giải thích cách thiết lập và chạy các agent phân tích tài chính A2A cung cấp phân tích danh mục đầu tư, đánh giá rủi ro, phân tích tương quan và hiểu biết về hành vi. Các agent được xây dựng bằng framework LangChain và tích hợp với các nhà cung cấp LLM bên ngoài như đã nêu trong yêu cầu kỹ thuật.

## Điều kiện tiên quyết

- Node.js v20+ (như đã nêu trong TECH.md)
- Trình quản lý gói pnpm
- Cơ sở dữ liệu MySQL (như đã nêu trong TECH.md)
- Quyền truy cập API Qwen 3 (như đã nêu trong TECH.md), có khả năng mở rộng sang các nhà cung cấp khác như OpenAI
- Khóa API Qwen 3 hợp lệ (có khả năng cấu hình các nhà cung cấp khác)

## Environment Setup

1. **Clone the repository** (if not already done):
   ```bash
   git clone <repository-url>
   cd finance-agent-app/backend
   ```

2. **Install dependencies**:
   ```bash
   pnpm install
   ```

3. **Configure environment variables**:
   Copy the example environment file and update with your settings:
   ```bash
   cp .env.example .env
   ```

4. **Update the .env file** with required configurations:
   ```bash
   # Database Configuration
   DB_HOST=localhost
   DB_PORT=3306
   DB_NAME=finance_agent_db
   DB_USERNAME=your_username
   DB_PASSWORD=your_password

   # LLM Provider Configuration (Qwen 3 as primary, with OpenAI extensibility)
   QWEN_API_KEY=your_qwen_api_key  # Primary LLM provider as per TECH.md
   AI_PROVIDER_BASE_URL=https://dashscope.aliyuncs.com/compatible-mode/v1  # Qwen 3 API endpoint
   AI_MODEL=qwen-max  # Qwen 3 model as per TECH.md
   # Alternative: For OpenAI extension later:
   # OPENAI_API_KEY=your_openai_api_key
   # AI_PROVIDER_BASE_URL=https://api.openai.com/v1
   # AI_MODEL=gpt-4o-mini

   # Application Configuration
   JWT_SECRET=your_jwt_secret
   PORT=3000
   ```

## Running the Application

1. **Start the database** (ensure MySQL is running)

2. **Run database migrations**:
   ```bash
   pnpm run migrate
   ```

3. **Start the backend service**:
   ```bash
   pnpm run start:dev
   ```

4. **Verify the service is running**:
   Navigate to `http://localhost:3000/api/health` to check the health status

## Using the A2A Financial Agents

### Registering Agents

The system automatically registers the four financial analysis agents when the service starts:

- Portfolio Analyzer Agent: Handles portfolio composition analysis
- Risk Assessment Agent: Evaluates portfolio risk factors
- Correlation Analysis Agent: Assesses portfolio diversification
- Behavior Analysis Agent: Identifies behavioral patterns
- A2A Orchestrator Agent: Agent thông minh sử dụng LLM để phân tích yêu cầu người dùng và ra quyết định điều phối thông minh

### Making Analysis Requests

#### Via API

1. **Authenticate** by obtaining a JWT token through the auth endpoints
2. **Send a request** to the A2A messaging endpoint:

```bash
curl -X POST http://localhost:3000/api/a2a/message \
  -H "Authorization: Bearer YOUR_JWT_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "targetAgentId": "portfolio-analyzer",
    "action": "analyze",
    "payload": {
      "portfolio": [
        {
          "symbol": "AAPL",
          "quantity": 10,
          "price": 150.00,
          "assetType": "stock",
          "currency": "USD"
        }
      ]
    }
  }'
```

#### Via Chat Interface

The agents integrate with the existing chat system. Simply ask financial analysis questions in the chat interface, and the system will route appropriate requests to the specialized agents.

### Available Agent Capabilities

1. **Portfolio Analyzer Agent**
   - Analyzes portfolio composition by asset type, symbol, and currency
   - Provides descriptive analysis of portfolio structure
   - Response time: Under 10 seconds for portfolios with <50 assets

2. **Risk Assessment Agent**
   - Evaluates structural risks in portfolio allocation
   - Identifies concentration risks and diversification concerns
   - Provides risk warnings without investment recommendations

3. **Correlation Analysis Agent**
   - Assesses portfolio diversification across asset classes
   - Identifies potential correlation risks
   - Generates diversification recommendations (descriptive only)

4. **Behavior Analysis Agent**
   - Identifies behavioral patterns in trading history
   - Provides neutral observations about investment behavior
   - Highlights potential behavioral biases

## Testing the Implementation

### Unit Tests

Run unit tests for the new agent implementations:

```bash
pnpm run test:unit --testPathPattern=agents
```

### Integration Tests

Run integration tests for A2A communication:

```bash
pnpm run test:integration --testPathPattern=a2a
```

### End-to-End Tests

Run end-to-end tests for the complete analysis workflow:

```bash
pnpm run test:e2e
```

## Troubleshooting

### Common Issues

1. **Agent not responding**: Verify that the LLM provider API key is correctly configured
2. **Slow analysis**: Check that the portfolio size is within performance limits (<100 assets)
3. **Authentication errors**: Ensure JWT token is properly obtained and included in requests

### Logs

Check application logs in the `finance-agent-app/backend/logs/` directory for detailed error information.

## Performance Expectations

- Portfolio analysis: <10 seconds for portfolios with <50 assets
- Risk assessment: <15 seconds for portfolios with <100 assets
- Complete analysis suite: <30 seconds for portfolios with <100 assets
- Concurrent request handling: Supports multiple simultaneous analysis requests

## Compliance Notes

- All agent responses are descriptive, not advisory (no buy/sell recommendations)
- Financial analysis is read-only with no transaction capabilities
- All analysis requests and responses are logged for compliance auditing