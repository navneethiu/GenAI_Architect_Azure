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

#### Yahoo Finance News:
- No additional setup required!
- Uses LangChain's built-in Yahoo Finance integration
- Provides reliable financial news data for publicly traded companies

### Environment Setup

Create `.env` file (recommended approach):
```bash
AZURE_OPENAI_ENDPOINT=https://your-resource-name.openai.azure.com/
AZURE_OPENAI_API_KEY=your-azure-openai-api-key-here
AZURE_OPENAI_DEPLOYMENT=gpt-4o
AZURE_OPENAI_API_VERSION=2024-02-15-preview
```
## Running the Application

### Command Line Interface:
```bash
python market_sentiment_analyzer.py
```

### MLflow Tracking Dashboard:
```bash
mlflow ui --backend-store-uri http://20.75.92.162:5000/
```
Access at: http://20.75.92.162:5000/

## Sample Usage

```python
from market_sentiment_analyzer import MarketSentimentAnalyzer

# Initialize with your Azure OpenAI credentials
analyzer = MarketSentimentAnalyzer(
    azure_endpoint="https://your-resource.openai.azure.com/",
    azure_api_key="your-api-key",
    deployment_name="gpt-4o"  # Your actual deployment name
)

# Analyze a company
result = analyzer.process_company("Microsoft Corporation")
print(json.dumps(result, indent=2))
```

## Implementation Details

### Architecture Decisions:

1. **Yahoo Finance Integration**: Chose Yahoo Finance over web search APIs for more reliable financial news without additional API costs or rate limiting concerns

2. **Stock Code Extraction**: Implemented multi-tier approach:
   - Direct lookup table for major companies
   - Partial matching for company name variations  
   - yfinance fallback for additional coverage

3. **Error Handling**: Comprehensive fallback mechanisms at each stage to ensure the pipeline continues even with partial failures

4. **MLflow Integration**: Complete tracking of all processing steps, errors, and performance metrics for debugging and optimization

### Challenges Encountered & Solutions:

1. **Azure OpenAI Parameter Changes**: Updated to use newer LangChain parameter names (`openai_api_key`, `azure_deployment`, `openai_api_version`)

2. **News Source Reliability**: Switched from web search to Yahoo Finance for more consistent financial news coverage

3. **Token Management**: Implemented content truncation to stay within OpenAI token limits while preserving key information

4. **Rate Limiting**: Added retry logic and delays for API calls to handle temporary failures

5. **Stock Code Coverage**: Expanded lookup table through testing with various company names and edge cases

## File Structure

```
├── market_sentiment_analyzer.py    # Core analysis pipeline
├── requirements.txt               # Python dependencies
├── sample_output.json            # Example Microsoft analysis result
├── .env.template                 # Environment setup template
├── README.md                     # This documentation
└── mlruns/                      # MLflow tracking data (created on first run)
```

## Output Format

Returns structured JSON with comprehensive analysis:

```json
{
  "company_name": "Microsoft",
  "stock_code": "MSFT", 
  "newsdesc": "Summary of recent financial news and key developments...",
  "sentiment": "Positive|Negative|Neutral",
  "people_names": ["Satya Nadella", "Amy Hood"],
  "places_names": ["Redmond", "Washington", "United States"],
  "other_companies_referred": ["Amazon", "Google", "OpenAI"],
  "related_industries": ["Cloud Computing", "AI", "Enterprise Software"],
  "market_implications": "Detailed analysis of potential market impact...",
  "confidence_score": 0.89
}
```

## MLflow Tracking

Comprehensive monitoring includes:
- **Input Parameters**: Company name, stock code, processing timestamp
- **Processing Steps**: Stock extraction success/failure, news fetching results, sentiment analysis
- **Performance Metrics**: Processing time, content length, API response times
- **Error Tracking**: Detailed error logs with context for debugging
- **Result Quality**: Confidence scores, entity extraction counts, analysis completeness

View detailed logs:
```bash
mlflow ui --backend-store-uri http://20.75.92.162:5000/
```

## Testing Results

Extensively tested with major companies:
- **Microsoft**: High confidence (0.89), comprehensive entity extraction
- **Apple**: Reliable sentiment classification, good market implications
- **Tesla**: Strong news coverage, detailed competitive analysis
