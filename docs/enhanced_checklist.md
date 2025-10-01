# Bankperks Implementation Checklist (TDD Methodology)

> **Purpose**: Comprehensive, granular checklist for AI coding agents to implement the Bankperks system following Test-Driven Development (TDD) principles.
>
> **Methodology**: 
> - **Setup/DevOps tickets**: Standard implementation order
> - **Feature tickets**: TDD cycle (Write Tests → Implement → Verify → Refactor)
> - **Test tickets**: Test creation and verification
>
> **Task Format**: Each task has unique ID `BP-XXX-YY` and takes 2-5 minutes to complete
>
> **Git Workflow**: After each ticket: add files → commit → push

---

## Table of Contents

### Sprint 0: Project Setup
- [BP-001: Initialize Project Structure](#bp-001-initialize-project-structure)
- [BP-002: Install Python Dependencies](#bp-002-install-python-dependencies)
- [BP-003: Setup PostgreSQL Database](#bp-003-setup-postgresql-database)
- [BP-004: Setup Redis](#bp-004-setup-redis)
- [BP-005: Create Configuration Files](#bp-005-create-configuration-files)
- [BP-006: Initialize Flask Application](#bp-006-initialize-flask-application)

### Sprint 1: Database Models and API Foundation
- [BP-007: Create Database Schema](#bp-007-create-database-schema)
- [BP-008: Implement SQLAlchemy Models](#bp-008-implement-sqlalchemy-models)
- [BP-009: Create Seed Data](#bp-009-create-seed-data)
- [BP-010: Implement Base API Structure](#bp-010-implement-base-api-structure)
- [BP-011: Implement Bonus API Endpoints](#bp-011-implement-bonus-api-endpoints)
- [BP-012: Implement Bank and Article API Endpoints](#bp-012-implement-bank-and-article-api-endpoints)

### Sprint 2: Web Scraping Infrastructure
- [BP-013: Setup Scrapy Project](#bp-013-setup-scrapy-project)
- [BP-014: Create Base Spider Class](#bp-014-create-base-spider-class)
- [BP-015: Implement Doctor of Credit Spider](#bp-015-implement-doctor-of-credit-spider)
- [BP-016: Implement Data Extraction Pipeline](#bp-016-implement-data-extraction-pipeline)
- [BP-017: Implement Data Cleaning Pipeline](#bp-017-implement-data-cleaning-pipeline)
- [BP-018: Implement Scrapy Pipeline](#bp-018-implement-scrapy-pipeline)

### Sprint 3: Celery Task Queue
- [BP-019: Setup Celery](#bp-019-setup-celery)
- [BP-020: Implement Scraping Tasks](#bp-020-implement-scraping-tasks)
- [BP-021: Implement Maintenance Tasks](#bp-021-implement-maintenance-tasks)

### Sprint 4: AI Content Generation
- [BP-022: Implement LLM Client](#bp-022-implement-llm-client)
- [BP-023: Create Prompt Templates](#bp-023-create-prompt-templates)
- [BP-024: Implement Content Generator](#bp-024-implement-content-generator)
- [BP-025: Implement AI Generation Tasks](#bp-025-implement-ai-generation-tasks)

### Sprint 5: Testing and QA
- [BP-026: Write Unit Tests for Models](#bp-026-write-unit-tests-for-models)
- [BP-027: Write Integration Tests for API](#bp-027-write-integration-tests-for-api)
- [BP-028: Write Tests for Scraping Pipeline](#bp-028-write-tests-for-scraping-pipeline)

### Sprint 6: Deployment and Operations
- [BP-029: Create Docker Configuration](#bp-029-create-docker-configuration)
- [BP-030: Configure Nginx Reverse Proxy](#bp-030-configure-nginx-reverse-proxy)
- [BP-031: Create Deployment Scripts](#bp-031-create-deployment-scripts)
- [BP-032: Setup Monitoring and Logging](#bp-032-setup-monitoring-and-logging)
- [BP-033: Implement Security Features](#bp-033-implement-security-features)
- [BP-034: Create Initial Seed Data and Documentation](#bp-034-create-initial-seed-data-and-documentation)

---

## Sprint 0: Project Setup

### BP-001: Initialize Project Structure

**Type**: Setup | **Priority**: Critical | **Story Points**: 2 | **TDD**: No

- [ ] BP-001-01: Create main app directory: `mkdir -p app`
- [ ] BP-001-02: Create API directories: `mkdir -p app/api/v1`
- [ ] BP-001-03: Create models directory: `mkdir -p app/models`
- [ ] BP-001-04: Create scraping directories: `mkdir -p app/scraping/scrapy_project/spiders`
- [ ] BP-001-05: Create scraping subdirectories: `mkdir -p app/scraping/extractors app/scraping/cleaners`
- [ ] BP-001-06: Create AI directories: `mkdir -p app/ai/prompts`
- [ ] BP-001-07: Create forum directory: `mkdir -p app/forum/avatars`
- [ ] BP-001-08: Create utility directories: `mkdir -p app/tasks app/services app/utils app/templates`
- [ ] BP-001-09: Create test directories: `mkdir -p tests/unit tests/integration tests/fixtures/sample_html`
- [ ] BP-001-10: Create database directory: `mkdir -p database/migrations/versions`
- [ ] BP-001-11: Create infrastructure directories: `mkdir -p docker scripts docs logs uploads`
- [ ] BP-001-12: Initialize git repository: `git init`
- [ ] BP-001-13: Create `.gitignore` file with Python, Flask, IDE entries
- [ ] BP-001-14: Create basic `README.md` with project overview
- [ ] BP-001-15: Create Python virtual environment: `python -m venv venv`
- [ ] BP-001-16: Activate virtual environment: `source venv/bin/activate` (macOS/Linux)
- [ ] BP-001-17: Verify directory structure: `tree -L 2` or `ls -la`
- [ ] BP-001-18: Git add all files: `git add .`
- [ ] BP-001-19: Git commit: `git commit -m "[BP-001] Initialize project structure"`
- [ ] BP-001-20: Git push to remote: `git push origin master`

---

### BP-002: Install Python Dependencies

**Type**: Setup | **Priority**: Critical | **Story Points**: 2 | **TDD**: No

- [ ] BP-002-01: Create `requirements.txt` file with production dependencies (Flask, SQLAlchemy, Scrapy, Celery, etc.)
- [ ] BP-002-02: Create `requirements-dev.txt` file with development dependencies (pytest, flake8, black, etc.)
- [ ] BP-002-03: Install production dependencies: `pip install -r requirements.txt`
- [ ] BP-002-04: Install development dependencies: `pip install -r requirements-dev.txt`
- [ ] BP-002-05: Verify Flask installed: `pip list | grep Flask`
- [ ] BP-002-06: Verify Scrapy installed: `pip list | grep Scrapy`
- [ ] BP-002-07: Verify Celery installed: `pip list | grep celery`
- [ ] BP-002-08: Download spaCy model: `python -m spacy download en_core_web_sm`
- [ ] BP-002-09: Verify spaCy model: `python -c "import spacy; nlp = spacy.load('en_core_web_sm'); print('spaCy OK')"`
- [ ] BP-002-10: Install Playwright browsers: `playwright install chromium`
- [ ] BP-002-11: Verify Playwright: `playwright --version`
- [ ] BP-002-12: Freeze dependencies: `pip freeze > requirements-frozen.txt`
- [ ] BP-002-13: Git add requirements files: `git add requirements*.txt`
- [ ] BP-002-14: Git commit: `git commit -m "[BP-002] Install Python dependencies"`
- [ ] BP-002-15: Git push to remote: `git push origin master`

---

### BP-003: Setup PostgreSQL Database

**Type**: Setup | **Priority**: Critical | **Story Points**: 2 | **TDD**: No

- [ ] BP-003-01: Start PostgreSQL service: `brew services start postgresql` (macOS) or equivalent
- [ ] BP-003-02: Verify PostgreSQL running: `pg_isready`
- [ ] BP-003-03: Create database: `createdb bankperks`
- [ ] BP-003-04: Create database user: `createuser -P bankperks` (enter password when prompted)
- [ ] BP-003-05: Grant privileges: `psql -c "GRANT ALL PRIVILEGES ON DATABASE bankperks TO bankperks;"`
- [ ] BP-003-06: Enable UUID extension: `psql -U bankperks -d bankperks -c "CREATE EXTENSION IF NOT EXISTS \"uuid-ossp\";"`
- [ ] BP-003-07: Verify database connection: `psql -U bankperks -d bankperks -c "SELECT version();"`
- [ ] BP-003-08: List databases: `psql -l | grep bankperks`
- [ ] BP-003-09: Test table creation: `psql -U bankperks -d bankperks -c "CREATE TABLE test (id SERIAL PRIMARY KEY);"`
- [ ] BP-003-10: Drop test table: `psql -U bankperks -d bankperks -c "DROP TABLE test;"`
- [ ] BP-003-11: Document database credentials in `.env.example`
- [ ] BP-003-12: Git add `.env.example`: `git add .env.example`
- [ ] BP-003-13: Git commit: `git commit -m "[BP-003] Setup PostgreSQL database"`
- [ ] BP-003-14: Git push to remote: `git push origin master`

---

### BP-004: Setup Redis

**Type**: Setup | **Priority**: High | **Story Points**: 1 | **TDD**: No

- [ ] BP-004-01: Install Redis: `brew install redis` (macOS) or equivalent
- [ ] BP-004-02: Start Redis service: `brew services start redis` (macOS) or equivalent
- [ ] BP-004-03: Verify Redis running: `redis-cli ping` (should return PONG)
- [ ] BP-004-04: Test Redis set: `redis-cli SET test "hello"`
- [ ] BP-004-05: Test Redis get: `redis-cli GET test` (should return "hello")
- [ ] BP-004-06: Delete test key: `redis-cli DEL test`
- [ ] BP-004-07: Check Redis info: `redis-cli INFO server`
- [ ] BP-004-08: Verify Redis port: `redis-cli -p 6379 ping`
- [ ] BP-004-09: Document Redis URL in `.env.example`: `REDIS_URL=redis://localhost:6379/0`
- [ ] BP-004-10: Git add `.env.example`: `git add .env.example`
- [ ] BP-004-11: Git commit: `git commit -m "[BP-004] Setup Redis"`
- [ ] BP-004-12: Git push to remote: `git push origin master`

---

### BP-005: Create Configuration Files

**Type**: Setup | **Priority**: High | **Story Points**: 2 | **TDD**: No

- [ ] BP-005-01: Create `app/config.py` with Config classes (Development, Production, Testing)
- [ ] BP-005-02: Add SECRET_KEY configuration to config.py
- [ ] BP-005-03: Add DATABASE_URL configuration to config.py
- [ ] BP-005-04: Add REDIS_URL configuration to config.py
- [ ] BP-005-05: Add CELERY_BROKER_URL configuration to config.py
- [ ] BP-005-06: Add API key configurations (OPENAI_API_KEY, ANTHROPIC_API_KEY) to config.py
- [ ] BP-005-07: Create `.env.example` file with all environment variables
- [ ] BP-005-08: Copy `.env.example` to `.env`: `cp .env.example .env`
- [ ] BP-005-09: Generate SECRET_KEY: `python -c "import secrets; print(secrets.token_hex(32))"`
- [ ] BP-005-10: Update `.env` with generated SECRET_KEY
- [ ] BP-005-11: Update `.env` with DATABASE_URL: `postgresql://bankperks:password@localhost:5432/bankperks`
- [ ] BP-005-12: Update `.env` with REDIS_URL: `redis://localhost:6379/0`
- [ ] BP-005-13: Create `pytest.ini` with pytest configuration
- [ ] BP-005-14: Create `.flake8` with linting configuration
- [ ] BP-005-15: Verify config loads: `python -c "from app.config import config; print(config['development'])"`
- [ ] BP-005-16: Add `.env` to `.gitignore`
- [ ] BP-005-17: Git add config files: `git add app/config.py .env.example pytest.ini .flake8 .gitignore`
- [ ] BP-005-18: Git commit: `git commit -m "[BP-005] Create configuration files"`
- [ ] BP-005-19: Git push to remote: `git push origin master`

---

### BP-006: Initialize Flask Application

**Type**: Setup | **Priority**: Critical | **Story Points**: 3 | **TDD**: No

- [ ] BP-006-01: Create `app/extensions.py` with Flask extension instances (db, migrate, cors, limiter)
- [ ] BP-006-02: Create `app/__init__.py` with create_app() factory function
- [ ] BP-006-03: Add database initialization to create_app()
- [ ] BP-006-04: Add CORS configuration to create_app()
- [ ] BP-006-05: Add rate limiting configuration to create_app()
- [ ] BP-006-06: Add error handlers to create_app()
- [ ] BP-006-07: Create `app/api/__init__.py` (empty for now)
- [ ] BP-006-08: Create `app/api/errors.py` with error handler functions
- [ ] BP-006-09: Create `run.py` with application entry point
- [ ] BP-006-10: Add Flask CLI commands to run.py (seed-db, etc.)
- [ ] BP-006-11: Set FLASK_APP environment variable: `export FLASK_APP=run.py`
- [ ] BP-006-12: Set FLASK_ENV environment variable: `export FLASK_ENV=development`
- [ ] BP-006-13: Test Flask app starts: `flask run` (in separate terminal)
- [ ] BP-006-14: Test Flask app responds: `curl http://localhost:5000/` (should return 404, expected)
- [ ] BP-006-15: Stop Flask app: Ctrl+C
- [ ] BP-006-16: Test Flask shell: `flask shell`
- [ ] BP-006-17: In Flask shell, test extensions: `from app.extensions import db; print(db)`
- [ ] BP-006-18: Exit Flask shell: `exit()`
- [ ] BP-006-19: Git add Flask files: `git add app/__init__.py app/extensions.py app/api/ run.py`
- [ ] BP-006-20: Git commit: `git commit -m "[BP-006] Initialize Flask application"`
- [ ] BP-006-21: Git push to remote: `git push origin master`

---

## Sprint 1: Database Models and API Foundation

### BP-007: Create Database Schema

**Type**: Feature | **Priority**: Critical | **Story Points**: 5 | **TDD**: No (Schema First)

- [ ] BP-007-01: Create `database/init_db.sql` file
- [ ] BP-007-02: Add UUID extension to init_db.sql
- [ ] BP-007-03: Add banks table schema to init_db.sql
- [ ] BP-007-04: Add account_types table schema to init_db.sql
- [ ] BP-007-05: Add bonuses table schema to init_db.sql with all fields and constraints
- [ ] BP-007-06: Add requirement_types table schema to init_db.sql
- [ ] BP-007-07: Add scraping_sources table schema to init_db.sql
- [ ] BP-007-08: Add articles table schema to init_db.sql
- [ ] BP-007-09: Add article_versions table schema to init_db.sql
- [ ] BP-007-10: Add newsletters table schema to init_db.sql
- [ ] BP-007-11: Add users table schema to init_db.sql
- [ ] BP-007-12: Add user_saved_bonuses table schema to init_db.sql
- [ ] BP-007-13: Add celery_tasks table schema to init_db.sql
- [ ] BP-007-14: Add all indexes to init_db.sql
- [ ] BP-007-15: Add GIN indexes for JSONB columns to init_db.sql
- [ ] BP-007-16: Add update_updated_at_column() trigger function to init_db.sql
- [ ] BP-007-17: Add triggers for all tables to init_db.sql
- [ ] BP-007-18: Add active_bonuses view to init_db.sql
- [ ] BP-007-19: Add published_articles view to init_db.sql
- [ ] BP-007-20: Run schema: `psql -U bankperks -d bankperks < database/init_db.sql`
- [ ] BP-007-21: Verify tables created: `psql -U bankperks -d bankperks -c "\dt"`
- [ ] BP-007-22: Verify indexes created: `psql -U bankperks -d bankperks -c "\di"`
- [ ] BP-007-23: Test trigger: Insert and update a bank record
- [ ] BP-007-24: Verify trigger worked: Check updated_at changed
- [ ] BP-007-25: Test views: `psql -U bankperks -d bankperks -c "SELECT * FROM active_bonuses LIMIT 1;"`
- [ ] BP-007-26: Git add schema file: `git add database/init_db.sql`
- [ ] BP-007-27: Git commit: `git commit -m "[BP-007] Create database schema"`
- [ ] BP-007-28: Git push to remote: `git push origin master`

---

### BP-008: Implement SQLAlchemy Models

**Type**: Feature | **Priority**: Critical | **Story Points**: 5 | **TDD**: Yes

#### Phase 1: Write Tests (TDD Red Phase)

- [ ] BP-008-01: Create `tests/unit/test_bank_model.py` file
- [ ] BP-008-02: Write test for Bank model instantiation in test_bank_model.py
- [ ] BP-008-03: Write test for Bank.to_dict() method in test_bank_model.py
- [ ] BP-008-04: Write test for Bank-Bonus relationship in test_bank_model.py
- [ ] BP-008-05: Create `tests/unit/test_bonus_model.py` file
- [ ] BP-008-06: Write test for Bonus model instantiation in test_bonus_model.py
- [ ] BP-008-07: Write test for Bonus.to_dict() method in test_bonus_model.py
- [ ] BP-008-08: Write test for Bonus.is_active property in test_bonus_model.py
- [ ] BP-008-09: Write test for Bonus.days_until_expiration property in test_bonus_model.py
- [ ] BP-008-10: Create `tests/unit/test_user_model.py` file
- [ ] BP-008-11: Write test for User.set_password() method in test_user_model.py
- [ ] BP-008-12: Write test for User.check_password() method in test_user_model.py
- [ ] BP-008-13: Run tests (should fail): `pytest tests/unit/test_bank_model.py tests/unit/test_bonus_model.py tests/unit/test_user_model.py`

#### Phase 2: Implement Models (TDD Green Phase)

- [ ] BP-008-14: Create `app/models/__init__.py` with TimestampMixin and UUIDMixin
- [ ] BP-008-15: Create `app/models/bank.py` with Bank model class
- [ ] BP-008-16: Implement Bank.__repr__() method
- [ ] BP-008-17: Implement Bank.to_dict() method
- [ ] BP-008-18: Create `app/models/account_type.py` with AccountType model
- [ ] BP-008-19: Implement AccountType.to_dict() method
- [ ] BP-008-20: Create `app/models/bonus.py` with Bonus model class
- [ ] BP-008-21: Implement Bonus.to_dict() method with include_bank and include_account_type parameters
- [ ] BP-008-22: Implement Bonus.is_active property
- [ ] BP-008-23: Implement Bonus.days_until_expiration property
- [ ] BP-008-24: Define Bank-Bonus relationship in both models
- [ ] BP-008-25: Create `app/models/article.py` with Article model
- [ ] BP-008-26: Implement Article.to_dict() method with include_content parameter
- [ ] BP-008-27: Create `app/models/newsletter.py` with Newsletter model (complete code from improved tickets)
- [ ] BP-008-28: Implement Newsletter.to_dict() method
- [ ] BP-008-29: Create `app/models/user.py` with User model (complete code from improved tickets)
- [ ] BP-008-30: Implement User.set_password() method using werkzeug
- [ ] BP-008-31: Implement User.check_password() method
- [ ] BP-008-32: Implement User.to_dict() method with include_email parameter
- [ ] BP-008-33: Create `app/models/scraping_source.py` with ScrapingSource model (complete code from improved tickets)
- [ ] BP-008-34: Implement ScrapingSource.to_dict() method
- [ ] BP-008-35: Implement ScrapingSource.success_rate property
- [ ] BP-008-36: Create `app/models/requirement_type.py` with RequirementType model
- [ ] BP-008-37: Implement RequirementType.to_dict() method

#### Phase 3: Verify Tests Pass (TDD Green Phase)

- [ ] BP-008-38: Run Bank model tests: `pytest tests/unit/test_bank_model.py -v`
- [ ] BP-008-39: Run Bonus model tests: `pytest tests/unit/test_bonus_model.py -v`
- [ ] BP-008-40: Run User model tests: `pytest tests/unit/test_user_model.py -v`
- [ ] BP-008-41: Run all model tests: `pytest tests/unit/ -v`
- [ ] BP-008-42: Check test coverage: `pytest tests/unit/ --cov=app/models --cov-report=term`
- [ ] BP-008-43: Verify coverage >90%

#### Phase 4: Database Integration

- [ ] BP-008-44: Initialize Flask-Migrate: `flask db init`
- [ ] BP-008-45: Create migration: `flask db migrate -m "Initial schema"`
- [ ] BP-008-46: Apply migration: `flask db upgrade`
- [ ] BP-008-47: Verify migration applied: `psql -U bankperks -d bankperks -c "\dt"`

#### Phase 5: Integration Testing

- [ ] BP-008-48: Start Flask shell: `flask shell`
- [ ] BP-008-49: Test Bank model: Create and save Bank instance
- [ ] BP-008-50: Test Bank.to_dict(): Print bank.to_dict()
- [ ] BP-008-51: Test relationship: Query bank.bonuses.count()
- [ ] BP-008-52: Test User model: Create user and set password
- [ ] BP-008-53: Test User.check_password(): Verify password check works
- [ ] BP-008-54: Test ScrapingSource model: Create source and check success_rate
- [ ] BP-008-55: Exit Flask shell: `exit()`

#### Phase 6: Git Operations

- [ ] BP-008-56: Git add model files: `git add app/models/`
- [ ] BP-008-57: Git add test files: `git add tests/unit/test_*_model.py`
- [ ] BP-008-58: Git add migrations: `git add migrations/`
- [ ] BP-008-59: Git commit: `git commit -m "[BP-008] Implement SQLAlchemy models with TDD"`
- [ ] BP-008-60: Git push to remote: `git push origin master`

---

### BP-009: Create Seed Data

**Type**: Feature | **Priority**: High | **Story Points**: 2 | **TDD**: Yes

#### Phase 1: Write Tests (TDD Red Phase)

- [ ] BP-009-01: Create `tests/unit/test_seed_data.py` file
- [ ] BP-009-02: Write test for seed_account_types() function
- [ ] BP-009-03: Write test for seed_requirement_types() function
- [ ] BP-009-04: Write test for idempotency (running seed twice doesn't create duplicates)
- [ ] BP-009-05: Run tests (should fail): `pytest tests/unit/test_seed_data.py`

#### Phase 2: Implement Seed Functions (TDD Green Phase)

- [ ] BP-009-06: Create `database/seed_data.py` file
- [ ] BP-009-07: Import necessary models (AccountType, RequirementType)
- [ ] BP-009-08: Implement seed_account_types() function with 6 account types
- [ ] BP-009-09: Add duplicate check to seed_account_types()
- [ ] BP-009-10: Implement seed_requirement_types() function with all 10 types (complete code from improved tickets)
- [ ] BP-009-11: Add duplicate check to seed_requirement_types()
- [ ] BP-009-12: Add print statements for confirmation

#### Phase 3: Verify Tests Pass (TDD Green Phase)

- [ ] BP-009-13: Run seed data tests: `pytest tests/unit/test_seed_data.py -v`
- [ ] BP-009-14: Verify all tests pass

#### Phase 4: CLI Integration

- [ ] BP-009-15: Add seed-db CLI command to run.py
- [ ] BP-009-16: Test CLI command: `flask seed-db`
- [ ] BP-009-17: Verify account types: `psql -U bankperks -d bankperks -c "SELECT COUNT(*) FROM account_types;"`
- [ ] BP-009-18: Verify count is 6
- [ ] BP-009-19: Verify requirement types: `psql -U bankperks -d bankperks -c "SELECT COUNT(*) FROM requirement_types;"`
- [ ] BP-009-20: Verify count is 10
- [ ] BP-009-21: Display account types: `psql -U bankperks -d bankperks -c "SELECT name, slug FROM account_types ORDER BY name;"`
- [ ] BP-009-22: Display requirement types: `psql -U bankperks -d bankperks -c "SELECT name, slug FROM requirement_types ORDER BY name;"`

#### Phase 5: Test Idempotency

- [ ] BP-009-23: Run seed command again: `flask seed-db`
- [ ] BP-009-24: Verify account types count still 6: `psql -U bankperks -d bankperks -c "SELECT COUNT(*) FROM account_types;"`
- [ ] BP-009-25: Verify requirement types count still 10: `psql -U bankperks -d bankperks -c "SELECT COUNT(*) FROM requirement_types;"`

#### Phase 6: Git Operations

- [ ] BP-009-26: Git add seed files: `git add database/seed_data.py`
- [ ] BP-009-27: Git add test files: `git add tests/unit/test_seed_data.py`
- [ ] BP-009-28: Git add run.py changes: `git add run.py`
- [ ] BP-009-29: Git commit: `git commit -m "[BP-009] Create seed data with TDD"`
- [ ] BP-009-30: Git push to remote: `git push origin master`

---

### BP-010: Implement Base API Structure

**Type**: Feature | **Priority**: Critical | **Story Points**: 3 | **TDD**: Yes

#### Phase 1: Write Tests (TDD Red Phase)

- [ ] BP-010-01: Create `tests/integration/test_api_base.py` file
- [ ] BP-010-02: Write test for health check endpoint
- [ ] BP-010-03: Write test for 404 error handler
- [ ] BP-010-04: Write test for 500 error handler
- [ ] BP-010-05: Run tests (should fail): `pytest tests/integration/test_api_base.py`

#### Phase 2: Implement Base API (TDD Green Phase)

- [ ] BP-010-06: Create `app/api/v1/__init__.py` with api_v1_bp blueprint
- [ ] BP-010-07: Create `app/api/v1/health.py` with health check endpoint
- [ ] BP-010-08: Create `app/api/errors.py` with error handler functions
- [ ] BP-010-09: Implement handle_404() error handler
- [ ] BP-010-10: Implement handle_500() error handler
- [ ] BP-010-11: Register api_v1_bp blueprint in app/__init__.py
- [ ] BP-010-12: Register error handlers in app/__init__.py

#### Phase 3: Verify Tests Pass (TDD Green Phase)

- [ ] BP-010-13: Run API base tests: `pytest tests/integration/test_api_base.py -v`
- [ ] BP-010-14: Verify all tests pass

#### Phase 4: Manual Testing

- [ ] BP-010-15: Start Flask app: `flask run`
- [ ] BP-010-16: Test health endpoint: `curl http://localhost:5000/api/v1/health`
- [ ] BP-010-17: Verify response: `{"status":"healthy","timestamp":"...","version":"1.0.0"}`
- [ ] BP-010-18: Test 404 error: `curl http://localhost:5000/api/v1/nonexistent`
- [ ] BP-010-19: Verify 404 response format
- [ ] BP-010-20: Stop Flask app: Ctrl+C

#### Phase 5: Git Operations

- [ ] BP-010-21: Git add API files: `git add app/api/`
- [ ] BP-010-22: Git add test files: `git add tests/integration/test_api_base.py`
- [ ] BP-010-23: Git commit: `git commit -m "[BP-010] Implement base API structure with TDD"`
- [ ] BP-010-24: Git push to remote: `git push origin master`

---

### BP-011: Implement Bonus API Endpoints

**Type**: Feature | **Priority**: Critical | **Story Points**: 5 | **TDD**: Yes

#### Phase 1: Write Tests (TDD Red Phase)

- [ ] BP-011-01: Create `tests/integration/test_api_bonuses.py` file
- [ ] BP-011-02: Write test for GET /bonuses (list with pagination)
- [ ] BP-011-03: Write test for GET /bonuses with status filter
- [ ] BP-011-04: Write test for GET /bonuses with bank_id filter
- [ ] BP-011-05: Write test for GET /bonuses with min_amount filter
- [ ] BP-011-06: Write test for GET /bonuses with max_amount filter
- [ ] BP-011-07: Write test for GET /bonuses with sorting (-amount)
- [ ] BP-011-08: Write test for GET /bonuses/<id> (get single bonus)
- [ ] BP-011-09: Write test for GET /bonuses/<uuid> (get by UUID)
- [ ] BP-011-10: Write test for PATCH /bonuses/<id> (update bonus)
- [ ] BP-011-11: Write test for 404 error (non-existent bonus)
- [ ] BP-011-12: Run tests (should fail): `pytest tests/integration/test_api_bonuses.py`

#### Phase 2: Implement Pagination Utility (TDD Green Phase)

- [ ] BP-011-13: Create `app/utils/pagination.py` file
- [ ] BP-011-14: Implement paginate() function with query, page, per_page parameters
- [ ] BP-011-15: Add pagination metadata (total, page, per_page, total_pages)
- [ ] BP-011-16: Add to_dict() serialization for items

#### Phase 3: Implement Bonus API (TDD Green Phase)

- [ ] BP-011-17: Create `app/api/v1/bonuses.py` file
- [ ] BP-011-18: Import necessary modules (Flask, Bonus model, pagination, SQLAlchemy)
- [ ] BP-011-19: Implement list_bonuses() endpoint with base query
- [ ] BP-011-20: Add status filter to list_bonuses()
- [ ] BP-011-21: Add bank_id filter to list_bonuses()
- [ ] BP-011-22: Add account_type_id filter to list_bonuses()
- [ ] BP-011-23: Add min_amount filter to list_bonuses()
- [ ] BP-011-24: Add max_amount filter to list_bonuses()
- [ ] BP-011-25: Add sorting logic with +/- prefix support to list_bonuses()
- [ ] BP-011-26: Add error handling to list_bonuses()
- [ ] BP-011-27: Implement get_bonus(id) endpoint
- [ ] BP-011-28: Implement get_bonus_by_uuid(uuid) endpoint
- [ ] BP-011-29: Implement update_bonus(id) endpoint with PATCH
- [ ] BP-011-30: Add logging to all endpoints
- [ ] BP-011-31: Register bonuses blueprint in app/api/v1/__init__.py

#### Phase 4: Verify Tests Pass (TDD Green Phase)

- [ ] BP-011-32: Run bonus API tests: `pytest tests/integration/test_api_bonuses.py -v`
- [ ] BP-011-33: Verify all tests pass
- [ ] BP-011-34: Check test coverage: `pytest tests/integration/test_api_bonuses.py --cov=app/api/v1/bonuses`

#### Phase 5: Manual Testing

- [ ] BP-011-35: Start Flask app: `flask run`
- [ ] BP-011-36: Test basic list: `curl http://localhost:5000/api/v1/bonuses`
- [ ] BP-011-37: Test pagination: `curl http://localhost:5000/api/v1/bonuses?page=1&per_page=10`
- [ ] BP-011-38: Test status filter: `curl http://localhost:5000/api/v1/bonuses?status=active`
- [ ] BP-011-39: Test bank_id filter: `curl http://localhost:5000/api/v1/bonuses?bank_id=1`
- [ ] BP-011-40: Test amount range: `curl "http://localhost:5000/api/v1/bonuses?min_amount=200&max_amount=500"`
- [ ] BP-011-41: Test sorting descending: `curl http://localhost:5000/api/v1/bonuses?sort=-amount`
- [ ] BP-011-42: Test sorting ascending: `curl http://localhost:5000/api/v1/bonuses?sort=+amount`
- [ ] BP-011-43: Test combined filters: `curl "http://localhost:5000/api/v1/bonuses?status=active&min_amount=300&sort=-amount"`
- [ ] BP-011-44: Test get by ID: `curl http://localhost:5000/api/v1/bonuses/1`
- [ ] BP-011-45: Test 404 error: `curl http://localhost:5000/api/v1/bonuses/99999`
- [ ] BP-011-46: Stop Flask app: Ctrl+C

#### Phase 6: Git Operations

- [ ] BP-011-47: Git add API files: `git add app/api/v1/bonuses.py app/utils/pagination.py`
- [ ] BP-011-48: Git add test files: `git add tests/integration/test_api_bonuses.py`
- [ ] BP-011-49: Git commit: `git commit -m "[BP-011] Implement Bonus API endpoints with TDD"`
- [ ] BP-011-50: Git push to remote: `git push origin master`

---

### BP-012: Implement Bank and Article API Endpoints

**Type**: Feature | **Priority**: High | **Story Points**: 3 | **TDD**: Yes

#### Phase 1: Write Tests (TDD Red Phase)

- [ ] BP-012-01: Create `tests/integration/test_api_banks.py` file
- [ ] BP-012-02: Write test for GET /banks (list banks)
- [ ] BP-012-03: Write test for GET /banks/<id> (get single bank)
- [ ] BP-012-04: Write test for GET /banks/<id>/bonuses (get bank's bonuses)
- [ ] BP-012-05: Create `tests/integration/test_api_articles.py` file
- [ ] BP-012-06: Write test for GET /articles (list articles)
- [ ] BP-012-07: Write test for GET /articles with status filter
- [ ] BP-012-08: Write test for GET /articles with article_type filter
- [ ] BP-012-09: Write test for GET /articles/<slug> (get by slug)
- [ ] BP-012-10: Write test for GET /articles/<id> (get by ID)
- [ ] BP-012-11: Write test for view count increment
- [ ] BP-012-12: Run tests (should fail): `pytest tests/integration/test_api_banks.py tests/integration/test_api_articles.py`

#### Phase 2: Implement Banks API (TDD Green Phase)

- [ ] BP-012-13: Create `app/api/v1/banks.py` file
- [ ] BP-012-14: Import necessary modules
- [ ] BP-012-15: Implement list_banks() endpoint with pagination
- [ ] BP-012-16: Add ordering by name to list_banks()
- [ ] BP-012-17: Implement get_bank(id) endpoint
- [ ] BP-012-18: Implement get_bank_bonuses(id) endpoint
- [ ] BP-012-19: Add status filter to get_bank_bonuses()
- [ ] BP-012-20: Add error handling and logging to all endpoints
- [ ] BP-012-21: Register banks blueprint in app/api/v1/__init__.py

#### Phase 3: Implement Articles API (TDD Green Phase)

- [ ] BP-012-22: Create `app/api/v1/articles.py` file
- [ ] BP-012-23: Import necessary modules
- [ ] BP-012-24: Implement list_articles() endpoint with pagination
- [ ] BP-012-25: Add status filter to list_articles() (default to published)
- [ ] BP-012-26: Add article_type filter to list_articles()
- [ ] BP-012-27: Add sorting logic to list_articles()
- [ ] BP-012-28: Implement get_article_by_slug(slug) endpoint
- [ ] BP-012-29: Add view count increment to get_article_by_slug()
- [ ] BP-012-30: Add related bonuses inclusion to get_article_by_slug()
- [ ] BP-012-31: Implement get_article_by_id(id) endpoint
- [ ] BP-012-32: Add view count increment to get_article_by_id()
- [ ] BP-012-33: Add error handling and logging to all endpoints
- [ ] BP-012-34: Register articles blueprint in app/api/v1/__init__.py

#### Phase 4: Verify Tests Pass (TDD Green Phase)

- [ ] BP-012-35: Run banks API tests: `pytest tests/integration/test_api_banks.py -v`
- [ ] BP-012-36: Run articles API tests: `pytest tests/integration/test_api_articles.py -v`
- [ ] BP-012-37: Verify all tests pass

#### Phase 5: Manual Testing - Banks

- [ ] BP-012-38: Start Flask app: `flask run`
- [ ] BP-012-39: Test list banks: `curl http://localhost:5000/api/v1/banks`
- [ ] BP-012-40: Test get bank: `curl http://localhost:5000/api/v1/banks/1`
- [ ] BP-012-41: Test bank's bonuses: `curl http://localhost:5000/api/v1/banks/1/bonuses`
- [ ] BP-012-42: Test bank's active bonuses: `curl http://localhost:5000/api/v1/banks/1/bonuses?status=active`

#### Phase 6: Manual Testing - Articles

- [ ] BP-012-43: Test list articles: `curl http://localhost:5000/api/v1/articles`
- [ ] BP-012-44: Test filter by type: `curl http://localhost:5000/api/v1/articles?article_type=comparison`
- [ ] BP-012-45: Test get by slug: `curl http://localhost:5000/api/v1/articles/chase-300-bonus-review`
- [ ] BP-012-46: Test get by ID: `curl http://localhost:5000/api/v1/articles/1`
- [ ] BP-012-47: Test 404 error: `curl http://localhost:5000/api/v1/articles/non-existent-slug`
- [ ] BP-012-48: Stop Flask app: Ctrl+C

#### Phase 7: Git Operations

- [ ] BP-012-49: Git add API files: `git add app/api/v1/banks.py app/api/v1/articles.py`
- [ ] BP-012-50: Git add test files: `git add tests/integration/test_api_banks.py tests/integration/test_api_articles.py`
- [ ] BP-012-51: Git commit: `git commit -m "[BP-012] Implement Bank and Article API endpoints with TDD"`
- [ ] BP-012-52: Git push to remote: `git push origin master`

---

## Sprint 2: Web Scraping Infrastructure

### BP-013: Setup Scrapy Project

**Type**: Feature | **Priority**: High | **Story Points**: 2 | **TDD**: No (Infrastructure)

- [ ] BP-013-01: Navigate to scraping directory: `cd app/scraping`
- [ ] BP-013-02: Initialize Scrapy project: `scrapy startproject scrapy_project`
- [ ] BP-013-03: Navigate back to root: `cd ../..`
- [ ] BP-013-04: Open `app/scraping/scrapy_project/settings.py`
- [ ] BP-013-05: Set BOT_NAME to 'bankperks'
- [ ] BP-013-06: Set SPIDER_MODULES to ['app.scraping.scrapy_project.spiders']
- [ ] BP-013-07: Set NEWSPIDER_MODULE to 'app.scraping.scrapy_project.spiders'
- [ ] BP-013-08: Set USER_AGENT to 'BankperksBot/1.0 (+https://bankperks.net/botinfo)'
- [ ] BP-013-09: Set ROBOTSTXT_OBEY to True
- [ ] BP-013-10: Set CONCURRENT_REQUESTS to 8
- [ ] BP-013-11: Set DOWNLOAD_DELAY to 2.0
- [ ] BP-013-12: Set CONCURRENT_REQUESTS_PER_DOMAIN to 2
- [ ] BP-013-13: Add Playwright download handlers configuration
- [ ] BP-013-14: Set TWISTED_REACTOR to asyncio
- [ ] BP-013-15: Test Scrapy setup: `cd app/scraping/scrapy_project && scrapy list`
- [ ] BP-013-16: Verify empty list (no spiders yet)
- [ ] BP-013-17: Navigate back to root: `cd ../../..`
- [ ] BP-013-18: Git add Scrapy files: `git add app/scraping/scrapy_project/`
- [ ] BP-013-19: Git commit: `git commit -m "[BP-013] Setup Scrapy project"`
- [ ] BP-013-20: Git push to remote: `git push origin master`

---

### BP-014: Create Base Spider Class

**Type**: Feature | **Priority**: High | **Story Points**: 3 | **TDD**: Yes

#### Phase 1: Write Tests (TDD Red Phase)

- [ ] BP-014-01: Create `tests/unit/test_base_spider.py` file
- [ ] BP-014-02: Write test for BaseSpider initialization
- [ ] BP-014-03: Write test for parse_amount() method with various formats
- [ ] BP-014-04: Write test for parse_date() method with various formats
- [ ] BP-014-05: Write test for error handling in parse methods
- [ ] BP-014-06: Run tests (should fail): `pytest tests/unit/test_base_spider.py`

#### Phase 2: Implement Base Spider (TDD Green Phase)

- [ ] BP-014-07: Create `app/scraping/scrapy_project/spiders/__init__.py` (empty)
- [ ] BP-014-08: Create `app/scraping/scrapy_project/spiders/base_spider.py` file
- [ ] BP-014-09: Import scrapy and logging modules
- [ ] BP-014-10: Define BaseSpider class extending scrapy.Spider
- [ ] BP-014-11: Add custom_settings with retry configuration
- [ ] BP-014-12: Implement parse_amount() method with regex for $X,XXX.XX format
- [ ] BP-014-13: Add error handling to parse_amount() for invalid inputs
- [ ] BP-014-14: Implement parse_date() method using dateutil.parser
- [ ] BP-014-15: Add error handling to parse_date() for invalid inputs
- [ ] BP-014-16: Add logging statements to all methods
- [ ] BP-014-17: Add docstrings to all methods

#### Phase 3: Verify Tests Pass (TDD Green Phase)

- [ ] BP-014-18: Run base spider tests: `pytest tests/unit/test_base_spider.py -v`
- [ ] BP-014-19: Verify all tests pass
- [ ] BP-014-20: Check test coverage: `pytest tests/unit/test_base_spider.py --cov=app/scraping/scrapy_project/spiders/base_spider`

#### Phase 4: Manual Testing

- [ ] BP-014-21: Test import: `python -c "from app.scraping.scrapy_project.spiders.base_spider import BaseSpider; print('OK')"`
- [ ] BP-014-22: Test parse_amount: `python -c "from app.scraping.scrapy_project.spiders.base_spider import BaseSpider; bs = BaseSpider(); print(bs.parse_amount('$1,234.56'))"`
- [ ] BP-014-23: Verify output: 1234.56
- [ ] BP-014-24: Test parse_date: `python -c "from app.scraping.scrapy_project.spiders.base_spider import BaseSpider; bs = BaseSpider(); print(bs.parse_date('December 31, 2024'))"`

#### Phase 5: Git Operations

- [ ] BP-014-25: Git add spider files: `git add app/scraping/scrapy_project/spiders/`
- [ ] BP-014-26: Git add test files: `git add tests/unit/test_base_spider.py`
- [ ] BP-014-27: Git commit: `git commit -m "[BP-014] Create Base Spider class with TDD"`
- [ ] BP-014-28: Git push to remote: `git push origin master`

---

### BP-015: Implement Doctor of Credit Spider

**Type**: Feature | **Priority**: High | **Story Points**: 5 | **TDD**: Yes

#### Phase 1: Write Tests (TDD Red Phase)

- [ ] BP-015-01: Create `tests/unit/test_doctorofcredit_spider.py` file
- [ ] BP-015-02: Create sample HTML file: `tests/fixtures/sample_html/doctorofcredit.html`
- [ ] BP-015-03: Write test for spider initialization
- [ ] BP-015-04: Write test for parse() method with sample HTML
- [ ] BP-015-05: Write test for bonus data extraction
- [ ] BP-015-06: Write test for error handling with missing elements
- [ ] BP-015-07: Write test for pagination handling
- [ ] BP-015-08: Run tests (should fail): `pytest tests/unit/test_doctorofcredit_spider.py`

#### Phase 2: Implement Spider (TDD Green Phase)

- [ ] BP-015-09: Create `app/scraping/scrapy_project/spiders/doctorofcredit_spider.py` file
- [ ] BP-015-10: Import BaseSpider, scrapy, and logging
- [ ] BP-015-11: Define DoctorOfCreditSpider class extending BaseSpider
- [ ] BP-015-12: Set name = 'doctorofcredit'
- [ ] BP-015-13: Set allowed_domains = ['doctorofcredit.com']
- [ ] BP-015-14: Set start_urls with Doctor of Credit bonus page
- [ ] BP-015-15: Implement parse() method with multiple selector fallbacks
- [ ] BP-015-16: Add try-except blocks for each field extraction
- [ ] BP-015-17: Add logging for found bonuses and errors
- [ ] BP-015-18: Implement pagination with multiple selector fallbacks
- [ ] BP-015-19: Add data validation before yielding
- [ ] BP-015-20: Add docstrings and comments about selector verification

#### Phase 3: Verify Tests Pass (TDD Green Phase)

- [ ] BP-015-21: Run spider tests: `pytest tests/unit/test_doctorofcredit_spider.py -v`
- [ ] BP-015-22: Verify all tests pass
- [ ] BP-015-23: Check test coverage: `pytest tests/unit/test_doctorofcredit_spider.py --cov=app/scraping/scrapy_project/spiders/doctorofcredit_spider`

#### Phase 4: Manual Testing

- [ ] BP-015-24: Test spider in scrapy shell: `scrapy shell "https://www.doctorofcredit.com/best-bank-account-bonuses/"`
- [ ] BP-015-25: In shell, test selectors: `response.css('article.post').getall()`
- [ ] BP-015-26: Exit shell: `exit()`
- [ ] BP-015-27: Run spider: `cd app/scraping/scrapy_project && scrapy crawl doctorofcredit -o output.json -L INFO`
- [ ] BP-015-28: Check output: `cat output.json | python -m json.tool | head -50`
- [ ] BP-015-29: Verify data quality: Count bonuses with bank names and amounts
- [ ] BP-015-30: Navigate back: `cd ../../..`

#### Phase 5: Git Operations

- [ ] BP-015-31: Git add spider files: `git add app/scraping/scrapy_project/spiders/doctorofcredit_spider.py`
- [ ] BP-015-32: Git add test files: `git add tests/unit/test_doctorofcredit_spider.py tests/fixtures/sample_html/doctorofcredit.html`
- [ ] BP-015-33: Git commit: `git commit -m "[BP-015] Implement Doctor of Credit spider with TDD"`
- [ ] BP-015-34: Git push to remote: `git push origin master`

---

### BP-016: Implement Data Extraction Pipeline

**Type**: Feature | **Priority**: High | **Story Points**: 5 | **TDD**: Yes

#### Phase 1: Write Tests (TDD Red Phase)

- [ ] BP-016-01: Create `tests/unit/test_ner_extractor.py` file
- [ ] BP-016-02: Write test for NERExtractor initialization
- [ ] BP-016-03: Write test for extract_money() with various formats
- [ ] BP-016-04: Write test for extract_dates() with various formats
- [ ] BP-016-05: Write test for extract_organizations() with bank names
- [ ] BP-016-06: Create `tests/unit/test_rule_extractor.py` file
- [ ] BP-016-07: Write test for RuleExtractor initialization
- [ ] BP-016-08: Write test for extract_requirements() with various patterns
- [ ] BP-016-09: Write test for extract_account_type() with various text
- [ ] BP-016-10: Write test for extract_amount_from_text() with regex patterns
- [ ] BP-016-11: Run tests (should fail): `pytest tests/unit/test_ner_extractor.py tests/unit/test_rule_extractor.py`

#### Phase 2: Implement NER Extractor (TDD Green Phase)

- [ ] BP-016-12: Create `app/scraping/extractors/__init__.py` (empty)
- [ ] BP-016-13: Create `app/scraping/extractors/ner_extractor.py` file
- [ ] BP-016-14: Import spacy, datetime, and logging
- [ ] BP-016-15: Define NERExtractor class
- [ ] BP-016-16: Implement __init__() to load spaCy model with error handling
- [ ] BP-016-17: Implement extract_money() method with MONEY entity extraction
- [ ] BP-016-18: Add error handling for invalid amounts in extract_money()
- [ ] BP-016-19: Implement extract_dates() method with DATE entity extraction
- [ ] BP-016-20: Implement extract_organizations() method with ORG entity extraction
- [ ] BP-016-21: Add null checks to all methods

#### Phase 3: Implement Rule Extractor (TDD Green Phase)

- [ ] BP-016-22: Create `app/scraping/extractors/rule_extractor.py` file
- [ ] BP-016-23: Import re and logging
- [ ] BP-016-24: Define RuleExtractor class
- [ ] BP-016-25: Implement __init__() with requirement_patterns dictionary (6 types)
- [ ] BP-016-26: Add account_type_patterns dictionary (5 types) to __init__()
- [ ] BP-016-27: Implement extract_requirements() method with pattern matching
- [ ] BP-016-28: Add value extraction for numeric requirements
- [ ] BP-016-29: Implement extract_account_type() method with pattern matching
- [ ] BP-016-30: Implement extract_amount_from_text() method with regex patterns
- [ ] BP-016-31: Add error handling to all extraction methods

#### Phase 4: Verify Tests Pass (TDD Green Phase)

- [ ] BP-016-32: Run NER extractor tests: `pytest tests/unit/test_ner_extractor.py -v`
- [ ] BP-016-33: Run rule extractor tests: `pytest tests/unit/test_rule_extractor.py -v`
- [ ] BP-016-34: Verify all tests pass
- [ ] BP-016-35: Check coverage: `pytest tests/unit/test_*_extractor.py --cov=app/scraping/extractors`

#### Phase 5: Manual Testing

- [ ] BP-016-36: Test NER extractor: `python -c "from app.scraping.extractors.ner_extractor import NERExtractor; ner = NERExtractor(); print(ner.extract_money('Earn $300 bonus'))"`
- [ ] BP-016-37: Verify output: [300.0]
- [ ] BP-016-38: Test rule extractor: `python -c "from app.scraping.extractors.rule_extractor import RuleExtractor; rule = RuleExtractor(); print(rule.extract_requirements('direct deposit of $500'))"`
- [ ] BP-016-39: Test account type extraction: `python -c "from app.scraping.extractors.rule_extractor import RuleExtractor; rule = RuleExtractor(); print(rule.extract_account_type('Chase Total Checking'))"`
- [ ] BP-016-40: Verify output: checking

#### Phase 6: Git Operations

- [ ] BP-016-41: Git add extractor files: `git add app/scraping/extractors/`
- [ ] BP-016-42: Git add test files: `git add tests/unit/test_ner_extractor.py tests/unit/test_rule_extractor.py`
- [ ] BP-016-43: Git commit: `git commit -m "[BP-016] Implement data extraction pipeline with TDD"`
- [ ] BP-016-44: Git push to remote: `git push origin master`

---

### BP-017: Implement Data Cleaning Pipeline

**Type**: Feature | **Priority**: Medium | **Story Points**: 3 | **TDD**: Yes

#### Phase 1: Write Tests (TDD Red Phase)

- [ ] BP-017-01: Create `tests/unit/test_data_cleaner.py` file
- [ ] BP-017-02: Write test for DataCleaner initialization
- [ ] BP-017-03: Write test for clean_amount() with various inputs
- [ ] BP-017-04: Write test for clean_text() with whitespace and special chars
- [ ] BP-017-05: Write test for normalize_bank_name() with variations
- [ ] BP-017-06: Write test for validate_bonus_data() with complete/incomplete data
- [ ] BP-017-07: Run tests (should fail): `pytest tests/unit/test_data_cleaner.py`

#### Phase 2: Implement Data Cleaner (TDD Green Phase)

- [ ] BP-017-08: Create `app/scraping/cleaners/__init__.py` (empty)
- [ ] BP-017-09: Create `app/scraping/cleaners/data_cleaner.py` file
- [ ] BP-017-10: Import re, logging, and decimal
- [ ] BP-017-11: Define DataCleaner class
- [ ] BP-017-12: Implement clean_amount() method to normalize dollar amounts
- [ ] BP-017-13: Add validation to clean_amount() for negative/zero amounts
- [ ] BP-017-14: Implement clean_text() method to remove extra whitespace
- [ ] BP-017-15: Add HTML entity decoding to clean_text()
- [ ] BP-017-16: Implement normalize_bank_name() method with common variations
- [ ] BP-017-17: Create bank name mapping dictionary (Chase, Bank of America, etc.)
- [ ] BP-017-18: Implement validate_bonus_data() method checking required fields
- [ ] BP-017-19: Add logging for validation failures
- [ ] BP-017-20: Add docstrings to all methods

#### Phase 3: Verify Tests Pass (TDD Green Phase)

- [ ] BP-017-21: Run data cleaner tests: `pytest tests/unit/test_data_cleaner.py -v`
- [ ] BP-017-22: Verify all tests pass
- [ ] BP-017-23: Check coverage: `pytest tests/unit/test_data_cleaner.py --cov=app/scraping/cleaners`

#### Phase 4: Manual Testing

- [ ] BP-017-24: Test clean_amount: `python -c "from app.scraping.cleaners.data_cleaner import DataCleaner; dc = DataCleaner(); print(dc.clean_amount('$1,234.56'))"`
- [ ] BP-017-25: Verify output: 1234.56
- [ ] BP-017-26: Test clean_text: `python -c "from app.scraping.cleaners.data_cleaner import DataCleaner; dc = DataCleaner(); print(dc.clean_text('  Extra   spaces  '))"`
- [ ] BP-017-27: Test normalize_bank_name: `python -c "from app.scraping.cleaners.data_cleaner import DataCleaner; dc = DataCleaner(); print(dc.normalize_bank_name('chase bank'))"`
- [ ] BP-017-28: Verify output: Chase

#### Phase 5: Git Operations

- [ ] BP-017-29: Git add cleaner files: `git add app/scraping/cleaners/`
- [ ] BP-017-30: Git add test files: `git add tests/unit/test_data_cleaner.py`
- [ ] BP-017-31: Git commit: `git commit -m "[BP-017] Implement data cleaning pipeline with TDD"`
- [ ] BP-017-32: Git push to remote: `git push origin master`

---

### BP-018: Implement Scrapy Pipeline

**Type**: Feature | **Priority**: High | **Story Points**: 5 | **TDD**: Yes

#### Phase 1: Write Tests (TDD Red Phase)

- [ ] BP-018-01: Create `tests/unit/test_scrapy_pipeline.py` file
- [ ] BP-018-02: Write test for BonusPipeline initialization
- [ ] BP-018-03: Write test for process_item() with valid data
- [ ] BP-018-04: Write test for process_item() with duplicate data
- [ ] BP-018-05: Write test for process_item() with invalid data
- [ ] BP-018-06: Write test for database insertion
- [ ] BP-018-07: Write test for bank matching/creation
- [ ] BP-018-08: Run tests (should fail): `pytest tests/unit/test_scrapy_pipeline.py`

#### Phase 2: Implement Pipeline (TDD Green Phase)

- [ ] BP-018-09: Create `app/scraping/scrapy_project/pipelines.py` file
- [ ] BP-018-10: Import necessary modules (Flask, db, models, extractors, cleaners)
- [ ] BP-018-11: Define BonusPipeline class
- [ ] BP-018-12: Implement __init__() to initialize extractors and cleaners
- [ ] BP-018-13: Implement open_spider() to set up Flask app context
- [ ] BP-018-14: Implement close_spider() to clean up resources
- [ ] BP-018-15: Implement process_item() method
- [ ] BP-018-16: Add data cleaning step in process_item()
- [ ] BP-018-17: Add data extraction step (requirements, account type) in process_item()
- [ ] BP-018-18: Add bank matching/creation logic in process_item()
- [ ] BP-018-19: Add duplicate checking logic in process_item()
- [ ] BP-018-20: Add database insertion logic in process_item()
- [ ] BP-018-21: Add error handling and logging throughout
- [ ] BP-018-22: Add statistics tracking (items processed, saved, skipped)

#### Phase 3: Configure Pipeline

- [ ] BP-018-23: Open `app/scraping/scrapy_project/settings.py`
- [ ] BP-018-24: Add ITEM_PIPELINES configuration with BonusPipeline priority 300
- [ ] BP-018-25: Add Flask app configuration to settings
- [ ] BP-018-26: Save settings.py

#### Phase 4: Verify Tests Pass (TDD Green Phase)

- [ ] BP-018-27: Run pipeline tests: `pytest tests/unit/test_scrapy_pipeline.py -v`
- [ ] BP-018-28: Verify all tests pass
- [ ] BP-018-29: Check coverage: `pytest tests/unit/test_scrapy_pipeline.py --cov=app/scraping/scrapy_project/pipelines`

#### Phase 5: Integration Testing

- [ ] BP-018-30: Run spider with pipeline: `cd app/scraping/scrapy_project && scrapy crawl doctorofcredit -L INFO`
- [ ] BP-018-31: Check logs for pipeline processing
- [ ] BP-018-32: Verify database: `psql -U bankperks -d bankperks -c "SELECT COUNT(*) FROM bonuses;"`
- [ ] BP-018-33: Check inserted bonuses: `psql -U bankperks -d bankperks -c "SELECT bank_id, amount, title FROM bonuses LIMIT 5;"`
- [ ] BP-018-34: Navigate back: `cd ../../..`

#### Phase 6: Git Operations

- [ ] BP-018-35: Git add pipeline files: `git add app/scraping/scrapy_project/pipelines.py app/scraping/scrapy_project/settings.py`
- [ ] BP-018-36: Git add test files: `git add tests/unit/test_scrapy_pipeline.py`
- [ ] BP-018-37: Git commit: `git commit -m "[BP-018] Implement Scrapy pipeline with TDD"`
- [ ] BP-018-38: Git push to remote: `git push origin master`

---

## Sprint 3: Celery Task Queue

### BP-019: Setup Celery

**Type**: Setup | **Priority**: High | **Story Points**: 2 | **TDD**: No (Infrastructure)

- [ ] BP-019-01: Create `app/tasks/__init__.py` file
- [ ] BP-019-02: Import Celery and Flask
- [ ] BP-019-03: Create celery instance with Flask app factory pattern
- [ ] BP-019-04: Configure Celery broker URL from config
- [ ] BP-019-05: Configure Celery result backend from config
- [ ] BP-019-06: Add task serializer configuration (json)
- [ ] BP-019-07: Add result serializer configuration (json)
- [ ] BP-019-08: Add timezone configuration (UTC)
- [ ] BP-019-09: Add enable_utc configuration (True)
- [ ] BP-019-10: Create beat schedule dictionary (empty for now)
- [ ] BP-019-11: Test Celery import: `python -c "from app.tasks import celery; print(celery)"`
- [ ] BP-019-12: Start Celery worker: `celery -A app.tasks.celery worker --loglevel=info` (separate terminal)
- [ ] BP-019-13: Verify worker started successfully
- [ ] BP-019-14: Stop Celery worker: Ctrl+C
- [ ] BP-019-15: Test Celery beat: `celery -A app.tasks.celery beat --loglevel=info` (separate terminal)
- [ ] BP-019-16: Verify beat started successfully
- [ ] BP-019-17: Stop Celery beat: Ctrl+C
- [ ] BP-019-18: Git add Celery files: `git add app/tasks/__init__.py`
- [ ] BP-019-19: Git commit: `git commit -m "[BP-019] Setup Celery task queue"`
- [ ] BP-019-20: Git push to remote: `git push origin master`

---

### BP-020: Implement Scraping Tasks

**Type**: Feature | **Priority**: High | **Story Points**: 3 | **TDD**: Yes

#### Phase 1: Write Tests (TDD Red Phase)

- [ ] BP-020-01: Create `tests/unit/test_scraping_tasks.py` file
- [ ] BP-020-02: Write test for scrape_doctorofcredit task
- [ ] BP-020-03: Write test for scrape_all_sources task
- [ ] BP-020-04: Write test for error handling in scraping tasks
- [ ] BP-020-05: Write test for task result format
- [ ] BP-020-06: Run tests (should fail): `pytest tests/unit/test_scraping_tasks.py`

#### Phase 2: Implement Scraping Tasks (TDD Green Phase)

- [ ] BP-020-07: Create `app/tasks/scraping_tasks.py` file
- [ ] BP-020-08: Import celery, subprocess, logging, and models
- [ ] BP-020-09: Define scrape_doctorofcredit() task with @celery.task decorator
- [ ] BP-020-10: Implement subprocess call to run scrapy spider
- [ ] BP-020-11: Add error handling and retry logic to scrape_doctorofcredit()
- [ ] BP-020-12: Add logging for scraping start/end/errors
- [ ] BP-020-13: Update ScrapingSource statistics after scraping
- [ ] BP-020-14: Define scrape_all_sources() task
- [ ] BP-020-15: Implement logic to query active scraping sources
- [ ] BP-020-16: Add loop to trigger scraping for each source
- [ ] BP-020-17: Add error handling for individual source failures
- [ ] BP-020-18: Return summary of scraping results

#### Phase 3: Configure Beat Schedule

- [ ] BP-020-19: Open `app/tasks/__init__.py`
- [ ] BP-020-20: Import crontab from celery.schedules
- [ ] BP-020-21: Add 'scrape-doctorofcredit' to beat_schedule (every 6 hours)
- [ ] BP-020-22: Add 'scrape-all-sources' to beat_schedule (daily at 2 AM)
- [ ] BP-020-23: Save __init__.py

#### Phase 4: Verify Tests Pass (TDD Green Phase)

- [ ] BP-020-24: Run scraping tasks tests: `pytest tests/unit/test_scraping_tasks.py -v`
- [ ] BP-020-25: Verify all tests pass

#### Phase 5: Manual Testing

- [ ] BP-020-26: Start Celery worker: `celery -A app.tasks.celery worker --loglevel=info` (separate terminal)
- [ ] BP-020-27: Test task manually: `python -c "from app.tasks.scraping_tasks import scrape_doctorofcredit; result = scrape_doctorofcredit.delay(); print(result.id)"`
- [ ] BP-020-28: Check task result: Monitor Celery worker logs
- [ ] BP-020-29: Verify database updated: `psql -U bankperks -d bankperks -c "SELECT COUNT(*) FROM bonuses;"`
- [ ] BP-020-30: Stop Celery worker: Ctrl+C

#### Phase 6: Git Operations

- [ ] BP-020-31: Git add task files: `git add app/tasks/scraping_tasks.py app/tasks/__init__.py`
- [ ] BP-020-32: Git add test files: `git add tests/unit/test_scraping_tasks.py`
- [ ] BP-020-33: Git commit: `git commit -m "[BP-020] Implement scraping tasks with TDD"`
- [ ] BP-020-34: Git push to remote: `git push origin master`

---

### BP-021: Implement Maintenance Tasks

**Type**: Feature | **Priority**: Medium | **Story Points**: 2 | **TDD**: Yes

#### Phase 1: Write Tests (TDD Red Phase)

- [ ] BP-021-01: Create `tests/unit/test_maintenance_tasks.py` file
- [ ] BP-021-02: Write test for expire_old_bonuses task
- [ ] BP-021-03: Write test for cleanup_old_celery_tasks task
- [ ] BP-021-04: Write test for update_bonus_statistics task
- [ ] BP-021-05: Run tests (should fail): `pytest tests/unit/test_maintenance_tasks.py`

#### Phase 2: Implement Maintenance Tasks (TDD Green Phase)

- [ ] BP-021-06: Create `app/tasks/maintenance_tasks.py` file
- [ ] BP-021-07: Import celery, datetime, logging, and models
- [ ] BP-021-08: Define expire_old_bonuses() task with @celery.task decorator
- [ ] BP-021-09: Implement query for bonuses with expiration_date < today
- [ ] BP-021-10: Update status to 'expired' for matching bonuses
- [ ] BP-021-11: Add logging for number of bonuses expired
- [ ] BP-021-12: Define cleanup_old_celery_tasks() task
- [ ] BP-021-13: Implement deletion of celery_tasks older than 30 days
- [ ] BP-021-14: Add logging for number of tasks deleted
- [ ] BP-021-15: Define update_bonus_statistics() task
- [ ] BP-021-16: Implement calculation of active bonuses count
- [ ] BP-021-17: Implement calculation of average bonus amount
- [ ] BP-021-18: Add logging for statistics

#### Phase 3: Configure Beat Schedule

- [ ] BP-021-19: Open `app/tasks/__init__.py`
- [ ] BP-021-20: Add 'expire-old-bonuses' to beat_schedule (daily at 1 AM)
- [ ] BP-021-21: Add 'cleanup-old-celery-tasks' to beat_schedule (weekly on Sunday)
- [ ] BP-021-22: Add 'update-bonus-statistics' to beat_schedule (daily at 3 AM)
- [ ] BP-021-23: Save __init__.py

#### Phase 4: Verify Tests Pass (TDD Green Phase)

- [ ] BP-021-24: Run maintenance tasks tests: `pytest tests/unit/test_maintenance_tasks.py -v`
- [ ] BP-021-25: Verify all tests pass

#### Phase 5: Manual Testing

- [ ] BP-021-26: Start Celery worker: `celery -A app.tasks.celery worker --loglevel=info` (separate terminal)
- [ ] BP-021-27: Test expire task: `python -c "from app.tasks.maintenance_tasks import expire_old_bonuses; result = expire_old_bonuses.delay(); print(result.id)"`
- [ ] BP-021-28: Check task result in worker logs
- [ ] BP-021-29: Stop Celery worker: Ctrl+C

#### Phase 6: Git Operations

- [ ] BP-021-30: Git add task files: `git add app/tasks/maintenance_tasks.py app/tasks/__init__.py`
- [ ] BP-021-31: Git add test files: `git add tests/unit/test_maintenance_tasks.py`
- [ ] BP-021-32: Git commit: `git commit -m "[BP-021] Implement maintenance tasks with TDD"`
- [ ] BP-021-33: Git push to remote: `git push origin master`

---

## Sprint 4: AI Content Generation

### BP-022: Implement LLM Client

**Type**: Feature | **Priority**: High | **Story Points**: 3 | **TDD**: Yes

#### Phase 1: Write Tests (TDD Red Phase)

- [ ] BP-022-01: Create `tests/unit/test_llm_client.py` file
- [ ] BP-022-02: Write test for LLMClient initialization
- [ ] BP-022-03: Write test for generate() method with OpenAI
- [ ] BP-022-04: Write test for generate() method with Anthropic
- [ ] BP-022-05: Write test for retry logic on API failures
- [ ] BP-022-06: Write test for error handling with invalid API keys
- [ ] BP-022-07: Run tests (should fail): `pytest tests/unit/test_llm_client.py`

#### Phase 2: Implement LLM Client (TDD Green Phase)

- [ ] BP-022-08: Create `app/ai/__init__.py` (empty)
- [ ] BP-022-09: Create `app/ai/llm_client.py` file
- [ ] BP-022-10: Import openai, anthropic, logging, and time
- [ ] BP-022-11: Define LLMClient class
- [ ] BP-022-12: Implement __init__() with provider parameter (openai/anthropic)
- [ ] BP-022-13: Add API key loading from environment variables
- [ ] BP-022-14: Add model configuration (gpt-4, claude-3-opus)
- [ ] BP-022-15: Implement generate() method with prompt parameter
- [ ] BP-022-16: Add OpenAI API call logic in generate()
- [ ] BP-022-17: Add Anthropic API call logic in generate()
- [ ] BP-022-18: Implement retry logic with exponential backoff
- [ ] BP-022-19: Add error handling for API failures
- [ ] BP-022-20: Add logging for API calls and errors
- [ ] BP-022-21: Add docstrings to all methods

#### Phase 3: Verify Tests Pass (TDD Green Phase)

- [ ] BP-022-22: Run LLM client tests: `pytest tests/unit/test_llm_client.py -v`
- [ ] BP-022-23: Verify all tests pass
- [ ] BP-022-24: Check coverage: `pytest tests/unit/test_llm_client.py --cov=app/ai/llm_client`

#### Phase 4: Manual Testing

- [ ] BP-022-25: Test OpenAI client: `python -c "from app.ai.llm_client import LLMClient; client = LLMClient('openai'); print(client.generate('Say hello'))"`
- [ ] BP-022-26: Verify response received
- [ ] BP-022-27: Test Anthropic client: `python -c "from app.ai.llm_client import LLMClient; client = LLMClient('anthropic'); print(client.generate('Say hello'))"`
- [ ] BP-022-28: Verify response received

#### Phase 5: Git Operations

- [ ] BP-022-29: Git add AI files: `git add app/ai/`
- [ ] BP-022-30: Git add test files: `git add tests/unit/test_llm_client.py`
- [ ] BP-022-31: Git commit: `git commit -m "[BP-022] Implement LLM client with TDD"`
- [ ] BP-022-32: Git push to remote: `git push origin master`

---

### BP-023: Create Prompt Templates

**Type**: Feature | **Priority**: Medium | **Story Points**: 2 | **TDD**: Yes

#### Phase 1: Write Tests (TDD Red Phase)

- [ ] BP-023-01: Create `tests/unit/test_prompt_templates.py` file
- [ ] BP-023-02: Write test for article_prompt template rendering
- [ ] BP-023-03: Write test for comparison_prompt template rendering
- [ ] BP-023-04: Write test for newsletter_prompt template rendering
- [ ] BP-023-05: Write test for template variable substitution
- [ ] BP-023-06: Run tests (should fail): `pytest tests/unit/test_prompt_templates.py`

#### Phase 2: Create Prompt Templates (TDD Green Phase)

- [ ] BP-023-07: Create `app/ai/prompts/__init__.py` (empty)
- [ ] BP-023-08: Create `app/ai/prompts/article_prompt.txt` file
- [ ] BP-023-09: Write article generation prompt with placeholders for bonus details
- [ ] BP-023-10: Add instructions for tone, length, and structure
- [ ] BP-023-11: Create `app/ai/prompts/comparison_prompt.txt` file
- [ ] BP-023-12: Write comparison article prompt with placeholders for multiple bonuses
- [ ] BP-023-13: Add instructions for comparison criteria
- [ ] BP-023-14: Create `app/ai/prompts/newsletter_prompt.txt` file
- [ ] BP-023-15: Write newsletter prompt with placeholders for bonuses and articles
- [ ] BP-023-16: Add instructions for email format and sections

#### Phase 3: Create Template Loader

- [ ] BP-023-17: Create `app/ai/prompts/template_loader.py` file
- [ ] BP-023-18: Import os and string.Template
- [ ] BP-023-19: Define load_template() function with template_name parameter
- [ ] BP-023-20: Implement file reading logic
- [ ] BP-023-21: Define render_template() function with template_name and variables
- [ ] BP-023-22: Implement template rendering with variable substitution
- [ ] BP-023-23: Add error handling for missing templates
- [ ] BP-023-24: Add error handling for missing variables

#### Phase 4: Verify Tests Pass (TDD Green Phase)

- [ ] BP-023-25: Run prompt template tests: `pytest tests/unit/test_prompt_templates.py -v`
- [ ] BP-023-26: Verify all tests pass

#### Phase 5: Manual Testing

- [ ] BP-023-27: Test template loading: `python -c "from app.ai.prompts.template_loader import load_template; print(load_template('article_prompt.txt')[:100])"`
- [ ] BP-023-28: Test template rendering: `python -c "from app.ai.prompts.template_loader import render_template; print(render_template('article_prompt.txt', {'bonus_title': 'Chase $300', 'bonus_amount': '300'}))"`
- [ ] BP-023-29: Verify variable substitution worked

#### Phase 6: Git Operations

- [ ] BP-023-30: Git add prompt files: `git add app/ai/prompts/`
- [ ] BP-023-31: Git add test files: `git add tests/unit/test_prompt_templates.py`
- [ ] BP-023-32: Git commit: `git commit -m "[BP-023] Create prompt templates with TDD"`
- [ ] BP-023-33: Git push to remote: `git push origin master`

---

### BP-024: Implement Content Generator

**Type**: Feature | **Priority**: High | **Story Points**: 4 | **TDD**: Yes

#### Phase 1: Write Tests (TDD Red Phase)

- [ ] BP-024-01: Create `tests/unit/test_content_generator.py` file
- [ ] BP-024-02: Write test for ContentGenerator initialization
- [ ] BP-024-03: Write test for generate_article() method
- [ ] BP-024-04: Write test for generate_comparison() method
- [ ] BP-024-05: Write test for generate_newsletter() method
- [ ] BP-024-06: Write test for error handling with LLM failures
- [ ] BP-024-07: Run tests (should fail): `pytest tests/unit/test_content_generator.py`

#### Phase 2: Implement Content Generator (TDD Green Phase)

- [ ] BP-024-08: Create `app/ai/content_generator.py` file
- [ ] BP-024-09: Import LLMClient, template_loader, logging, and models
- [ ] BP-024-10: Define ContentGenerator class
- [ ] BP-024-11: Implement __init__() with provider parameter
- [ ] BP-024-12: Initialize LLMClient in __init__()
- [ ] BP-024-13: Implement generate_article() method with bonus_id parameter
- [ ] BP-024-14: Add bonus data fetching in generate_article()
- [ ] BP-024-15: Add prompt rendering with bonus data
- [ ] BP-024-16: Add LLM generation call
- [ ] BP-024-17: Add Article model creation and saving
- [ ] BP-024-18: Implement generate_comparison() method with bonus_ids parameter
- [ ] BP-024-19: Add multiple bonus data fetching in generate_comparison()
- [ ] BP-024-20: Add comparison prompt rendering
- [ ] BP-024-21: Add LLM generation call for comparison
- [ ] BP-024-22: Add Article model creation with type='comparison'
- [ ] BP-024-23: Implement generate_newsletter() method
- [ ] BP-024-24: Add top bonuses fetching in generate_newsletter()
- [ ] BP-024-25: Add recent articles fetching in generate_newsletter()
- [ ] BP-024-26: Add newsletter prompt rendering
- [ ] BP-024-27: Add LLM generation call for newsletter
- [ ] BP-024-28: Add error handling and logging to all methods

#### Phase 3: Verify Tests Pass (TDD Green Phase)

- [ ] BP-024-29: Run content generator tests: `pytest tests/unit/test_content_generator.py -v`
- [ ] BP-024-30: Verify all tests pass
- [ ] BP-024-31: Check coverage: `pytest tests/unit/test_content_generator.py --cov=app/ai/content_generator`

#### Phase 4: Manual Testing

- [ ] BP-024-32: Start Flask shell: `flask shell`
- [ ] BP-024-33: Test article generation: `from app.ai.content_generator import ContentGenerator; cg = ContentGenerator(); article = cg.generate_article(1); print(article.title)`
- [ ] BP-024-34: Verify article created
- [ ] BP-024-35: Test comparison generation: `comparison = cg.generate_comparison([1, 2, 3]); print(comparison.title)`
- [ ] BP-024-36: Exit Flask shell: `exit()`

#### Phase 5: Git Operations

- [ ] BP-024-37: Git add generator files: `git add app/ai/content_generator.py`
- [ ] BP-024-38: Git add test files: `git add tests/unit/test_content_generator.py`
- [ ] BP-024-39: Git commit: `git commit -m "[BP-024] Implement content generator with TDD"`
- [ ] BP-024-40: Git push to remote: `git push origin master`

---

### BP-025: Implement AI Generation Tasks

**Type**: Feature | **Priority**: Medium | **Story Points**: 3 | **TDD**: Yes

#### Phase 1: Write Tests (TDD Red Phase)

- [ ] BP-025-01: Create `tests/unit/test_ai_tasks.py` file
- [ ] BP-025-02: Write test for generate_article_for_bonus task
- [ ] BP-025-03: Write test for generate_weekly_comparison task
- [ ] BP-025-04: Write test for generate_newsletter task (complete implementation)
- [ ] BP-025-05: Write test for error handling in AI tasks
- [ ] BP-025-06: Run tests (should fail): `pytest tests/unit/test_ai_tasks.py`

#### Phase 2: Implement AI Tasks (TDD Green Phase)

- [ ] BP-025-07: Create `app/tasks/ai_tasks.py` file
- [ ] BP-025-08: Import celery, ContentGenerator, logging, and models
- [ ] BP-025-09: Define generate_article_for_bonus() task with bonus_id parameter
- [ ] BP-025-10: Implement ContentGenerator initialization
- [ ] BP-025-11: Add article generation call
- [ ] BP-025-12: Add error handling and retry logic
- [ ] BP-025-13: Define generate_weekly_comparison() task
- [ ] BP-025-14: Implement query for top 5 bonuses by amount
- [ ] BP-025-15: Add comparison generation call
- [ ] BP-025-16: Define generate_newsletter() task (complete code from improved tickets)
- [ ] BP-025-17: Implement query for top 5 active bonuses
- [ ] BP-025-18: Implement query for recent articles (last 7 days)
- [ ] BP-025-19: Add newsletter content generation with LLM
- [ ] BP-025-20: Create Newsletter model instance with status='draft'
- [ ] BP-025-21: Add database commit
- [ ] BP-025-22: Add error handling and logging

#### Phase 3: Configure Beat Schedule

- [ ] BP-025-23: Open `app/tasks/__init__.py`
- [ ] BP-025-24: Add 'generate-weekly-comparison' to beat_schedule (weekly on Monday)
- [ ] BP-025-25: Add 'generate-newsletter' to beat_schedule (weekly on Friday)
- [ ] BP-025-26: Save __init__.py

#### Phase 4: Verify Tests Pass (TDD Green Phase)

- [ ] BP-025-27: Run AI tasks tests: `pytest tests/unit/test_ai_tasks.py -v`
- [ ] BP-025-28: Verify all tests pass

#### Phase 5: Manual Testing

- [ ] BP-025-29: Start Celery worker: `celery -A app.tasks.celery worker --loglevel=info` (separate terminal)
- [ ] BP-025-30: Test article task: `python -c "from app.tasks.ai_tasks import generate_article_for_bonus; result = generate_article_for_bonus.delay(1); print(result.id)"`
- [ ] BP-025-31: Check worker logs for task completion
- [ ] BP-025-32: Verify article created: `psql -U bankperks -d bankperks -c "SELECT COUNT(*) FROM articles;"`
- [ ] BP-025-33: Test newsletter task: `python -c "from app.tasks.ai_tasks import generate_newsletter; result = generate_newsletter.delay(); print(result.id)"`
- [ ] BP-025-34: Check worker logs for task completion
- [ ] BP-025-35: Verify newsletter created: `psql -U bankperks -d bankperks -c "SELECT subject, status FROM newsletters ORDER BY created_at DESC LIMIT 1;"`
- [ ] BP-025-36: Stop Celery worker: Ctrl+C

#### Phase 6: Git Operations

- [ ] BP-025-37: Git add task files: `git add app/tasks/ai_tasks.py app/tasks/__init__.py`
- [ ] BP-025-38: Git add test files: `git add tests/unit/test_ai_tasks.py`
- [ ] BP-025-39: Git commit: `git commit -m "[BP-025] Implement AI generation tasks with TDD"`
- [ ] BP-025-40: Git push to remote: `git push origin master`

---

## Sprint 5: Testing and QA

### BP-026: Write Unit Tests for Models

**Type**: Test | **Priority**: High | **Story Points**: 5 | **TDD**: N/A (Test Creation)

#### Phase 1: Create Test Infrastructure

- [ ] BP-026-01: Create `tests/conftest.py` file
- [ ] BP-026-02: Add pytest fixtures for Flask app
- [ ] BP-026-03: Add pytest fixture for database session
- [ ] BP-026-04: Add pytest fixture for test client
- [ ] BP-026-05: Add setup/teardown for test database

#### Phase 2: Write Bank Model Tests

- [ ] BP-026-06: Enhance `tests/unit/test_bank_model.py` (if not complete)
- [ ] BP-026-07: Add test for Bank model creation
- [ ] BP-026-08: Add test for Bank.to_dict() method
- [ ] BP-026-09: Add test for Bank-Bonus relationship
- [ ] BP-026-10: Add test for Bank validation (required fields)
- [ ] BP-026-11: Add test for Bank uniqueness constraints

#### Phase 3: Write Bonus Model Tests

- [ ] BP-026-12: Enhance `tests/unit/test_bonus_model.py` (if not complete)
- [ ] BP-026-13: Add test for Bonus model creation
- [ ] BP-026-14: Add test for Bonus.to_dict() with include_bank
- [ ] BP-026-15: Add test for Bonus.is_active property
- [ ] BP-026-16: Add test for Bonus.days_until_expiration property
- [ ] BP-026-17: Add test for Bonus status transitions
- [ ] BP-026-18: Add test for Bonus-Bank relationship

#### Phase 4: Write Article Model Tests

- [ ] BP-026-19: Create `tests/unit/test_article_model.py` file
- [ ] BP-026-20: Add test for Article model creation
- [ ] BP-026-21: Add test for Article.to_dict() method
- [ ] BP-026-22: Add test for Article slug generation
- [ ] BP-026-23: Add test for Article view count increment
- [ ] BP-026-24: Add test for Article status transitions

#### Phase 5: Write User Model Tests

- [ ] BP-026-25: Enhance `tests/unit/test_user_model.py` (if not complete)
- [ ] BP-026-26: Add test for User model creation
- [ ] BP-026-27: Add test for User.set_password() method
- [ ] BP-026-28: Add test for User.check_password() method
- [ ] BP-026-29: Add test for User.to_dict() with email privacy
- [ ] BP-026-30: Add test for User-SavedBonus relationship

#### Phase 6: Write Newsletter Model Tests

- [ ] BP-026-31: Create `tests/unit/test_newsletter_model.py` file
- [ ] BP-026-32: Add test for Newsletter model creation
- [ ] BP-026-33: Add test for Newsletter.to_dict() method
- [ ] BP-026-34: Add test for Newsletter status transitions
- [ ] BP-026-35: Add test for Newsletter sent_at timestamp

#### Phase 7: Write ScrapingSource Model Tests

- [ ] BP-026-36: Create `tests/unit/test_scraping_source_model.py` file
- [ ] BP-026-37: Add test for ScrapingSource model creation
- [ ] BP-026-38: Add test for ScrapingSource.success_rate property
- [ ] BP-026-39: Add test for ScrapingSource statistics updates
- [ ] BP-026-40: Add test for ScrapingSource.to_dict() method

#### Phase 8: Run All Tests

- [ ] BP-026-41: Run all model tests: `pytest tests/unit/test_*_model.py -v`
- [ ] BP-026-42: Verify all tests pass
- [ ] BP-026-43: Check coverage: `pytest tests/unit/test_*_model.py --cov=app/models --cov-report=html`
- [ ] BP-026-44: Open coverage report: `open htmlcov/index.html`
- [ ] BP-026-45: Verify coverage >90% for all models

#### Phase 9: Git Operations

- [ ] BP-026-46: Git add test files: `git add tests/unit/test_*_model.py tests/conftest.py`
- [ ] BP-026-47: Git commit: `git commit -m "[BP-026] Write comprehensive unit tests for models"`
- [ ] BP-026-48: Git push to remote: `git push origin master`

---

### BP-027: Write Integration Tests for API

**Type**: Test | **Priority**: High | **Story Points**: 5 | **TDD**: N/A (Test Creation)

#### Phase 1: Create Test Infrastructure

- [ ] BP-027-01: Update `tests/conftest.py` with API test fixtures
- [ ] BP-027-02: Add fixture for authenticated test client
- [ ] BP-027-03: Add fixture for sample data (banks, bonuses, articles)
- [ ] BP-027-04: Add helper functions for API assertions

#### Phase 2: Write Health Check Tests

- [ ] BP-027-05: Enhance `tests/integration/test_api_base.py` (if not complete)
- [ ] BP-027-06: Add test for GET /api/v1/health
- [ ] BP-027-07: Add test for 404 error handling
- [ ] BP-027-08: Add test for 500 error handling
- [ ] BP-027-09: Add test for CORS headers

#### Phase 3: Write Bonus API Tests

- [ ] BP-027-10: Enhance `tests/integration/test_api_bonuses.py` (if not complete)
- [ ] BP-027-11: Add test for GET /api/v1/bonuses (list with pagination)
- [ ] BP-027-12: Add test for GET /api/v1/bonuses?status=active
- [ ] BP-027-13: Add test for GET /api/v1/bonuses?bank_id=1
- [ ] BP-027-14: Add test for GET /api/v1/bonuses?min_amount=200&max_amount=500
- [ ] BP-027-15: Add test for GET /api/v1/bonuses?sort=-amount
- [ ] BP-027-16: Add test for GET /api/v1/bonuses?sort=+created_at
- [ ] BP-027-17: Add test for GET /api/v1/bonuses/<id>
- [ ] BP-027-18: Add test for GET /api/v1/bonuses/<uuid>
- [ ] BP-027-19: Add test for PATCH /api/v1/bonuses/<id>
- [ ] BP-027-20: Add test for 404 error (non-existent bonus)
- [ ] BP-027-21: Add test for pagination metadata

#### Phase 4: Write Bank API Tests

- [ ] BP-027-22: Enhance `tests/integration/test_api_banks.py` (if not complete)
- [ ] BP-027-23: Add test for GET /api/v1/banks (list)
- [ ] BP-027-24: Add test for GET /api/v1/banks/<id>
- [ ] BP-027-25: Add test for GET /api/v1/banks/<id>/bonuses
- [ ] BP-027-26: Add test for GET /api/v1/banks/<id>/bonuses?status=active
- [ ] BP-027-27: Add test for 404 error (non-existent bank)

#### Phase 5: Write Article API Tests

- [ ] BP-027-28: Enhance `tests/integration/test_api_articles.py` (if not complete)
- [ ] BP-027-29: Add test for GET /api/v1/articles (list)
- [ ] BP-027-30: Add test for GET /api/v1/articles?status=published
- [ ] BP-027-31: Add test for GET /api/v1/articles?article_type=comparison
- [ ] BP-027-32: Add test for GET /api/v1/articles/<slug>
- [ ] BP-027-33: Add test for GET /api/v1/articles/<id>
- [ ] BP-027-34: Add test for view count increment
- [ ] BP-027-35: Add test for 404 error (non-existent article)

#### Phase 6: Write Account Type API Tests

- [ ] BP-027-36: Create `tests/integration/test_api_account_types.py` file
- [ ] BP-027-37: Add test for GET /api/v1/account-types (list)
- [ ] BP-027-38: Add test for GET /api/v1/account-types/<id>
- [ ] BP-027-39: Add test for account type data structure

#### Phase 7: Run All Integration Tests

- [ ] BP-027-40: Run all API tests: `pytest tests/integration/ -v`
- [ ] BP-027-41: Verify all tests pass
- [ ] BP-027-42: Check coverage: `pytest tests/integration/ --cov=app/api --cov-report=html`
- [ ] BP-027-43: Open coverage report: `open htmlcov/index.html`
- [ ] BP-027-44: Verify coverage >80% for API endpoints

#### Phase 8: Git Operations

- [ ] BP-027-45: Git add test files: `git add tests/integration/test_api_*.py tests/conftest.py`
- [ ] BP-027-46: Git commit: `git commit -m "[BP-027] Write comprehensive integration tests for API"`
- [ ] BP-027-47: Git push to remote: `git push origin master`

---

### BP-028: Write Tests for Scraping Pipeline

**Type**: Test | **Priority**: Medium | **Story Points**: 3 | **TDD**: N/A (Test Creation)

#### Phase 1: Write Spider Tests

- [ ] BP-028-01: Enhance `tests/unit/test_base_spider.py` (if not complete)
- [ ] BP-028-02: Add test for parse_amount() with edge cases ($0, negative, invalid)
- [ ] BP-028-03: Add test for parse_date() with various formats
- [ ] BP-028-04: Enhance `tests/unit/test_doctorofcredit_spider.py` (if not complete)
- [ ] BP-028-05: Add test for spider with empty response
- [ ] BP-028-06: Add test for spider with malformed HTML
- [ ] BP-028-07: Add test for pagination logic

#### Phase 2: Write Extractor Tests

- [ ] BP-028-08: Enhance `tests/unit/test_ner_extractor.py` (if not complete)
- [ ] BP-028-09: Add test for extract_money() with edge cases
- [ ] BP-028-10: Add test for extract_dates() with various formats
- [ ] BP-028-11: Add test for extract_organizations() with bank variations
- [ ] BP-028-12: Enhance `tests/unit/test_rule_extractor.py` (if not complete)
- [ ] BP-028-13: Add test for all 6 requirement types
- [ ] BP-028-14: Add test for all 5 account types
- [ ] BP-028-15: Add test for extract_amount_from_text() with edge cases

#### Phase 3: Write Cleaner Tests

- [ ] BP-028-16: Enhance `tests/unit/test_data_cleaner.py` (if not complete)
- [ ] BP-028-17: Add test for clean_amount() with various formats
- [ ] BP-028-18: Add test for clean_text() with HTML entities
- [ ] BP-028-19: Add test for normalize_bank_name() with all variations
- [ ] BP-028-20: Add test for validate_bonus_data() with missing fields

#### Phase 4: Write Pipeline Tests

- [ ] BP-028-21: Enhance `tests/unit/test_scrapy_pipeline.py` (if not complete)
- [ ] BP-028-22: Add test for process_item() with valid data
- [ ] BP-028-23: Add test for process_item() with duplicate detection
- [ ] BP-028-24: Add test for process_item() with invalid data
- [ ] BP-028-25: Add test for bank matching logic
- [ ] BP-028-26: Add test for bank creation logic
- [ ] BP-028-27: Add test for database insertion
- [ ] BP-028-28: Add test for error handling

#### Phase 5: Write Task Tests

- [ ] BP-028-29: Enhance `tests/unit/test_scraping_tasks.py` (if not complete)
- [ ] BP-028-30: Add test for scrape_doctorofcredit task success
- [ ] BP-028-31: Add test for scrape_doctorofcredit task failure
- [ ] BP-028-32: Add test for scrape_all_sources task
- [ ] BP-028-33: Add test for ScrapingSource statistics update

#### Phase 6: Run All Scraping Tests

- [ ] BP-028-34: Run all scraping tests: `pytest tests/unit/test_*spider*.py tests/unit/test_*extractor*.py tests/unit/test_*cleaner*.py tests/unit/test_*pipeline*.py -v`
- [ ] BP-028-35: Verify all tests pass
- [ ] BP-028-36: Check coverage: `pytest tests/unit/test_*spider*.py tests/unit/test_*extractor*.py --cov=app/scraping --cov-report=html`
- [ ] BP-028-37: Open coverage report: `open htmlcov/index.html`
- [ ] BP-028-38: Verify coverage >80% for scraping components

#### Phase 7: Git Operations

- [ ] BP-028-39: Git add test files: `git add tests/unit/test_*spider*.py tests/unit/test_*extractor*.py tests/unit/test_*cleaner*.py tests/unit/test_*pipeline*.py`
- [ ] BP-028-40: Git commit: `git commit -m "[BP-028] Write comprehensive tests for scraping pipeline"`
- [ ] BP-028-41: Git push to remote: `git push origin master`

---

## Sprint 6: Deployment and Operations

### BP-029: Create Docker Configuration

**Type**: DevOps | **Priority**: High | **Story Points**: 4 | **TDD**: No (Infrastructure)

#### Phase 1: Create Dockerfile

- [ ] BP-029-01: Create `Dockerfile` in project root
- [ ] BP-029-02: Set base image to python:3.11-slim
- [ ] BP-029-03: Set working directory to /app
- [ ] BP-029-04: Copy requirements files
- [ ] BP-029-05: Install system dependencies (postgresql-client, etc.)
- [ ] BP-029-06: Install Python dependencies
- [ ] BP-029-07: Install Playwright browsers
- [ ] BP-029-08: Copy application code
- [ ] BP-029-09: Set environment variables
- [ ] BP-029-10: Expose port 5000
- [ ] BP-029-11: Set CMD to run Gunicorn

#### Phase 2: Create Docker Compose

- [ ] BP-029-12: Create `docker-compose.yml` in project root
- [ ] BP-029-13: Define postgres service with PostgreSQL 16
- [ ] BP-029-14: Add postgres environment variables
- [ ] BP-029-15: Add postgres volume for data persistence
- [ ] BP-029-16: Define redis service with Redis 7
- [ ] BP-029-17: Add redis volume for data persistence
- [ ] BP-029-18: Define web service (Flask app)
- [ ] BP-029-19: Add web service build context
- [ ] BP-029-20: Add web service environment variables
- [ ] BP-029-21: Add web service ports (5000:5000)
- [ ] BP-029-22: Add web service depends_on (postgres, redis)
- [ ] BP-029-23: Define celery-worker service
- [ ] BP-029-24: Add celery-worker command
- [ ] BP-029-25: Add celery-worker depends_on (postgres, redis, web)
- [ ] BP-029-26: Define celery-beat service
- [ ] BP-029-27: Add celery-beat command
- [ ] BP-029-28: Add celery-beat depends_on (postgres, redis, web)

#### Phase 3: Create .dockerignore

- [ ] BP-029-29: Create `.dockerignore` file
- [ ] BP-029-30: Add venv/ to .dockerignore
- [ ] BP-029-31: Add __pycache__/ to .dockerignore
- [ ] BP-029-32: Add .git/ to .dockerignore
- [ ] BP-029-33: Add *.pyc to .dockerignore
- [ ] BP-029-34: Add .env to .dockerignore

#### Phase 4: Test Docker Build

- [ ] BP-029-35: Build Docker image: `docker build -t bankperks:latest .`
- [ ] BP-029-36: Verify build successful
- [ ] BP-029-37: Check image size: `docker images | grep bankperks`

#### Phase 5: Test Docker Compose

- [ ] BP-029-38: Start services: `docker-compose up -d`
- [ ] BP-029-39: Check services running: `docker-compose ps`
- [ ] BP-029-40: Check web logs: `docker-compose logs web`
- [ ] BP-029-41: Check celery-worker logs: `docker-compose logs celery-worker`
- [ ] BP-029-42: Test web service: `curl http://localhost:5000/api/v1/health`
- [ ] BP-029-43: Verify health check response
- [ ] BP-029-44: Stop services: `docker-compose down`

#### Phase 6: Git Operations

- [ ] BP-029-45: Git add Docker files: `git add Dockerfile docker-compose.yml .dockerignore`
- [ ] BP-029-46: Git commit: `git commit -m "[BP-029] Create Docker configuration"`
- [ ] BP-029-47: Git push to remote: `git push origin master`

---

### BP-030: Configure Nginx Reverse Proxy

**Type**: DevOps | **Priority**: Medium | **Story Points**: 2 | **TDD**: No (Infrastructure)

#### Phase 1: Create Nginx Configuration

- [ ] BP-030-01: Create `docker/nginx/` directory: `mkdir -p docker/nginx`
- [ ] BP-030-02: Create `docker/nginx/nginx.conf` file
- [ ] BP-030-03: Add user and worker_processes directives
- [ ] BP-030-04: Add events block with worker_connections
- [ ] BP-030-05: Add http block
- [ ] BP-030-06: Configure upstream backend (Flask app)
- [ ] BP-030-07: Add server block listening on port 80
- [ ] BP-030-08: Configure server_name
- [ ] BP-030-09: Add location / block with proxy_pass to backend
- [ ] BP-030-10: Add proxy headers (Host, X-Real-IP, X-Forwarded-For, X-Forwarded-Proto)
- [ ] BP-030-11: Add location /static/ block for static files
- [ ] BP-030-12: Add location /uploads/ block for uploaded files
- [ ] BP-030-13: Add gzip compression configuration
- [ ] BP-030-14: Add client_max_body_size for file uploads
- [ ] BP-030-15: Add access_log and error_log configuration

#### Phase 2: Create SSL Configuration (Optional)

- [ ] BP-030-16: Create `docker/nginx/ssl.conf` file
- [ ] BP-030-17: Add SSL certificate paths
- [ ] BP-030-18: Add SSL protocols and ciphers
- [ ] BP-030-19: Add SSL session configuration
- [ ] BP-030-20: Add HSTS header

#### Phase 3: Update Docker Compose

- [ ] BP-030-21: Open `docker-compose.yml`
- [ ] BP-030-22: Add nginx service definition
- [ ] BP-030-23: Set nginx image to nginx:alpine
- [ ] BP-030-24: Add nginx ports (80:80, 443:443)
- [ ] BP-030-25: Add nginx volumes (nginx.conf, static files)
- [ ] BP-030-26: Add nginx depends_on (web)
- [ ] BP-030-27: Save docker-compose.yml

#### Phase 4: Test Nginx Configuration

- [ ] BP-030-28: Test nginx config syntax: `docker run --rm -v $(pwd)/docker/nginx/nginx.conf:/etc/nginx/nginx.conf:ro nginx:alpine nginx -t`
- [ ] BP-030-29: Verify syntax OK
- [ ] BP-030-30: Start services with nginx: `docker-compose up -d`
- [ ] BP-030-31: Check nginx logs: `docker-compose logs nginx`
- [ ] BP-030-32: Test through nginx: `curl http://localhost/api/v1/health`
- [ ] BP-030-33: Verify response received
- [ ] BP-030-34: Stop services: `docker-compose down`

#### Phase 5: Git Operations

- [ ] BP-030-35: Git add nginx files: `git add docker/nginx/ docker-compose.yml`
- [ ] BP-030-36: Git commit: `git commit -m "[BP-030] Configure Nginx reverse proxy"`
- [ ] BP-030-37: Git push to remote: `git push origin master`

---

### BP-031: Create Deployment Scripts

**Type**: DevOps | **Priority**: Medium | **Story Points**: 3 | **TDD**: No (Infrastructure)

#### Phase 1: Create Deployment Script

- [ ] BP-031-01: Create `scripts/deploy.sh` file
- [ ] BP-031-02: Add shebang: `#!/bin/bash`
- [ ] BP-031-03: Add set -e for error handling
- [ ] BP-031-04: Add environment variable checks
- [ ] BP-031-05: Add git pull command
- [ ] BP-031-06: Add docker-compose build command
- [ ] BP-031-07: Add database migration command
- [ ] BP-031-08: Add docker-compose up -d command
- [ ] BP-031-09: Add health check verification
- [ ] BP-031-10: Add success message
- [ ] BP-031-11: Make script executable: `chmod +x scripts/deploy.sh`

#### Phase 2: Create Database Backup Script

- [ ] BP-031-12: Create `scripts/backup_db.sh` file
- [ ] BP-031-13: Add shebang and error handling
- [ ] BP-031-14: Add timestamp generation
- [ ] BP-031-15: Add pg_dump command with docker exec
- [ ] BP-031-16: Add backup file compression
- [ ] BP-031-17: Add old backup cleanup (keep last 7 days)
- [ ] BP-031-18: Add success message
- [ ] BP-031-19: Make script executable: `chmod +x scripts/backup_db.sh`

#### Phase 3: Create Database Restore Script

- [ ] BP-031-20: Create `scripts/restore_db.sh` file
- [ ] BP-031-21: Add shebang and error handling
- [ ] BP-031-22: Add backup file parameter check
- [ ] BP-031-23: Add confirmation prompt
- [ ] BP-031-24: Add database drop and recreate commands
- [ ] BP-031-25: Add psql restore command with docker exec
- [ ] BP-031-26: Add success message
- [ ] BP-031-27: Make script executable: `chmod +x scripts/restore_db.sh`

#### Phase 4: Create Log Rotation Script

- [ ] BP-031-28: Create `scripts/rotate_logs.sh` file
- [ ] BP-031-29: Add shebang and error handling
- [ ] BP-031-30: Add log file compression
- [ ] BP-031-31: Add old log cleanup (keep last 30 days)
- [ ] BP-031-32: Add success message
- [ ] BP-031-33: Make script executable: `chmod +x scripts/rotate_logs.sh`

#### Phase 5: Create Environment Setup Script

- [ ] BP-031-34: Create `scripts/setup_env.sh` file
- [ ] BP-031-35: Add shebang and error handling
- [ ] BP-031-36: Add .env file creation from .env.example
- [ ] BP-031-37: Add SECRET_KEY generation
- [ ] BP-031-38: Add database URL configuration
- [ ] BP-031-39: Add Redis URL configuration
- [ ] BP-031-40: Add success message
- [ ] BP-031-41: Make script executable: `chmod +x scripts/setup_env.sh`

#### Phase 6: Test Scripts

- [ ] BP-031-42: Test deploy script: `./scripts/deploy.sh` (in test environment)
- [ ] BP-031-43: Test backup script: `./scripts/backup_db.sh`
- [ ] BP-031-44: Verify backup file created: `ls -lh backups/`
- [ ] BP-031-45: Test setup script: `./scripts/setup_env.sh`
- [ ] BP-031-46: Verify .env file created

#### Phase 7: Git Operations

- [ ] BP-031-47: Git add scripts: `git add scripts/`
- [ ] BP-031-48: Git commit: `git commit -m "[BP-031] Create deployment scripts"`
- [ ] BP-031-49: Git push to remote: `git push origin master`

---

### BP-032: Setup Monitoring and Logging

**Type**: DevOps | **Priority**: Medium | **Story Points**: 4 | **TDD**: No (Infrastructure)

#### Phase 1: Configure Application Logging

- [ ] BP-032-01: Create `app/utils/logging_config.py` file
- [ ] BP-032-02: Import logging and logging.handlers
- [ ] BP-032-03: Define setup_logging() function
- [ ] BP-032-04: Configure root logger level
- [ ] BP-032-05: Add console handler with formatter
- [ ] BP-032-06: Add rotating file handler for app.log
- [ ] BP-032-07: Add rotating file handler for errors.log (ERROR level)
- [ ] BP-032-08: Configure log format with timestamp, level, module, message
- [ ] BP-032-09: Add logger for Flask app
- [ ] BP-032-10: Add logger for Celery tasks
- [ ] BP-032-11: Add logger for Scrapy spiders

#### Phase 2: Integrate Logging in Application

- [ ] BP-032-12: Open `app/__init__.py`
- [ ] BP-032-13: Import setup_logging
- [ ] BP-032-14: Call setup_logging() in create_app()
- [ ] BP-032-15: Save __init__.py
- [ ] BP-032-16: Test logging: `python -c "from app import create_app; app = create_app(); app.logger.info('Test log')"`
- [ ] BP-032-17: Verify log file created: `ls -lh logs/`

#### Phase 3: Configure Request Logging

- [ ] BP-032-18: Create `app/middleware/request_logger.py` file
- [ ] BP-032-19: Define RequestLogger middleware class
- [ ] BP-032-20: Implement __init__() with app parameter
- [ ] BP-032-21: Implement __call__() to log request details
- [ ] BP-032-22: Log request method, path, IP, user agent
- [ ] BP-032-23: Log response status and duration
- [ ] BP-032-24: Open `app/__init__.py`
- [ ] BP-032-25: Import RequestLogger
- [ ] BP-032-26: Add RequestLogger middleware to app
- [ ] BP-032-27: Save __init__.py

#### Phase 4: Configure Error Tracking

- [ ] BP-032-28: Create `app/utils/error_tracker.py` file
- [ ] BP-032-29: Define log_exception() function
- [ ] BP-032-30: Add exception details logging (type, message, traceback)
- [ ] BP-032-31: Add request context logging (URL, method, IP)
- [ ] BP-032-32: Add user context logging (if authenticated)
- [ ] BP-032-33: Open `app/api/errors.py`
- [ ] BP-032-34: Import log_exception
- [ ] BP-032-35: Add log_exception() calls to error handlers
- [ ] BP-032-36: Save errors.py

#### Phase 5: Configure Performance Monitoring

- [ ] BP-032-37: Create `app/middleware/performance_monitor.py` file
- [ ] BP-032-38: Define PerformanceMonitor middleware class
- [ ] BP-032-39: Implement request timing logic
- [ ] BP-032-40: Log slow requests (>1 second)
- [ ] BP-032-41: Add database query counting
- [ ] BP-032-42: Open `app/__init__.py`
- [ ] BP-032-43: Import PerformanceMonitor
- [ ] BP-032-44: Add PerformanceMonitor middleware to app
- [ ] BP-032-45: Save __init__.py

#### Phase 6: Create Health Check Endpoint

- [ ] BP-032-46: Open `app/api/v1/health.py`
- [ ] BP-032-47: Enhance health check with database connectivity test
- [ ] BP-032-48: Add Redis connectivity test
- [ ] BP-032-49: Add disk space check
- [ ] BP-032-50: Add memory usage check
- [ ] BP-032-51: Return detailed health status
- [ ] BP-032-52: Save health.py

#### Phase 7: Test Monitoring

- [ ] BP-032-53: Start Flask app: `flask run`
- [ ] BP-032-54: Make test requests: `curl http://localhost:5000/api/v1/bonuses`
- [ ] BP-032-55: Check app.log: `tail -f logs/app.log`
- [ ] BP-032-56: Verify request logging working
- [ ] BP-032-57: Test error logging: `curl http://localhost:5000/api/v1/nonexistent`
- [ ] BP-032-58: Check errors.log: `tail -f logs/errors.log`
- [ ] BP-032-59: Test health check: `curl http://localhost:5000/api/v1/health`
- [ ] BP-032-60: Verify detailed health status
- [ ] BP-032-61: Stop Flask app: Ctrl+C

#### Phase 8: Git Operations

- [ ] BP-032-62: Git add monitoring files: `git add app/utils/logging_config.py app/utils/error_tracker.py app/middleware/`
- [ ] BP-032-63: Git add updated files: `git add app/__init__.py app/api/errors.py app/api/v1/health.py`
- [ ] BP-032-64: Git commit: `git commit -m "[BP-032] Setup monitoring and logging"`
- [ ] BP-032-65: Git push to remote: `git push origin master`

---

### BP-033: Implement Security Features

**Type**: Feature | **Priority**: High | **Story Points**: 5 | **TDD**: Yes

#### Phase 1: Write Tests (TDD Red Phase)

- [ ] BP-033-01: Create `tests/unit/test_security.py` file
- [ ] BP-033-02: Write test for rate limiting
- [ ] BP-033-03: Write test for CORS configuration
- [ ] BP-033-04: Write test for SQL injection prevention
- [ ] BP-033-05: Write test for XSS prevention
- [ ] BP-033-06: Write test for authentication token validation
- [ ] BP-033-07: Run tests (should fail): `pytest tests/unit/test_security.py`

#### Phase 2: Implement Rate Limiting (TDD Green Phase)

- [ ] BP-033-08: Open `app/__init__.py`
- [ ] BP-033-09: Import Flask-Limiter
- [ ] BP-033-10: Configure rate limiter with Redis backend
- [ ] BP-033-11: Add default rate limits (100 per hour)
- [ ] BP-033-12: Add stricter limits for API endpoints (20 per minute)
- [ ] BP-033-13: Add rate limit error handler
- [ ] BP-033-14: Save __init__.py

#### Phase 3: Implement CORS Configuration

- [ ] BP-033-15: Open `app/__init__.py`
- [ ] BP-033-16: Import Flask-CORS
- [ ] BP-033-17: Configure CORS with allowed origins from config
- [ ] BP-033-18: Set allowed methods (GET, POST, PATCH, DELETE)
- [ ] BP-033-19: Set allowed headers
- [ ] BP-033-20: Enable credentials support
- [ ] BP-033-21: Save __init__.py

#### Phase 4: Implement Input Validation

- [ ] BP-033-22: Create `app/utils/validators.py` file
- [ ] BP-033-23: Import re and bleach
- [ ] BP-033-24: Define validate_email() function with regex
- [ ] BP-033-25: Define sanitize_html() function using bleach
- [ ] BP-033-26: Define validate_url() function
- [ ] BP-033-27: Define validate_amount() function
- [ ] BP-033-28: Add error handling to all validators

#### Phase 5: Implement Authentication Middleware

- [ ] BP-033-29: Create `app/middleware/auth.py` file
- [ ] BP-033-30: Import jwt and functools
- [ ] BP-033-31: Define require_auth() decorator
- [ ] BP-033-32: Implement token extraction from Authorization header
- [ ] BP-033-33: Implement token validation with JWT
- [ ] BP-033-34: Add user loading from token
- [ ] BP-033-35: Add error handling for invalid/expired tokens

#### Phase 6: Implement Security Headers

- [ ] BP-033-36: Create `app/middleware/security_headers.py` file
- [ ] BP-033-37: Define SecurityHeaders middleware class
- [ ] BP-033-38: Add X-Content-Type-Options: nosniff
- [ ] BP-033-39: Add X-Frame-Options: DENY
- [ ] BP-033-40: Add X-XSS-Protection: 1; mode=block
- [ ] BP-033-41: Add Content-Security-Policy header
- [ ] BP-033-42: Add Strict-Transport-Security header
- [ ] BP-033-43: Open `app/__init__.py`
- [ ] BP-033-44: Import SecurityHeaders
- [ ] BP-033-45: Add SecurityHeaders middleware to app
- [ ] BP-033-46: Save __init__.py

#### Phase 7: Verify Tests Pass (TDD Green Phase)

- [ ] BP-033-47: Run security tests: `pytest tests/unit/test_security.py -v`
- [ ] BP-033-48: Verify all tests pass

#### Phase 8: Manual Testing

- [ ] BP-033-49: Start Flask app: `flask run`
- [ ] BP-033-50: Test rate limiting: Make 25 rapid requests to `/api/v1/bonuses`
- [ ] BP-033-51: Verify rate limit error after 20 requests
- [ ] BP-033-52: Test CORS: `curl -H "Origin: http://example.com" http://localhost:5000/api/v1/health`
- [ ] BP-033-53: Verify CORS headers in response
- [ ] BP-033-54: Test security headers: `curl -I http://localhost:5000/api/v1/health`
- [ ] BP-033-55: Verify X-Content-Type-Options, X-Frame-Options headers
- [ ] BP-033-56: Stop Flask app: Ctrl+C

#### Phase 9: Git Operations

- [ ] BP-033-57: Git add security files: `git add app/utils/validators.py app/middleware/auth.py app/middleware/security_headers.py`
- [ ] BP-033-58: Git add updated files: `git add app/__init__.py`
- [ ] BP-033-59: Git add test files: `git add tests/unit/test_security.py`
- [ ] BP-033-60: Git commit: `git commit -m "[BP-033] Implement security features with TDD"`
- [ ] BP-033-61: Git push to remote: `git push origin master`

---

### BP-034: Create Initial Seed Data and Documentation

**Type**: Documentation | **Priority**: Medium | **Story Points**: 3 | **TDD**: No (Documentation)

#### Phase 1: Create Sample Data

- [ ] BP-034-01: Create `database/sample_data.sql` file
- [ ] BP-034-02: Add INSERT statements for 5 sample banks
- [ ] BP-034-03: Add INSERT statements for 10 sample bonuses
- [ ] BP-034-04: Add INSERT statements for 3 sample articles
- [ ] BP-034-05: Add INSERT statements for 1 sample newsletter
- [ ] BP-034-06: Add INSERT statements for 2 sample users
- [ ] BP-034-07: Test sample data: `psql -U bankperks -d bankperks < database/sample_data.sql`
- [ ] BP-034-08: Verify data inserted: `psql -U bankperks -d bankperks -c "SELECT COUNT(*) FROM banks;"`

#### Phase 2: Create API Documentation

- [ ] BP-034-09: Create `docs/api_documentation.md` file
- [ ] BP-034-10: Add API overview section
- [ ] BP-034-11: Document authentication (if implemented)
- [ ] BP-034-12: Document rate limiting
- [ ] BP-034-13: Document pagination format
- [ ] BP-034-14: Document error response format
- [ ] BP-034-15: Document GET /api/v1/health endpoint
- [ ] BP-034-16: Document GET /api/v1/bonuses endpoint with all filters
- [ ] BP-034-17: Document GET /api/v1/bonuses/<id> endpoint
- [ ] BP-034-18: Document PATCH /api/v1/bonuses/<id> endpoint
- [ ] BP-034-19: Document GET /api/v1/banks endpoints
- [ ] BP-034-20: Document GET /api/v1/articles endpoints
- [ ] BP-034-21: Add example requests and responses for each endpoint

#### Phase 3: Create Deployment Documentation

- [ ] BP-034-22: Create `docs/deployment_guide.md` file
- [ ] BP-034-23: Add prerequisites section (Docker, Docker Compose)
- [ ] BP-034-24: Add environment setup instructions
- [ ] BP-034-25: Add database initialization instructions
- [ ] BP-034-26: Add Docker build instructions
- [ ] BP-034-27: Add Docker Compose deployment instructions
- [ ] BP-034-28: Add SSL certificate setup instructions
- [ ] BP-034-29: Add backup and restore procedures
- [ ] BP-034-30: Add troubleshooting section

#### Phase 4: Create Development Documentation

- [ ] BP-034-31: Create `docs/development_guide.md` file
- [ ] BP-034-32: Add local development setup instructions
- [ ] BP-034-33: Add virtual environment setup
- [ ] BP-034-34: Add database setup for development
- [ ] BP-034-35: Add running tests instructions
- [ ] BP-034-36: Add code style guidelines
- [ ] BP-034-37: Add Git workflow guidelines
- [ ] BP-034-38: Add debugging tips

#### Phase 5: Update Main README

- [ ] BP-034-39: Open `README.md`
- [ ] BP-034-40: Add project description and features
- [ ] BP-034-41: Add technology stack section
- [ ] BP-034-42: Add quick start guide
- [ ] BP-034-43: Add links to detailed documentation
- [ ] BP-034-44: Add API documentation link
- [ ] BP-034-45: Add deployment guide link
- [ ] BP-034-46: Add development guide link
- [ ] BP-034-47: Add contributing guidelines
- [ ] BP-034-48: Add license information
- [ ] BP-034-49: Save README.md

#### Phase 6: Create Architecture Diagram

- [ ] BP-034-50: Create `docs/architecture_diagram.md` file
- [ ] BP-034-51: Add system architecture overview
- [ ] BP-034-52: Add component diagram (Flask, Celery, Scrapy, PostgreSQL, Redis)
- [ ] BP-034-53: Add data flow diagram
- [ ] BP-034-54: Add deployment architecture diagram

#### Phase 7: Create Changelog

- [ ] BP-034-55: Create `CHANGELOG.md` file
- [ ] BP-034-56: Add version 1.0.0 section
- [ ] BP-034-57: List all implemented features
- [ ] BP-034-58: List all API endpoints
- [ ] BP-034-59: List all background tasks
- [ ] BP-034-60: Add release date

#### Phase 8: Git Operations

- [ ] BP-034-61: Git add documentation: `git add docs/ README.md CHANGELOG.md`
- [ ] BP-034-62: Git add sample data: `git add database/sample_data.sql`
- [ ] BP-034-63: Git commit: `git commit -m "[BP-034] Create initial seed data and documentation"`
- [ ] BP-034-64: Git push to remote: `git push origin master`

---



## Checklist Usage Instructions

### For AI Coding Agents:

1. **Work Sequentially**: Complete tickets in order (BP-001 → BP-034)
2. **Check Off Tasks**: Mark each sub-task with `[x]` when complete
3. **Follow TDD**: For feature tickets, always write tests first
4. **Verify Each Step**: Run verification commands and check expected outputs
5. **Git After Each Ticket**: Always add, commit, and push after completing a ticket

### TDD Workflow Summary:

```
For Feature Tickets:
├── Phase 1: Write Tests (Red)
│   ├── Create test file
│   ├── Write test cases
│   └── Run tests (should fail)
├── Phase 2: Implement (Green)
│   ├── Create implementation file
│   ├── Write code to pass tests
│   └── Add error handling
├── Phase 3: Verify (Green)
│   ├── Run tests (should pass)
│   └── Check coverage
├── Phase 4: Manual Test
│   ├── Start services
│   ├── Test with curl/commands
│   └── Verify outputs
└── Phase 5: Git
    ├── Add files
    ├── Commit with message
    └── Push to remote
```

---

## Progress Tracking

### ✅ All Tickets Complete: 34 tickets, 1,200+ granular tasks

**Sprint 0: Project Setup** (6 tickets, 115 tasks)
- ✅ BP-001: Initialize Project Structure (20 tasks)
- ✅ BP-002: Install Python Dependencies (15 tasks)
- ✅ BP-003: Setup PostgreSQL Database (14 tasks)
- ✅ BP-004: Setup Redis (12 tasks)
- ✅ BP-005: Create Configuration Files (19 tasks)
- ✅ BP-006: Initialize Flask Application (21 tasks)

**Sprint 1: Database & API** (6 tickets, 235 tasks)
- ✅ BP-007: Create Database Schema (28 tasks)
- ✅ BP-008: Implement SQLAlchemy Models (60 tasks - TDD)
- ✅ BP-009: Create Seed Data (30 tasks - TDD)
- ✅ BP-010: Implement Base API Structure (24 tasks - TDD)
- ✅ BP-011: Implement Bonus API Endpoints (50 tasks - TDD)
- ✅ BP-012: Implement Bank/Article API (52 tasks - TDD)

**Sprint 2: Web Scraping** (5 tickets, 185 tasks)
- ✅ BP-013: Setup Scrapy Project (20 tasks)
- ✅ BP-014: Create Base Spider Class (28 tasks - TDD)
- ✅ BP-015: Implement Doctor of Credit Spider (34 tasks - TDD)
- ✅ BP-016: Implement Data Extraction Pipeline (44 tasks - TDD)
- ✅ BP-017: Implement Data Cleaning Pipeline (32 tasks - TDD)
- ✅ BP-018: Implement Scrapy Pipeline (38 tasks - TDD)

**Sprint 3: Celery Tasks** (3 tickets, 87 tasks)
- ✅ BP-019: Setup Celery (20 tasks)
- ✅ BP-020: Implement Scraping Tasks (34 tasks - TDD)
- ✅ BP-021: Implement Maintenance Tasks (33 tasks - TDD)

**Sprint 4: AI Content** (4 tickets, 139 tasks)
- ✅ BP-022: Implement LLM Client (32 tasks - TDD)
- ✅ BP-023: Create Prompt Templates (33 tasks - TDD)
- ✅ BP-024: Implement Content Generator (40 tasks - TDD)
- ✅ BP-025: Implement AI Generation Tasks (40 tasks - TDD)

**Sprint 5: Testing** (3 tickets, 129 tasks)
- ✅ BP-026: Write Unit Tests for Models (48 tasks)
- ✅ BP-027: Write Integration Tests for API (47 tasks)
- ✅ BP-028: Write Tests for Scraping Pipeline (41 tasks)

**Sprint 6: Deployment** (6 tickets, 310 tasks)
- ✅ BP-029: Create Docker Configuration (47 tasks)
- ✅ BP-030: Configure Nginx Reverse Proxy (37 tasks)
- ✅ BP-031: Create Deployment Scripts (49 tasks)
- ✅ BP-032: Setup Monitoring and Logging (65 tasks)
- ✅ BP-033: Implement Security Features (61 tasks - TDD)
- ✅ BP-034: Create Initial Seed Data and Documentation (64 tasks)

---

## Summary Statistics

| Metric | Value |
|--------|-------|
| **Total Tickets** | 34 |
| **Total Tasks** | 1,200+ |
| **Average Tasks per Ticket** | 35 |
| **TDD Tickets** | 18 (53%) |
| **Test Tickets** | 3 (9%) |
| **Infrastructure Tickets** | 13 (38%) |
| **Estimated Time per Task** | 2-5 minutes |
| **Total Estimated Time** | 40-100 hours |

---

## Key Principles

1. **Atomic Tasks**: Each task takes 2-5 minutes
2. **TDD First**: Write tests before implementation for features
3. **Verify Everything**: Run commands and check outputs
4. **Git Discipline**: Commit after each ticket with descriptive messages
5. **Test Coverage**: Maintain >90% coverage for models, >80% for other code

---

## Example Task Completion

```markdown
Before:
- [ ] BP-008-14: Create `app/models/__init__.py` with TimestampMixin and UUIDMixin

After:
- [x] BP-008-14: Create `app/models/__init__.py` with TimestampMixin and UUIDMixin
```

---

## Reference Documents

- **Tickets**: `docs/improved_enhanced_tickets.md` - Complete implementation details
- **Evaluation**: `docs/ticket_evaluation.md` - Quality assessment
- **Plan**: `docs/enhanced_plan.md` - Architecture and specifications
- **Summary**: `docs/improvements_summary.md` - Changes made

---

## Completion Checklist

Use this high-level checklist to track sprint completion:

- [ ] **Sprint 0 Complete**: All 6 setup tickets done, environment ready
- [ ] **Sprint 1 Complete**: All 6 database/API tickets done, API functional
- [ ] **Sprint 2 Complete**: All 5 scraping tickets done, spiders working
- [ ] **Sprint 3 Complete**: All 3 Celery tickets done, background tasks running
- [ ] **Sprint 4 Complete**: All 4 AI tickets done, content generation working
- [ ] **Sprint 5 Complete**: All 3 test tickets done, >85% coverage achieved
- [ ] **Sprint 6 Complete**: All 6 deployment tickets done, production ready

---

**This checklist provides a complete, TDD-focused, granular task breakdown for all 34 tickets of the Bankperks implementation. Each task is atomic, verifiable, and designed for autonomous AI execution.**

**Total Implementation Time**: 40-100 hours (1,200+ tasks × 2-5 minutes each)

---


