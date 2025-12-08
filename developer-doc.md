---
title: Developer Documentation
layout: default
nav_order: 4
---

# Developer Documentation

## Setup
- Clone the repository

```bash
git clone git@github.com:NYU-Cloud/Vulnerability-Detector.git
```

- Create a .env file with the following sections

```bash
# Frontend Configuration
FRONTEND_REDIRECT_URI=http://localhost:3000/auth/callback

# JWT Configuration
JWT_SECRET=<insert secret>
JWT_EXPIRATION_HOURS=24
REFRESH_TOKEN_EXPIRATION_DAYS=30

# Application Configuration
SECRET_KEY=<insert secret key>

# AWS Secrets Manager Configuration
# Set to 'true' to use AWS Secrets Manager for sensitive values
# Set to 'false' to use environment variables (for local development)
USE_AWS_SECRETS=false
AWS_REGION=us-east-1

# Database Configuration
# DATABASE_URI=postgresql://<user>:<password>@host:5432/<dbname>

# GitHub OAuth Configuration
# Create a GitHub App in the organization and add these credentials
GITHUB_CLIENT_ID=<insert client id>
GITHUB_OAUTH_AUTHORIZE_URL=https://github.com/login/oauth/authorize
GITHUB_OAUTH_TOKEN_URL=https://github.com/login/oauth/access_token
GITHUB_API_BASE_URL=https://api.github.com
GITHUB_OAUTH_SCOPE=read:user user:email repo workflow
GITHUB_REDIRECT_URI=http://localhost:8080/auth/github/callback
GITHUB_CLIENT_SECRET=<insert client secret>
```

## Folder Structure

```bash
.
├── backend
│   ├── alembic.ini
│   ├── app
│   │   ├── auth.py
│   │   ├── auth_routes.py
│   │   ├── auth_utils.py
│   │   ├── config.py
│   │   ├── db.py
│   │   ├── db_utils.py
│   │   ├── __init__.py
│   │   ├── models.py
│   │   ├── routes.py
│   │   ├── secrets_manager.py
│   │   ├── suggestions_routes.py
│   │   └── token_storage.py
│   ├── Dockerfile
│   ├── Dockerfile.cdk
│   ├── migrations
│   │   ├── env.py
│   │   ├── README
│   │   ├── script.py.mako
│   │   └── versions
│   │       └── 001_add_users_and_suggestions.py
│   ├── requirements.txt
│   ├── test_token.html
│   ├── verify_auth.sh
│   └── wsgi.py
├── frontend
│   ├── app.js
│   ├── index.html
│   └── server.py
├── infra
│   ├── app.py
│   ├── cdk.context.json
│   ├── cdk.json
│   ├── COGNITO_SETUP.md
│   ├── complete_github_setup.sh
│   ├── get-cognito-config.sh
│   ├── GITHUB_OAUTH_AUTOMATION.md
│   ├── infra
│   │   ├── infra_stack.py
│   │   └── __init__.py
│   ├── lambda_functions
│   │   └── update_github_oauth.py
│   ├── requirements.txt
│   ├── run_migration.sh
│   └── SECRETS_CDK_SETUP.md
├── README.md
└── worker
    ├── db_models.py
    ├── Dockerfile
    ├── main.py
    └── requirements.txt

10 directories, 44 files
```

## Cloud Deployment
- **Infrastructure:** `cdk deploy` in `infra/` directory
- **Backend:** Docker image built and pushed to ECR automatically
- **Worker:** Docker image built and pushed to ECR automatically
- **Frontend:** S3 deployment via CDK (automatic on stack update)
- **Database Migrations:** Alembic migrations run manually or via Lambda