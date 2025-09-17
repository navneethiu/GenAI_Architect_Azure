# Real-Time Market Sentiment Analyzer

A LangChain-powered pipeline that analyzes market sentiment for companies using Azure OpenAI GPT-4o and Yahoo Finance news data. This project was developed as part of an Azure GenAI architect training assignment.

## Project Overview

After working through various implementation challenges and testing different approaches, this analyzer provides comprehensive sentiment analysis by:
- Extracting stock codes using an expanded lookup table with fallback methods
- Fetching real-time financial news via Yahoo Finance (no additional API costs)
- Using Azure OpenAI GPT-4o mini for structured sentiment analysis with confidence scoring
- Tracking all operations with MLflow for complete debugging and monitoring

## Key Features

- **Robust Company Analysis**: Handles various company name formats and edge cases
- **Smart Stock Code Detection**: Multiple fallback methods for ticker symbol extraction  
- **Yahoo Finance Integration**: Reliable financial news without additional API requirements
- **AI-Powered Sentiment Analysis**: Structured JSON output with confidence scoring
- **Complete MLflow Tracking**: Detailed logging for debugging and optimization
- **Error Handling**: Comprehensive fallback responses for various failure scenarios

## Setup Instructions

### Prerequisites
```bash
Python 3.10+
pip install -r requirements.txt
```
### Required Configuration

#### Azure OpenAI Setup:
- Create Azure OpenAI resource in Azure Portal
- Deploy GPT-4o model (note your deployment name)
- Get endpoint URL and API key from Azure portal
- Ensure sufficient quota allocation for your use case

