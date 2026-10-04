# SkillBridge

An academia–industry collaboration portal for student skill profiles, portfolio evidence, opportunity matching and faculty support.

Originally developed by **Varun, Pranay and Ananya** for **Code2Web BUILD_A_THON at SRM Ramapuram**, this repository continues from the shared team checkpoint as Varun's personal development version. The prototype was developed with AI assistance.

## Project status

Personal development starts from team checkpoint `f320478`. This is a student prototype under active development.

**Personal deployment:** not yet available. A separate deployment will be linked here when ready.

The existing shared team demo runs from a separate repository and deployment branch. Changes in this repository do not automatically update that demo.

## Features

| Workspace | Capabilities |
| --- | --- |
| Student | Skill assessments, skill passport, projects and certifications, opportunity applications, faculty feedback and mentorship tasks |
| Industry | Company profile, job and internship postings, required skills and weights, applicant skill matches and gaps, shortlisting |
| Faculty | Development opportunities, project reviews, evidence verification, student skill-gap inspection and mentorship allocation |
| Institution | Aggregate skill-gap and industry-demand reports, placement readiness summaries and evidence auditing |

Feature details are documented in [backend/README.md](backend/README.md) and [FACULTY_FEATURES.md](FACULTY_FEATURES.md).

## How matching and AI work

Skill and opportunity matching uses deterministic scoring based on skill requirements, weights and available evidence. These scores are descriptive matches, not predictions of hiring outcomes.

An optional **local Ollama** integration can explain results without changing numeric scores. It requires a separately running local model and is not required for the core application. AI-generated explanations are not verified as available in the shared hosted demo.

## Tech stack

| Layer | Technologies |
| --- | --- |
| Frontend | React, Vite, JavaScript, React Router and CSS |
| Backend | Python, Flask, SQLAlchemy, Flask-Migrate and JWT authentication |
| Database | SQLite for default local development; PostgreSQL through `DATABASE_URL` for hosted deployments |
| Testing | pytest, Vitest and React Testing Library |
| Optional AI | Ollama with a locally installed model |

The shared team deployment uses Render for the frontend and backend, and Neon PostgreSQL for its database.

## Run locally

Install Python 3, Node.js and npm before starting. Use a Python version compatible with the dependencies in `backend/requirements.txt`.

### 1. Clone the repository

```bash
git clone https://github.com/StackVarun/skillbridge.git
cd skillbridge
```

### 2. Set up the backend

```bash
cd backend
python3 -m venv .venv
source .venv/bin/activate
python -m pip install -r requirements.txt
cp .env.example .env
```

On Windows, activate the virtual environment with `.venv\Scripts\activate` instead.

Edit `backend/.env` and replace `SECRET_KEY` and `JWT_SECRET_KEY` with separate secret values. Leave `DATABASE_URL` unset to use the local `database/skillbridge.db`. Use the same database configuration for migrations, seeds and the running server.

```bash
python -m flask --app run.py db upgrade
python seed_reference_data.py
python run.py
```

The backend runs on `http://localhost:5001` by default. Its health endpoint is `http://localhost:5001/health`.

### 3. Start the frontend

Open a second terminal from the repository root:

```bash
cd frontend
npm install
npm run dev
```

Open the URL printed by Vite, normally `http://localhost:5173`. The development server proxies `/api` to the local backend. Keep `VITE_API_BASE_URL=/api` for local development; if you use another backend port, configure `API_PROXY_TARGET` accordingly.

### Optional demo data

From `backend/`, with the virtual environment active:

```bash
DEMO_PASSWORD='choose-a-local-demo-password' python seed_demo.py
```

The script seeds demo role accounts and synthetic portfolio/application data. Read its output for account details and posting ownership. Run it against a development database; it is not part of normal server startup. See [backend/README.md](backend/README.md) for institution account provisioning and optional AI configuration.

## Project structure

- `frontend/` — React pages, routing, API client and frontend tests
- `backend/` — Flask application, models, routes, services, migrations and tests
- `database/` — default local SQLite database
- `FACULTY_FEATURES.md` — faculty support setup and behavior

## Validation commands

Backend, with its virtual environment active:

```bash
cd backend
python -m pytest -q
```

Frontend:

```bash
cd frontend
npm test
npm run build
```

These are the project's validation commands; this README update does not claim a new successful test run.

## Development direction

- Improve the usability of each role's workspace.
- Refine application tracking and opportunity discovery.
- Improve validation, error handling and test coverage.
- Configure a separate deployment for this personal version.

These are planned improvements, not completed features.

## Prototype limitations

- Demo portfolios and applications are synthetic; self-reported evidence is not a verified credential.
- Self-registered industry and faculty accounts are not independently verified.
- Placement readiness and demand reports summarize available data; they are not predictive hiring models.
- The optional local AI runtime needs separate setup and hosting to work outside local development.

## Credits

The original team project was developed by **Varun, Pranay and Ananya**. This personal continuation preserves that team foundation and commit history.
