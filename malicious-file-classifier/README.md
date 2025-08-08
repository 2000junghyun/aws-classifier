# Malicious file classifier

## Overview

This project implements an **AI-assisted malware analysis pipeline** triggered by file uploads to an AWS S3 bucket. It performs **static analysis** using multiple detection techniques to classify files as malicious or benign.

The core logic is built into an **AWS Lambda function**, which sequentially invokes modular analyzers such as **Threat Intelligence (TI) lookup**, **metadata classification**, **content signature comparison**, and **fuzzy hash similarity detection**.

## Tech Stack

- AWS Lambda
- Python 3.9
- S3 Event Trigger
- Static Analysis Techniques:
    - Threat Intelligence Matching
    - Metadata Classification
    - Content Signature Comparison
    - Fuzzy Hash Similarity Check
- Custom detection modules (`utils/`, `modules/`)
- Dataset: `fuzzy_hash_db/`

## Directory Structure

```
.
├── lambda_function.py              # Main Lambda entry point
├── run_lambda.py
├── fuzzy_hash_db/
└── utils/
     ├── static_analyzer/
     │   ├── ext_filter.py             # Extension-based filtering
     │   ├── mime_checker.py           # MIME type checker
     │   └── size_checker.py           # File size filter
     ├── ti_checker.py                 # Threat Intelligence lookup logic
     ├── metadata_classifier.py        # Core logic for metadata classification
     ├── content_signature_checker.py  # YARA-like signature comparison logic
     └── fuzzy_hash_analyzer.py        # Similarity detection via ssdeep
```

## How to Use

### Prerequisites

- Python 3.9+
- Install dependencies:

```bash
pip install -r requirements.txt
```

### Local Testing

```bash
python run_lambda.py
```

This simulates an S3 event using `test_files/event.json`.

### Deploy to AWS

1. Zip the Lambda handler and dependencies:
    
    ```bash
    bash
    CopyEdit
    zip -r lambda_package.zip lambda_function.py utils/ modules/ fuzzy_hash_db/
    
    ```
    
2. Upload to AWS Lambda and set S3 event as the trigger.

> Make sure to configure appropriate IAM roles and access policies for S3 read permissions.
> 

## Features / Main Logic

- **TI Lookup**
    
    Checks the uploaded file against threat intelligence databases.
    
- **Metadata Classification**
    
    Analyzes static features (size, extension, MIME type, etc.) and classifies the file using rule-based heuristics or ML-based logic.
    
- **Content Signature Matching**
    
    Compares the file's content against known malicious patterns.
    
- **Fuzzy Hash Comparison**
    
    Uses `ssdeep` hashing to measure similarity to known malware samples in the fuzzy hash database.
    
- **Extensible Architecture**
    
    Additional modules (e.g. AI-based analysis, ATS integration) can be easily added in the pipeline.
    

## Future Work

- Integrate **AI-based content analysis**
- Implement **ATS (Automated Threat Scoring) module**
- Add real-time alerting via SNS or Slack
- Provide a web frontend for file submission and result visualization

## Motivation / Impact

This project simulates a **cloud-native static malware analysis system** optimized for speed, modularity, and scalability. It offers a practical demonstration of **security automation**, combining **signature**, **heuristic**, and **intelligence-based** approaches in a serverless architecture.
