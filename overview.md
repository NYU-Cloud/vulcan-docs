---
title: Overview
---

# Overview

This page contains details on  the **Problem Statement**, **Components & Architecture**, and **API Endpoints**.

---

## Problem Statement

VulCAN is a Vulnerability Code ANalyser that will allow users to submit GitHub repository URLs for security scanning. The application will analyze the code files within this repository for common vulnerabilities such as SQL injection, cross-site scripting (XSS), hardcoded credentials and more with Semgrep. Upon completion, it will generate a report highlighting potential security issues, the criticality levels and provide recommendations for fixing them using GenAI.

---

## Components & Architecture

### System Overview  
Explain the system at a high level.

### Architecture Diagram  
Add architecture diagram

### Components
- API Server  
- Worker / Scheduler  
- Database / Storage  
- External Integrations  

### Data Flow  
how data moves through the system.

---

## API Endpoints

### Format
Each endpoint should include:

- Method  
- Path  
- Purpose  
- Request  
- Response  

---

### Example Endpoints

#### `GET /health`
Checks if the service is running.

**Response**
```json
{ "status": "ok" }

