# CSIC 2010 Dataset Investigation

## 1. Overview

The CSIC 2010 dataset is used for HTTP-based web attack detection.

- File: `csic_database.csv`
- Format: CSV
- Total records: 61,065
- Normal requests: 36,000
- Anomalous requests: 25,065
- Location: `data/raw/csic/`

## 2. Dataset Features

The dataset contains HTTP request information:

- HTTP method
- User-Agent
- HTTP headers
- Cookies
- Content type
- Request body
- Requested URL
- Classification label

## 3. Classification

The dataset provides two equivalent representations of the class:

| Label | Meaning | Records |
|---|---|---:|
| 0 | Normal | 36,000 |
| 1 | Anomalous | 25,065 |

The first unnamed CSV column contains the textual labels (`Normal`, `Anomalous`), while `classification` contains their numeric equivalents.

## 4. Data Quality Observations

Missing values were identified:

| Field | Missing records |
|---|---:|
| Accept | 397 |
| content-type | 43,088 |
| lenght | 43,088 |
| content | 43,088 |

The missing request-body-related fields may correspond to requests without a body, particularly GET requests. This should be verified during preprocessing.

## 5. Potential Usage

- HTTP request analysis
- Normal and anomalous traffic classification
- URL and payload inspection
- Feature extraction for machine learning
- Web attack detection

## 6. Limitations

- Historical dataset from 2010.
- Generated traffic may not represent modern production environments.
- Class distribution is imbalanced.
- Missing HTTP fields must be handled during preprocessing.

## 7. Conclusion

CSIC 2010 provides labeled HTTP request data that complements the Kaggle Apache access logs used for bot behavior analysis.

It will serve as a dataset for investigating and evaluating the web attack detection component.
