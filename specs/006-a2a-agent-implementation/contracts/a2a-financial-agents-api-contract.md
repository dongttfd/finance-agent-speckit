# API Contract: A2A Financial Agents

## Tổng quan

Tài liệu này xác định hợp đồng API cho các agent phân tích tài chính A2A (Agent-to-Agent). Các agent này cung cấp phân tích danh mục đầu tư, đánh giá rủi ro, phân tích tương quan và hiểu biết về hành vi bằng cách sử dụng Qwen 3 làm nhà cung cấp LLM chính như đã nêu trong yêu cầu kỹ thuật, với thiết kế cho phép mở rộng dễ dàng sang các nhà cung cấp khác như OpenAI trong tương lai. Đặc biệt, A2A Orchestrator Agent là một agent thông minh sử dụng LLM để phân tích yêu cầu người dùng và ra quyết định điều phối các agent khác một cách thông minh.

## Base URL

`http://localhost:3000/api/a2a` (development)  
`https://your-domain.com/api/a2a` (production)

## Authentication

All endpoints require a valid JWT token in the Authorization header:
```
Authorization: Bearer <jwt_token>
```

## Common Data Types

### PortfolioItem
```json
{
  "symbol": "string (required)",
  "quantity": "number (required)",
  "price": "number (required)",
  "assetType": "string (required) - stock, bond, crypto, fund, cash, etc.",
  "currency": "string (required) - USD, EUR, GBP, etc.",
  "purchaseDate": "string (optional) - ISO 8601 date format"
}
```

### TransactionRecord
```json
{
  "id": "string (required) - unique transaction identifier",
  "symbol": "string (required)",
  "type": "string (required) - BUY, SELL, DIVIDEND, etc.",
  "quantity": "number (required)",
  "price": "number (required)",
  "timestamp": "string (required) - ISO 8601 datetime format",
  "currency": "string (required)"
}
```

## Agent Endpoints

### Portfolio Analyzer Agent

#### POST /portfolio-analyzer/analyze

Analyze the composition of a portfolio by asset type, symbol, and currency.

**Request:**
```json
{
  "portfolio": [
    {
      "symbol": "AAPL",
      "quantity": 10,
      "price": 150.00,
      "assetType": "stock",
      "currency": "USD"
    }
  ],
  "options": {
    "groupBy": ["assetType", "symbol", "currency"],
    "includeHistorical": false
  }
}
```

**Response (Success 200):**
```json
{
  "id": "string - unique analysis result ID",
  "analysisType": "portfolio-composition",
  "summary": "string - descriptive summary of portfolio composition",
  "composition": {
    "byAssetType": {
      "stock": 65.5,
      "bond": 20.0,
      "crypto": 10.0,
      "cash": 4.5
    },
    "bySymbol": {
      "AAPL": 15.2,
      "MSFT": 12.8,
      "GOOGL": 10.5
    },
    "byCurrency": {
      "USD": 85.0,
      "EUR": 10.0,
      "GBP": 5.0
    }
  },
  "insights": ["array of descriptive insights"],
  "timestamp": "string - ISO 8601 datetime"
}
```

**Response (Validation Error 400):**
```json
{
  "error": "Bad Request",
  "message": "string - validation error details",
  "details": ["array of specific validation errors"]
}
```

### Risk Assessment Agent

#### POST /risk-assessment/analyze

Evaluate structural risks in a portfolio including concentration, diversification, and currency exposure.

**Request:**
```json
{
  "portfolio": [
    {
      "symbol": "AAPL",
      "quantity": 10,
      "price": 150.00,
      "assetType": "stock",
      "currency": "USD"
    }
  ],
  "options": {
    "includeCurrencyRisk": true,
    "includeSectorRisk": true,
    "includeConcentrationRisk": true
  }
}
```

**Response (Success 200):**
```json
{
  "id": "string - unique analysis result ID",
  "analysisType": "risk-assessment",
  "summary": "string - descriptive summary of risk assessment",
  "riskFactors": {
    "concentrationRisk": {
      "level": "string - low, medium, high, critical",
      "details": [
        {
          "factor": "string - concentration factor",
          "value": "number - percentage or metric",
          "description": "string - description of risk"
        }
      ]
    },
    "diversificationLevel": "string - poor, fair, good, excellent",
    "currencyExposure": {
      "dominantCurrency": "string - currency code",
      "exposurePercentage": "number - percentage in dominant currency",
      "otherCurrencies": {
        "EUR": 15.0,
        "GBP": 5.0
      }
    }
  },
  "warnings": ["array of risk warnings"],
  "timestamp": "string - ISO 8601 datetime"
}
```

### Correlation Analysis Agent

#### POST /correlation-analysis/analyze

Analyze diversification and correlation patterns in a portfolio.

**Request:**
```json
{
  "portfolio": [
    {
      "symbol": "AAPL",
      "quantity": 10,
      "price": 150.00,
      "assetType": "stock",
      "currency": "USD",
      "sector": "Technology"
    }
  ],
  "options": {
    "byAssetClass": true,
    "bySector": true,
    "byGeography": true
  }
}
```

