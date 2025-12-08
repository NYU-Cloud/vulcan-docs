---
title: GenAI Integration
layout: default
nav_order: 3
---

# GenAI Integration

## AI suggestions
VulCAN leverages GenAI models to provide automated remediation for vulnerabilities detected by the custom security scanner. The current integration uses Gemini (Free Tier) and is designed to scale efficiently using `threadpool` workers. AI-generated fixes and explanations are persisted in a relational database (RDS) as soon as they are ready.

Future support for other models, including Claude and OpenAI GPT models, is planned to provide flexibility and enhanced capabilities.

## Supported Models
### Gemini (Current Production Model)
- Used the Free Tier with flash 2.5 model.
- Provides high-quality code generation and explanation for detected vulnerabilities.
- Ideal for lightweight, cost-effective automation in the current setup.

#### Rate limits:
- **RPM:** 10 request per minute
- **TPM:** 250k tokens per minute
- **RPD:** 20 requests per day


### Future Scope: Claude & OpenAI
Integration with these models will allow the system to switch dynamically based on use-case or improvement in recommendations.

## Prompt Construction
For each vulnerability, the scanner generates a structured prompt for the GenAI model:
```
A security scanner detected a vulnerability ({vulnerability_type}) in the following code snippet 
from file {file_path}. The vulnerable section is indicated by '>>>'.

Vulnerable Code:
---
{code_snippet}
---

Please provide a safe, corrected version of the code and a brief explanation of the fix.
```
The `code_snippet` is fetched with the file and line number details from semgrep. It provides vulnerability type, file path, and vulnerable code snippet and instructs the model to return corrected code and brief explanation.

## Threadpool Worker Architecture
Threadpool workers are used to handle multiple GenAI requests concurrently. Each worker operates independently, generating fixes and explanations in parallel. This improves throughput and latency of vulnerability remediation and ensures adherence to model-specific rate limits (e.g., Gemini Free Tier concurrency constraints).

## Data Persistence
Once a GenAI response is received, it is immediately stored in RDS. This enables reliable storage and auditability of AI-generated fixes. This is then used in the `Scan History` tab where a user can see the code fix suggestion, explanation of the fix enabling the user to commit the proposed change. 