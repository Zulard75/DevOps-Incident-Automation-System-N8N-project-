# DevOps Incident Automation System

An automated incident detection and response platform that monitors application health, collects error logs, uses AI to summarize incidents, sends notifications, and tracks incident lifecycles.

## Overview

The **DevOps Incident Automation System** aims to reduce manual effort in identifying, investigating, and tracking application failures.

The system combines monitoring tools, a Node.js backend, n8n workflow automation, PostgreSQL, and AI-powered log analysis to create a centralized incident management workflow.

When a service experiences a failure, the system can receive an alert, collect relevant diagnostic information, generate an incident summary, notify the appropriate people, and maintain a record of the incident until resolution.

## Architecture

```mermaid
flowchart TD
    A[Application / Docker Container] --> B[Prometheus Monitoring]
    B --> C[Alertmanager]
    C --> D[n8n Webhook Workflow]
    D --> E[Node.js / Express API]
    E --> F[PostgreSQL Database]
    D --> G[Log Collection]
    G --> H[AI Incident Analysis]
    H --> I[Severity Classification]
    I --> J[Email / Slack Notification]
    E --> K[Incident Tracking]
    F --> K
```

## Tech Stack

| Technology | Purpose |
|---|---|
| Node.js | Backend runtime |
| Express.js | REST API development |
| n8n | Workflow automation and orchestration |
| PostgreSQL | Incident and event storage |
| Drizzle ORM | Database schema and queries |
| Docker | Application containerization |
| Docker Compose | Local multi-service orchestration |
| Prometheus | Metrics collection and alert rules |
| Grafana | Monitoring dashboards and visualization |
| LLM API | Incident summarization and suggested actions |
| Git and GitHub | Version control and documentation |

## Key Features

### 1. Automated Incident Detection
- Monitor application health and service availability.
- Detect HTTP errors and service failures.
- Receive monitoring alerts through webhooks.

### 2. Log Collection and Analysis
- Collect relevant application logs and error details.
- Associate diagnostic information with incidents.
- Generate concise AI-powered incident summaries.

### 3. Incident Severity Classification
- Classify incidents as Low, Medium, High, or Critical.
- Use configurable rules and available diagnostic context.
- Prioritize incidents that require immediate investigation.

### 4. Workflow Automation with n8n
- Trigger workflows when an alert is received.
- Validate and enrich incoming incident events.
- Coordinate log collection, AI analysis, database updates, and notifications.
- Handle workflow failures and retry eligible operations.

### 5. Incident Lifecycle Management
- Track incidents from Open to Investigating to Resolved.
- Store timestamps, service details, severity, and status changes.
- Maintain an audit trail of incident activity.
- Deduplicate repeated alerts for the same underlying incident.

### 6. Notifications and Suggested Remediation
- Send incident alerts through email or Slack.
- Include the incident summary, severity, and relevant diagnostics.
- Suggest potential troubleshooting steps.
- Require human approval before executing risky remediation actions.

## Incident Workflow

1. An application failure or monitoring alert is detected.
2. The alert is delivered to an n8n webhook.
3. The workflow validates the event and gathers diagnostic information.
4. The backend creates or updates the corresponding incident.
5. Relevant logs are analyzed and summarized using an LLM.
6. The incident severity and recommended actions are recorded.
7. Notifications are sent to the configured recipients.
8. The incident remains tracked until it is resolved.

## Planned Project Structure

```text
devops-incident-automation/
├── backend/
│   ├── src/
│   │   ├── routes/
│   │   ├── controllers/
│   │   ├── services/
│   │   ├── middleware/
│   │   └── server.js
│   ├── drizzle/
│   ├── drizzle.config.js
│   ├── package.json
│   └── .env.example
├── monitoring/
│   ├── prometheus.yml
│   └── alert-rules.yml
├── n8n/
│   └── workflows/
├── database/
│   └── schema.sql
├── sample-app/
├── docker-compose.yml
├── .gitignore
└── README.md
```

The structure may evolve as the implementation progresses.

## Getting Started

### Prerequisites

- Node.js and npm
- Docker Desktop or Docker Engine
- Docker Compose
- Git
- An LLM API key for AI-powered analysis
- Email or Slack credentials for notifications, if enabled

### Installation

**1. Clone the repository**

```bash
git clone <your-repository-url>
cd devops-incident-automation
```

**2. Configure environment variables**

Create the required environment files using the provided `.env.example` templates.

Typical configuration values may include:

```env
PORT=4000
DATABASE_URL=postgresql://user:password@localhost:5432/incidents
N8N_WEBHOOK_URL=http://localhost:5678/webhook/incident
LLM_API_KEY=your_api_key
```

Use secure credentials in your local environment. Never commit actual secrets to GitHub.

**3. Start the services**

Once `docker-compose.yml` and service configurations are implemented:

```bash
docker compose up -d --build
```

**4. Verify service health**

```bash
docker compose ps
```

Check application logs:

```bash
docker compose logs -f
```

The final service URLs and configuration will be documented as the implementation is completed.

## API Design (Planned)

| Method | Endpoint | Description |
|---|---|---|
| GET | `/health` | Check backend health |
| POST | `/api/incidents` | Create an incident |
| GET | `/api/incidents` | List incidents |
| GET | `/api/incidents/:id` | Retrieve incident details |
| PATCH | `/api/incidents/:id/status` | Update incident status |
| GET | `/api/incidents/:id/events` | Retrieve incident activity |

Incoming webhook validation, authentication, and request validation will be added where required.

## Reliability and Security

- Validate incoming webhook payloads.
- Protect credentials using environment variables.
- Implement bounded retries and failure handling.
- Prevent duplicate incident creation where possible.
- Record status changes and important workflow events.
- Avoid logging secrets or unnecessary sensitive data.
- Require explicit approval for potentially disruptive remediation actions.

## Development Roadmap

- [ ] Define architecture and database schema.
- [ ] Build the Express backend and health endpoint.
- [ ] Configure PostgreSQL and Drizzle ORM.
- [ ] Containerize the sample application and supporting services.
- [ ] Configure Prometheus and alert rules.
- [ ] Build n8n incident-processing workflows.
- [ ] Integrate log collection and AI summarization.
- [ ] Implement incident severity and deduplication.
- [ ] Add email or Slack notifications.
- [ ] Test retries, failures, and incident resolution.
- [ ] Document deployment and create a demonstration.

## Future Enhancements

- Incident dashboard with service health and incident history.
- Alert correlation across multiple services.
- Mean Time to Detect (MTTD) and Mean Time to Resolve (MTTR) metrics.
- Role-based access control.
- Kubernetes monitoring and deployment.
- Controlled automated remediation with approval and audit logging.

## Project Goals

This project is intended to demonstrate practical skills in:

- Backend development and REST API design.
- Workflow orchestration with n8n.
- Monitoring and alerting.
- Docker-based service deployment.
- Relational database design.
- AI-assisted operational troubleshooting.
- Error handling, observability, and incident management.

## License

Choose an appropriate open-source license before publishing the repository.