**Response (Success 200):**
```json
{
  "id": "string - unique analysis result ID",
  "analysisType": "correlation-analysis",
  "summary": "string - descriptive summary of diversification assessment",
  "diversificationMetrics": {
    "assetClassDiversity": "number - 0 to 1 score",
    "sectorDiversity": "number - 0 to 1 score",
    "geographicDiversity": "number - 0 to 1 score"
  },
  "correlationFindings": [
    {
      "assetGroup1": "string - first group name",
      "assetGroup2": "string - second group name",
      "correlationCoefficient": "number - -1 to 1",
      "significance": "string - low, medium, high"
    }
  ],
  "diversificationRecommendations": ["array of descriptive recommendations"],
  "timestamp": "string - ISO 8601 datetime"
}
```

### Behavior Analysis Agent

#### POST /behavior-analysis/analyze

Analyze behavioral patterns in investment history.

**Request:**
```json
{
  "transactionHistory": [
    {
      "id": "tx-123",
      "symbol": "AAPL",
      "type": "BUY",
      "quantity": 10,
      "price": 150.00,
      "timestamp": "2023-01-15T10:30:00Z",
      "currency": "USD"
    }
  ],
  "options": {
    "includeFrequencyAnalysis": true,
    "includeTimingAnalysis": true,
    "includeRiskAnalysis": true
  }
}
```

**Response (Success 200):**
```json
{
  "id": "string - unique analysis result ID",
  "analysisType": "behavior-analysis",
  "summary": "string - descriptive summary of behavioral observations",
  "behavioralPatterns": {
    "frequencyPattern": "string - day-trader, swing-trader, buy-hold, etc.",
    "timingPattern": "string - market-timer, dollar-cost-averager, etc.",
    "riskPattern": "string - conservative, moderate, aggressive",
    "herdingIndicator": "number - 0 to 1, tendency to follow trends"
  },
  "patternDescriptions": ["array of descriptive pattern summaries"],
  "behavioralObservations": ["array of neutral behavioral observations"],
  "timestamp": "string - ISO 8601 datetime"
}
```

## General A2A Endpoints

### POST /message

Send a message between agents using the A2A protocol.

**Request:**
```json
{
  "targetAgentId": "string (required) - ID of the target agent",
  "action": "string (required) - action to perform",
  "payload": "object (required) - data to send to the agent",
  "correlationId": "string (optional) - ID for tracking related messages",
  "chatMessageId": "string (optional) - associated chat message ID"
}
```

**Response (Success 201):**
```json
{
  "id": "string - unique message ID",
  "sourceAgentId": "string - ID of the source agent",
  "destinationAgentId": "string - ID of the destination agent",
  "messageType": "string - REQUEST, RESPONSE, ERROR",
  "action": "string - action performed",
  "payload": "object - response data from the agent",
  "status": "string - PENDING, PROCESSING, COMPLETED, FAILED",
  "correlationId": "string - correlation ID for tracking",
  "createdAt": "string - ISO 8601 datetime",
  "processedAt": "string - ISO 8601 datetime (if processed)"
}
```

### GET /agents

Get all available agents.

**Response (Success 200):**
```json
{
  "agents": [
    {
      "id": "string - unique agent ID",
      "name": "string - agent name",
      "description": "string - agent description",
      "endpointUrl": "string - agent endpoint URL",
      "isActive": "boolean - whether agent is active",
      "capabilities": "object - agent capabilities",
      "createdAt": "string - ISO 8601 datetime",
      "updatedAt": "string - ISO 8601 datetime"
    }
  ]
}
```

### GET /agent/{id}

Get a specific agent by ID.

**Response (Success 200):**
```json
{
  "id": "string - unique agent ID",
  "name": "string - agent name",
  "description": "string - agent description",
  "endpointUrl": "string - agent endpoint URL",
  "isActive": "boolean - whether agent is active",
  "capabilities": "object - agent capabilities",
  "createdAt": "string - ISO 8601 datetime",
  "updatedAt": "string - ISO 8601 datetime"
}
```

### GET /messages/agent/{agentId}

Get messages for a specific agent.

**Response (Success 200):**
```json
[
  {
    "id": "string - unique message ID",
    "sourceAgentId": "string - ID of the source agent",
    "destinationAgentId": "string - ID of the destination agent",
    "messageType": "string - REQUEST, RESPONSE, ERROR",
    "action": "string - action performed",
    "payload": "object - message payload",
    "status": "string - PENDING, PROCESSING, COMPLETED, FAILED",
    "correlationId": "string - correlation ID for tracking",
    "createdAt": "string - ISO 8601 datetime",
    "processedAt": "string - ISO 8601 datetime (if processed)"
  }
]
```

## Error Responses

All error responses follow the same format:

```json
{
  "error": "string - error type",
  "message": "string - error description",
  "statusCode": "number - HTTP status code",
  "timestamp": "string - ISO 8601 datetime",
  "path": "string - requested path"
}
```

## Compliance Requirements

1. All agent responses must be descriptive, not advisory (no buy/sell recommendations)
2. All analysis is read-only with no transaction capabilities
3. All requests and responses are logged for compliance auditing
4. Financial data is processed securely with appropriate access controls