# Dataset Investigation

## 1. Dataset Overview

The project uses Apache access logs from the Zanbil e-commerce website (`zanbil.ir`).

- **Access logs:** `access.log` (~3.3 GB)
- **Hostname mapping:** `client_hostname.csv`
- **Hostname records:** 258,445 (excluding header)
- **Observed period:** January 22–26, 2019
- **Log format:** Apache Combined Log Format

## 2. Access Log Structure

Each record contains:

| Field | Description |
|---|---|
| IP | Client IP address |
| Timestamp | Request timestamp |
| Method | HTTP method |
| URL | Requested resource |
| Status | HTTP response status |
| Size | Response size |
| Referer | Referring page |
| User-Agent | Browser or bot identification |

## 3. Initial Observations

- The dataset contains both automated crawlers and regular visitors.
- Known crawler examples include AhrefsBot.
- User-Agent strings provide an initial indication of automated traffic but can be spoofed.
- Some client IP addresses have reverse DNS hostnames.
- Several IP addresses have unresolved hostnames (`Unknown host`).

## 4. Planned Processing

The next implementation steps are:

1. Parse Apache access logs into structured events.
2. Validate and normalize extracted fields.
3. Associate client IP addresses with available hostname information.
4. Prepare structured events for Kafka ingestion.
5. Extract behavioral features for bot detection.

## 5. Important Considerations

Reverse DNS and User-Agent information alone are insufficient to establish whether a visitor is a legitimate bot.

Detection will combine request metadata and behavioral analysis.