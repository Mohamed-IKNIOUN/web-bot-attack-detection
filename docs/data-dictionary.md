# Data Dictionary — Silver Layer

## 1. Overview

The Silver layer contains cleaned, validated and normalized datasets.

Since the Zanbil Apache access logs and CSIC 2010 dataset serve different purposes, they are maintained as two separate Silver tables.

- `silver_access_logs`: Request-level access logs used for bot behavior analysis.
- `silver_csic_requests`: Labeled HTTP requests used for supervised web attack detection.

## 2. Purpose of Transformation Rules

The transformation rules defined in this document are intended for the developers responsible for implementing the data ingestion and processing pipeline---(HANANIII ).

They describe how raw records from the Bronze layer must be cleaned, validated and converted before being stored in the Silver layer.

These rules serve as a shared data preparation contract, ensuring that all team members apply consistent transformations and handle missing or invalid values in the same way.

### Where Are They Applied?

Transformations are applied during the Bronze-to-Silver processing stage:

```text
Raw Dataset
     |
     v
Bronze Layer
(Original data)
     |
     v
Transformation / Cleaning
(Apply documented rules)
     |
     v
Silver Layer
(Cleaned and normalized data)
```

### Examples

| Raw value | Transformation | Silver value |
|---|---|---|
| `"-"` | Replace missing marker | `NULL` |
| `"200"` | Convert to integer | `200` |
| `"30577"` | Convert to BIGINT | `30577` |
| `"Unknown host"` | Replace unresolved hostname | `NULL` |
| Timestamp with `+0330` | Parse and preserve timezone | TIMESTAMP |

**Important:** The raw Bronze datasets must remain unchanged. All cleaning and normalization operations must produce new Silver records without modifying the original data.

## 3. silver_access_logs

**Source:** Zanbil Apache Access Logs  
**Granularity:** One row per HTTP request.  
**Purpose:** Analyze client behavior, identify automated traffic and extract IP-based behavioral features.

| Column | Data Type | Nullable | Description |
|---|---|---|---|
| ip | STRING | No | Client IP address |
| ts | TIMESTAMP | No | Request timestamp, preserving the original timezone |
| method | STRING | No | HTTP request method |
| url | STRING | No | Requested resource and query parameters |
| protocol | STRING | No | HTTP protocol version |
| status | INT | No | HTTP response status code |
| bytes | BIGINT | Yes | Response size in bytes |
| referrer | STRING | Yes | Referring URL |
| user_agent | STRING | Yes | Client User-Agent string |
| hostname | STRING | Yes | Reverse DNS hostname from client_hostname.csv |

### Transformation Rules

- Convert `-` values to NULL for optional fields.
- Parse timestamps using the original Apache timestamp format.
- Preserve the `+0330` timezone offset.
- Convert status codes to integers.
- Convert response sizes to BIGINT.
- Join hostname information using the client IP.
- Keep unresolved hostnames as NULL.

## 4. silver_csic_requests

**Source:** CSIC 2010 HTTP Dataset (`csic_database.csv`)  
**Granularity:** One row per HTTP request.  
**Purpose:** Train and evaluate supervised web attack detection models.

| Column | Data Type | Nullable | Description |
|---|---|---|---|
| request_id | STRING | No | Unique identifier generated during ingestion |
| method | STRING | No | HTTP request method |
| user_agent | STRING | Yes | Client User-Agent |
| pragma | STRING | Yes | HTTP Pragma header |
| cache_control | STRING | Yes | HTTP Cache-Control header |
| accept | STRING | Yes | Accepted content types |
| accept_encoding | STRING | Yes | Supported content encodings |
| accept_charset | STRING | Yes | Accepted character encodings |
| language | STRING | Yes | Preferred language |
| host | STRING | Yes | HTTP Host header |
| cookie | STRING | Yes | Request cookies |
| content_type | STRING | Yes | Request body media type |
| connection | STRING | Yes | Connection header |
| content_length | INT | Yes | Request body length |
| content | STRING | Yes | HTTP request body |
| original_classification | STRING | No | Original textual label |
| classification | INT | No | Binary label: 0 = Normal, 1 = Anomalous |
| url | STRING | No | Requested URL |

### Transformation Rules

- Rename `lenght` to `content_length`.
- Rename the unnamed first column to `original_classification`.
- Preserve original textual labels.
- Use `classification` as the numeric target variable.
- Convert empty optional fields to NULL.
- Generate a reproducible `request_id`.
- Separate URL and HTTP protocol during parsing if needed.
- Do not invent IP addresses, timestamps or response status codes.
- Preserve request bodies and query parameters for attack analysis.

## 5. Gold Layer

### gold_ip_behavior

**Source:** `silver_access_logs` only.  
**Granularity:** One row per client IP.

This table contains aggregated behavioral features used for bot detection.

| Feature | Data Type | Description |
|---|---|---|
| ip | STRING | Client IP address |
| total_requests | BIGINT | Total requests made by the IP |
| unique_urls | BIGINT | Number of distinct requested URLs |
| get_requests | BIGINT | Number of GET requests |
| post_requests | BIGINT | Number of POST requests |
| error_requests | BIGINT | Number of HTTP 4xx and 5xx responses |
| avg_bytes | DOUBLE | Average response size |
| requests_per_minute | DOUBLE | Average request frequency |
| unique_user_agents | BIGINT | Number of distinct User-Agent strings |

These features will be used for behavioral analysis and clustering, including KMeans.

## 6. Dataset Separation and Responsibilities

| Dataset | Silver Table | Main Purpose |
|---|---|---|
| Zanbil | silver_access_logs | Bot behavior detection |
| CSIC 2010 | silver_csic_requests | Web attack classification |

The two datasets are not merged because their available fields and classification objectives differ.

CSIC 2010 is used for supervised model training and evaluation. Zanbil access logs are used for IP-level behavioral analysis and bot detection.

## 7. Data Quality Principles

- Preserve original source information where possible.
- Use consistent naming conventions.
- Represent missing optional values as NULL.
- Validate data types before writing Silver records.
- Preserve original labels and their meanings.
- Avoid inventing unavailable fields.
- Keep raw datasets unchanged in the Bronze layer.