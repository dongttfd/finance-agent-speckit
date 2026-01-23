# 006-a2a-agent-implementation

Implementation of A2A (Agent-to-Agent) financial analysis agents for portfolio analysis, risk assessment, correlation analysis, and behavioral insights.

## Overview

Milestone này triển khai bốn agent phân tích tài chính chuyên dụng sử dụng framework LangChain như đã nêu trong TECH.md. Các agent chủ yếu tích hợp với Qwen 3 làm nhà cung cấp LLM như đã nêu trong TECH.md, với thiết kế mô-đun cho phép mở rộng dễ dàng sang các nhà cung cấp khác như OpenAI trong tương lai. Các agent tuân theo giao thức A2A (Agent-to-Agent) bắt buộc với tin nhắn JSON tiêu chuẩn.

## Components

- **Portfolio Analyzer Agent**: Analyzes portfolio composition by asset type, symbol, and currency
- **Risk Assessment Agent**: Evaluates structural risks including concentration and diversification
- **Correlation Analysis Agent**: Assesses portfolio diversification across asset classes and sectors
- **Behavior Analysis Agent**: Identifies behavioral patterns in investment history

## Documentation

- [Feature Specification](spec.md) - Detailed requirements and user stories
- [Implementation Plan](plan.md) - Technical approach and architecture
- [Research](research.md) - Technology analysis and decisions
- [Data Model](data-model.md) - Entity definitions and relationships
- [Quick Start Guide](quickstart.md) - Setup and usage instructions
- [API Contracts](contracts/) - API specifications for agent communication
- [Implementation Checklist](checklists/implementation-checklist.md) - Development tasks tracking

## Architecture

The implementation extends the existing A2A orchestration system with specialized financial analysis capabilities while maintaining compliance with financial regulations - ensuring all analysis is descriptive rather than advisory.

## Compliance

All agents are designed to provide descriptive analysis only, with strict compliance measures to prevent any investment advice from being generated.