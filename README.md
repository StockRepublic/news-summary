# News Summary API Documentation

## Overview
The News Summary API provides access to curated news summaries related to financial instruments and market sectors. The service processes news articles, extracts key information, and delivers concise summaries.

## Base URL
```
/v1/news
```

## Authentication
The API requires HTTP Basic Authentication:

```
curl -X GET "https://beta.pulse.stockrepublic.io/v1/news?isin=US88160R1014" -u username:password
```

Required credentials:
- **Username**: Your API username
- **Password**: Your API password

Although the API is stateless and doesn't store user context between requests, valid authentication credentials must be provided with each request.

## Endpoints

### Get News Summaries
Retrieve news summaries related to specific financial instruments or market sectors.

#### Request Format
```
GET /v1/news?isin={isin}&sector={sector}
```

#### Parameters

| Parameter | Type | Description | Example |
|-----------|------|-------------|---------|
| isin | string | International Securities Identification Number for specific instruments | US88160R1014 |
| sector | string | Market sector identifier | basic-materials |
| experience_level | integer | Optional user experience level (1-3) | 2 |

Parameters can be repeated for multiple values:
```
/v1/news?isin=US123456781&isin=US132456782&isin=US123456783
```

#### Response Format
```json
{
  "count": integer,
  "next": string,
  "previous": string,
  "results": [
    {
      "id": string,
      "sources": array,
      "headline": string,
      "summary": string,
      "created_at": string
    }
  ]
}
```

| Field | Description |
|-------|-------------|
| count | Total number of results |
| next | URL for the next page of results (null if no more pages) |
| previous | URL for the previous page of results (null if first page) |
| results | Array of news items |
| id | Unique identifier for the news item |
| sources | Array of source information including provider, content, and relevance data |
| headline | Concise headline of the news |
| summary | Summarized content of the news item |
| created_at | Timestamp when the summary was created |

#### Example Request
```
GET /v1/news?isin=US88160R1014
```

#### Example Response
```json
{
  "count": 4,
  "next": null,
  "previous": null,
  "results": [
    {
      "id": "7eaed82f-643f-4e81-bed5-4a286239ff4e",
      "sources": [...],
      "headline": "India Blocks BYD's Market Entry Due to Security Concerns",
      "summary": "India's Minister Piyush Goyal confirms that the Chinese electric vehicle manufacturer BYD will not be allowed to enter the Indian market, citing security concerns as the primary reason.",
      "created_at": "2025-04-09T15:43:29.130193Z"
    },
    {
      "id": "7329da32-e2d4-4da8-b340-9bb6a624926c",
      "sources": [...],
      "headline": "Tesla Featured on Benchmark Capital's 'Best Ideas' List",
      "summary": "Benchmark Capital has included Tesla on its list of top investment ideas, indicating the company's strong market position and potential for future growth.",
      "created_at": "2025-04-09T15:43:17.787130Z"
    }
  ]
}
```

## Source Details
Each news item contains detailed source information:

```json
"sources": [
  {
    "id": string,
    "companies": array,
    "provider": string,
    "provider_id": string,
    "title": string,
    "content": string,
    "language": string,
    "relevance_score": integer,
    "sectors": array,
    "industry": array
  }
]
```

## Filtering Options
- Filter by multiple ISINs
- Filter by sector

## Error Handling
The API returns standard HTTP status codes:
- 200: Success
- 400: Bad request
- 404: Resource not found
- 500: Server error

## Implementation Notes
- The API is stateless - all required information is passed via URL parameters
- Results are sorted by relevance and recency
- News content may be available in multiple languages
- Each news item contains relevance scores to indicate importance
