# Bankperks Development Tickets

> **Purpose**: This document contains detailed software development tickets for implementing the Bankperks system. Each ticket is designed to be independently actionable by an AI coding agent and follows the implementation sequence defined in `docs/enhanced_plan.md`.

---

## Table of Contents

- [Sprint 0: Project Setup](#sprint-0-project-setup) (BP-001 to BP-006)
- [Sprint 1: Database Models and API Foundation](#sprint-1-database-models-and-api-foundation) (BP-007 to BP-012)
- [Sprint 2: Web Scraping Infrastructure](#sprint-2-web-scraping-infrastructure) (BP-013 to BP-018)
- [Sprint 3: Celery Task Queue](#sprint-3-celery-task-queue) (BP-019 to BP-021)
- [Sprint 4: AI Content Generation](#sprint-4-ai-content-generation) (BP-022 to BP-025)
- [Sprint 5: Testing and QA](#sprint-5-testing-and-qa) (BP-026 to BP-028)
- [Sprint 6: Deployment and Operations](#sprint-6-deployment-and-operations) (BP-029 to BP-034)

**Total Tickets**: 34  
**Total Story Points**: 142

---

## Sprint 0: Project Setup

### BP-001: Initialize Project Structure

**Type**: Setup  
**Priority**: Critical  
**Story Points**: 2  
**Sprint**: Sprint 0  
**Dependencies**: None

**Description**:
Create the complete directory structure for the Bankperks project as specified in Section 10.1 of the enhanced plan. This includes all folders for app modules, tests, database scripts, Docker configuration, and documentation.

**Acceptance Criteria**:
- [ ] All directories from Section 10.1 exist in the project root
- [ ] Git repository is initialized with `.gitignore` file
- [ ] `README.md` file exists with project overview
- [ ] Virtual environment is created at `venv/`
- [ ] Virtual environment is activated
- [ ] Directory structure matches the specification exactly

**Implementation Notes**:
- Reference: Section 10.1 of `docs/enhanced_plan.md`
- Create main directories:
  ```bash
  mkdir -p app/{api/v1,models,scraping/scrapy_project/spiders,scraping/extractors,scraping/cleaners}
  mkdir -p app/{ai/prompts,forum/avatars,tasks,services,utils,templates}
  mkdir -p tests/{unit,integration,fixtures/sample_html}
  mkdir -p database/migrations/versions
  mkdir -p docker scripts docs logs uploads
  ```
- Initialize git: `git init`
- Create `.gitignore` with Python, Flask, IDE entries
- Create venv: `python -m venv venv`
- Activate: `source venv/bin/activate` (macOS/Linux) or `venv\Scripts\activate` (Windows)
- Verify: `tree -L 2` or `ls -la` should show all directories

---

### BP-002: Install Python Dependencies

**Type**: Setup  
**Priority**: Critical  
**Story Points**: 2  
**Sprint**: Sprint 0  
**Dependencies**: BP-001

**Description**:
Install all Python dependencies required for the Bankperks project, including Flask, SQLAlchemy, Scrapy, Celery, AI libraries, and development tools. Create requirements.txt and requirements-dev.txt files.

**Acceptance Criteria**:
- [ ] `requirements.txt` file created with all production dependencies
- [ ] `requirements-dev.txt` file created with development dependencies
- [ ] All packages installed successfully without errors
- [ ] spaCy English model downloaded
- [ ] Playwright Chromium browser installed
- [ ] `pip list` shows all required packages

**Implementation Notes**:
- Reference: Section 10.3 of `docs/enhanced_plan.md`
- Create `requirements.txt` with exact content from Section 10.3.1
- Create `requirements-dev.txt` with exact content from Section 10.3.2
- Install: `pip install -r requirements-dev.txt`
- Download spaCy model: `python -m spacy download en_core_web_sm`
- Install Playwright: `playwright install chromium`
- Verify installations:
  ```bash
  pip list | grep Flask
  pip list | grep Scrapy
  pip list | grep celery
  python -c "import spacy; nlp = spacy.load('en_core_web_sm'); print('spaCy OK')"
  playwright --version
  ```

---

### BP-003: Setup PostgreSQL Database

**Type**: Setup  
**Priority**: Critical  
**Story Points**: 2  
**Sprint**: Sprint 0  
**Dependencies**: None

**Description**:
Install PostgreSQL 16, create the bankperks database and user, and verify connectivity. This can be done in parallel with BP-001 and BP-002.

**Acceptance Criteria**:
- [ ] PostgreSQL 16 is installed and running
- [ ] Database `bankperks` is created
- [ ] User `bankperks` is created with password
- [ ] User has all privileges on the database
- [ ] Can connect to database using psql
- [ ] UUID extension is available

**Implementation Notes**:
- Install PostgreSQL 16 (method depends on OS)
- Create database and user:
  ```bash
  # Start PostgreSQL service
  # macOS: brew services start postgresql@16
  # Linux: sudo systemctl start postgresql
  
  # Create database and user
  createdb bankperks
  createuser -P bankperks  # Enter password: password
  
  # Grant privileges
  psql -d postgres -c "GRANT ALL PRIVILEGES ON DATABASE bankperks TO bankperks;"
  
  # Test connection
  psql -U bankperks -d bankperks -c "SELECT version();"
  psql -U bankperks -d bankperks -c "CREATE EXTENSION IF NOT EXISTS \"uuid-ossp\";"
  ```
- Verify: Connection should succeed and show PostgreSQL version

---

### BP-004: Setup Redis

**Type**: Setup  
**Priority**: Critical  
**Story Points**: 1  
**Sprint**: Sprint 0  
**Dependencies**: None

**Description**:
Install Redis server and verify it's running correctly. Redis will be used as the message broker for Celery and for caching.

**Acceptance Criteria**:
- [ ] Redis is installed
- [ ] Redis server is running on port 6379
- [ ] Can connect to Redis using redis-cli
- [ ] PING command returns PONG
- [ ] Redis is configured to start on system boot (optional)

**Implementation Notes**:
- Install Redis (method depends on OS):
  ```bash
  # macOS: brew install redis
  # Ubuntu: sudo apt-get install redis-server
  # Or use Docker: docker run -d -p 6379:6379 redis:7-alpine
  ```
- Start Redis:
  ```bash
  # macOS: brew services start redis
  # Linux: sudo systemctl start redis
  # Docker: already running
  ```
- Test connection:
  ```bash
  redis-cli ping  # Should return PONG
  redis-cli set test "hello"
  redis-cli get test  # Should return "hello"
  redis-cli del test
  ```
- Verify: `redis-cli INFO server` should show Redis version

---

### BP-005: Create Configuration Files

**Type**: Setup  
**Priority**: Critical  
**Story Points**: 3  
**Sprint**: Sprint 0  
**Dependencies**: BP-001

**Description**:
Create all configuration files including app/config.py, .env.example, .env, pytest.ini, and .flake8. These files define application settings, environment variables, and development tool configurations.

**Acceptance Criteria**:
- [ ] `app/config.py` exists with Config classes
- [ ] `.env.example` exists with all required variables
- [ ] `.env` exists with actual values (not committed to git)
- [ ] `pytest.ini` exists with test configuration
- [ ] `.flake8` exists with linting rules
- [ ] Configuration can be imported without errors
- [ ] `.env` is in `.gitignore`

**Implementation Notes**:
- Reference: Section 10.2 of `docs/enhanced_plan.md`
- Create `app/config.py` with exact content from Section 10.2.1
- Create `.env.example` with exact content from Section 10.2.2
- Copy `.env.example` to `.env`:
  ```bash
  cp .env.example .env
  ```
- Edit `.env` and fill in actual values:
  - SECRET_KEY: Generate with `python -c "import secrets; print(secrets.token_hex(32))"`
  - DATABASE_URL: `postgresql://bankperks:password@localhost:5432/bankperks`
  - REDIS_URL: `redis://localhost:6379/0`
  - Add API keys if available (OPENAI_API_KEY, etc.)
- Create `pytest.ini` with content from Section 12.1.1
- Create `.flake8`:
  ```ini
  [flake8]
  max-line-length = 120
  exclude = venv,.git,__pycache__,migrations
  ignore = E203,W503
  ```
- Verify: `python -c "from app.config import config; print(config['development'])"` should work

---

### BP-006: Initialize Flask Application

**Type**: Setup  
**Priority**: Critical  
**Story Points**: 3  
**Sprint**: Sprint 0  
**Dependencies**: BP-001, BP-002, BP-005

**Description**:
Create the Flask application factory, extensions initialization, and entry point (run.py). This establishes the core Flask application structure.

**Acceptance Criteria**:
- [ ] `app/__init__.py` exists with create_app function
- [ ] `app/extensions.py` exists with extension instances
- [ ] `run.py` exists with application entry point
- [ ] Flask application starts without errors
- [ ] Can access http://localhost:5000 (even if 404)
- [ ] Flask shell context works
- [ ] CLI commands are registered

**Implementation Notes**:
- Reference: Section 10.4 and 10.5 of `docs/enhanced_plan.md`
- Create `app/__init__.py` with exact content from Section 10.5
- Create `app/extensions.py` with exact content from Section 10.5
- Create `run.py` with exact content from Section 10.4
- Create empty `app/api/__init__.py` and `app/api/errors.py` (will be filled later)
- Test Flask application:
  ```bash
  export FLASK_APP=run.py
  export FLASK_ENV=development
  flask run
  ```
- In another terminal, test:
  ```bash
  curl http://localhost:5000/
  # Should return 404 (expected, no routes yet)
  ```
- Test Flask shell:
  ```bash
  flask shell
  >>> from app.extensions import db
  >>> print(db)
  >>> exit()
  ```
- Verify: Application starts and Flask shell works

---

## Sprint 1: Database Models and API Foundation

### BP-007: Create Database Schema

**Type**: Feature  
**Priority**: Critical  
**Story Points**: 5  
**Sprint**: Sprint 1  
**Dependencies**: BP-003, BP-006

**Description**:
Implement the complete PostgreSQL database schema including all tables, indexes, constraints, triggers, and views as specified in Section 9.1 of the enhanced plan.

**Acceptance Criteria**:
- [ ] `database/init_db.sql` file created with complete schema
- [ ] All 12+ tables created successfully
- [ ] All indexes created
- [ ] All foreign key constraints created
- [ ] Triggers for updated_at timestamps working
- [ ] Views (active_bonuses, published_articles) created
- [ ] Can query all tables without errors
- [ ] UUID extension enabled

**Implementation Notes**:
- Reference: Section 9.1 of `docs/enhanced_plan.md`
- Create `database/init_db.sql` with exact SQL from Section 9.1.1
- Run the schema:
  ```bash
  psql -U bankperks -d bankperks < database/init_db.sql
  ```
- Verify tables:
  ```bash
  psql -U bankperks -d bankperks -c "\dt"
  # Should show: banks, account_types, bonuses, scraping_sources, articles, 
  # article_versions, newsletters, users, user_saved_bonuses, celery_tasks, requirement_types
  ```
- Verify indexes:
  ```bash
  psql -U bankperks -d bankperks -c "\di"
  ```
- Test trigger:
  ```bash
  psql -U bankperks -d bankperks -c "INSERT INTO banks (name, slug) VALUES ('Test Bank', 'test-bank');"
  psql -U bankperks -d bankperks -c "UPDATE banks SET name='Test Bank Updated' WHERE slug='test-bank';"
  psql -U bankperks -d bankperks -c "SELECT name, created_at, updated_at FROM banks WHERE slug='test-bank';"
  # updated_at should be different from created_at
  ```
- Test views:
  ```bash
  psql -U bankperks -d bankperks -c "SELECT * FROM active_bonuses LIMIT 1;"
  ```

---

### BP-008: Implement SQLAlchemy Models

**Type**: Feature  
**Priority**: Critical  
**Story Points**: 5  
**Sprint**: Sprint 1  
**Dependencies**: BP-007

**Description**:
Create all SQLAlchemy model classes that map to the database schema, including Bank, AccountType, Bonus, Article, Newsletter, User, and ScrapingSource models with relationships and methods.

**Acceptance Criteria**:
- [ ] `app/models/__init__.py` exists with base mixins
- [ ] `app/models/bank.py` exists with Bank model
- [ ] `app/models/account_type.py` exists with AccountType model
- [ ] `app/models/bonus.py` exists with Bonus model
- [ ] `app/models/article.py` exists with Article model
- [ ] All models have to_dict() methods
- [ ] All relationships defined correctly
- [ ] Can import and instantiate models
- [ ] Flask-Migrate initialized

**Implementation Notes**:
- Reference: Section 9.2 of `docs/enhanced_plan.md`
- Create `app/models/__init__.py` with content from Section 9.2.1
- Create `app/models/bank.py` with content from Section 9.2.2
- Create `app/models/account_type.py` with content from Section 9.2.2
- Create `app/models/bonus.py` with content from Section 9.2.2
- Create `app/models/article.py` with content from Section 9.2.2
- Create remaining models (newsletter.py, user.py, scraping_source.py) following same pattern
- Initialize Flask-Migrate:
  ```bash
  flask db init
  flask db migrate -m "Initial schema"
  flask db upgrade
  ```
- Test models in Flask shell:
  ```bash
  flask shell
  >>> from app.models.bank import Bank
  >>> bank = Bank(name='Chase', slug='chase', website_url='https://chase.com')
  >>> print(bank)
  >>> print(bank.to_dict())
  >>> exit()
  ```

---

### BP-009: Create Seed Data

**Type**: Feature
**Priority**: High
**Story Points**: 2
**Sprint**: Sprint 1
**Dependencies**: BP-008

**Description**:
Create seed data script to populate initial account types and requirement types in the database. This provides the foundational reference data needed for the application.

**Acceptance Criteria**:
- [ ] `database/seed_data.py` file created
- [ ] `seed_account_types()` function implemented
- [ ] `seed_requirement_types()` function implemented
- [ ] Flask CLI command `flask seed-db` works
- [ ] 6 account types inserted
- [ ] 10 requirement types inserted
- [ ] Data persists in database
- [ ] Running seed command multiple times doesn't create duplicates

**Implementation Notes**:
- Reference: Section 11.2 Task 2.3 of `docs/enhanced_plan.md`
- Create `database/seed_data.py`:
  ```python
  from app.extensions import db
  from app.models.account_type import AccountType
  from app.models.requirement_type import RequirementType

  def seed_account_types():
      account_types = [
          {'name': 'Checking', 'slug': 'checking', 'description': 'Standard checking account'},
          {'name': 'Savings', 'slug': 'savings', 'description': 'Standard savings account'},
          {'name': 'Money Market', 'slug': 'money-market', 'description': 'Money market account'},
          {'name': 'Certificate of Deposit', 'slug': 'cd', 'description': 'Certificate of deposit account'},
          {'name': 'Business Checking', 'slug': 'business-checking', 'description': 'Business checking account'},
          {'name': 'Business Savings', 'slug': 'business-savings', 'description': 'Business savings account'}
      ]
      for at_data in account_types:
          existing = AccountType.query.filter_by(slug=at_data['slug']).first()
          if not existing:
              at = AccountType(**at_data)
              db.session.add(at)
      db.session.commit()
      print('Account types seeded')

  def seed_requirement_types():
      # Similar implementation for requirement types
      pass
  ```
- Update `run.py` to include seed-db command (already in Section 10.4)
- Run seed command:
  ```bash
  flask seed-db
  ```
- Verify:
  ```bash
  psql -U bankperks -d bankperks -c "SELECT * FROM account_types;"
  psql -U bankperks -d bankperks -c "SELECT * FROM requirement_types;"
  ```

---

### BP-010: Implement Base API Structure

**Type**: Feature
**Priority**: Critical
**Story Points**: 3
**Sprint**: Sprint 1
**Dependencies**: BP-008

**Description**:
Create the base API blueprint structure, error handlers, and health check endpoint. This establishes the foundation for all API endpoints.

**Acceptance Criteria**:
- [ ] `app/api/v1/__init__.py` exists with blueprint
- [ ] `app/api/errors.py` exists with error handlers
- [ ] `app/api/v1/health.py` exists with health endpoint
- [ ] Health endpoint returns 200 status
- [ ] Health endpoint returns JSON with status, timestamp, version
- [ ] Error handlers registered (404, 400, 500)
- [ ] Can access `/api/v1/health` successfully

**Implementation Notes**:
- Reference: Section 11.2 Task 2.4 and Section 13.1 of `docs/enhanced_plan.md`
- Create `app/api/v1/__init__.py`:
  ```python
  from flask import Blueprint

  api_v1_bp = Blueprint('api_v1', __name__)

  from app.api.v1 import health
  ```
- Create `app/api/errors.py` with content from Section 13.1
- Create `app/api/v1/health.py`:
  ```python
  from flask import jsonify
  from datetime import datetime
  from app.api.v1 import api_v1_bp

  @api_v1_bp.route('/health', methods=['GET'])
  def health_check():
      return jsonify({
          'status': 'healthy',
          'timestamp': datetime.utcnow().isoformat(),
          'version': '1.0.0'
      }), 200
  ```
- Update `app/__init__.py` to register blueprint and error handlers (already in Section 10.5)
- Test:
  ```bash
  flask run
  # In another terminal:
  curl http://localhost:5000/api/v1/health
  # Should return: {"status":"healthy","timestamp":"...","version":"1.0.0"}
  ```

---

### BP-011: Implement Bonus API Endpoints

**Type**: Feature
**Priority**: Critical
**Story Points**: 5
**Sprint**: Sprint 1
**Dependencies**: BP-010

**Description**:
Create complete Bonus CRUD API endpoints including list with pagination/filtering/sorting and get by ID. Implement pagination helper utilities.

**Acceptance Criteria**:
- [ ] `app/api/v1/bonuses.py` exists with all endpoints
- [ ] `app/utils/pagination.py` exists with helper functions
- [ ] GET /api/v1/bonuses returns paginated list
- [ ] Pagination works (page, per_page parameters)
- [ ] Filtering works (status, bank_id, min_amount, max_amount)
- [ ] Sorting works (sort parameter with +/- prefix)
- [ ] GET /api/v1/bonuses/{id} returns single bonus
- [ ] GET /api/v1/bonuses/{uuid} works with UUID
- [ ] 404 returned for non-existent bonus
- [ ] Response format matches OpenAPI schema

**Implementation Notes**:
- Reference: Section 11.2 Task 2.5 and Section 9.3 of `docs/enhanced_plan.md`
- Create `app/utils/pagination.py`:
  ```python
  from flask import request, url_for

  def paginate(query, schema):
      page = request.args.get('page', 1, type=int)
      per_page = min(request.args.get('per_page', 20, type=int), 100)

      page_obj = query.paginate(page=page, per_page=per_page, error_out=False)

      return {
          'data': [item.to_dict() for item in page_obj.items],
          'meta': {
              'total': page_obj.total,
              'page': page,
              'per_page': per_page,
              'total_pages': page_obj.pages
          }
      }
  ```
- Create `app/api/v1/bonuses.py` with endpoints:
  - `GET /bonuses` - list with filters
  - `GET /bonuses/<id>` - get single
  - `PATCH /bonuses/<id>` - update (admin only, can skip auth for now)
- Implement filtering logic for status, bank_id, account_type_id, min_amount, max_amount
- Implement sorting with SQLAlchemy order_by
- Register blueprint in `app/api/v1/__init__.py`
- Test with curl:
  ```bash
  curl http://localhost:5000/api/v1/bonuses
  curl http://localhost:5000/api/v1/bonuses?page=1&per_page=10
  curl http://localhost:5000/api/v1/bonuses?status=active
  curl http://localhost:5000/api/v1/bonuses?sort=-amount
  ```

---

### BP-012: Implement Bank and Article API Endpoints

**Type**: Feature
**Priority**: High
**Story Points**: 3
**Sprint**: Sprint 1
**Dependencies**: BP-011

**Description**:
Create Bank and Article API endpoints including list, get by ID, and get bonuses for a specific bank.

**Acceptance Criteria**:
- [ ] `app/api/v1/banks.py` exists with endpoints
- [ ] `app/api/v1/articles.py` exists with endpoints
- [ ] GET /api/v1/banks returns paginated list
- [ ] GET /api/v1/banks/{id} returns single bank
- [ ] GET /api/v1/banks/{id}/bonuses returns bank's bonuses
- [ ] GET /api/v1/articles returns paginated list
- [ ] GET /api/v1/articles/{slug} returns single article
- [ ] All responses match OpenAPI schema
- [ ] Related data properly serialized

**Implementation Notes**:
- Reference: Section 11.2 Task 2.6 and Section 9.3 of `docs/enhanced_plan.md`
- Create `app/api/v1/banks.py`:
  ```python
  from flask import jsonify
  from app.api.v1 import api_v1_bp
  from app.models.bank import Bank
  from app.models.bonus import Bonus
  from app.utils.pagination import paginate

  @api_v1_bp.route('/banks', methods=['GET'])
  def list_banks():
      query = Bank.query
      return jsonify(paginate(query, None))

  @api_v1_bp.route('/banks/<int:id>', methods=['GET'])
  def get_bank(id):
      bank = Bank.query.get_or_404(id)
      return jsonify({'data': bank.to_dict()})

  @api_v1_bp.route('/banks/<int:id>/bonuses', methods=['GET'])
  def get_bank_bonuses(id):
      bank = Bank.query.get_or_404(id)
      query = Bonus.query.filter_by(bank_id=id)
      return jsonify(paginate(query, None))
  ```
- Create `app/api/v1/articles.py` with similar structure
- Register blueprints in `app/api/v1/__init__.py`
- Test:
  ```bash
  curl http://localhost:5000/api/v1/banks
  curl http://localhost:5000/api/v1/articles
  ```

---

## Sprint 2: Web Scraping Infrastructure

### BP-013: Setup Scrapy Project

**Type**: Feature
**Priority**: High
**Story Points**: 2
**Sprint**: Sprint 2
**Dependencies**: BP-002

**Description**:
Initialize Scrapy project structure within the app/scraping directory and configure settings for user agent, download delays, and Playwright integration.

**Acceptance Criteria**:
- [ ] Scrapy project initialized in `app/scraping/scrapy_project/`
- [ ] `scrapy.cfg` file exists
- [ ] `settings.py` configured with proper values
- [ ] USER_AGENT set from config
- [ ] DOWNLOAD_DELAY = 2.0
- [ ] CONCURRENT_REQUESTS = 8
- [ ] ROBOTSTXT_OBEY = True
- [ ] Playwright middleware enabled
- [ ] `scrapy list` command works

**Implementation Notes**:
- Reference: Section 11.3 Task 3.1 of `docs/enhanced_plan.md`
- Navigate to scraping directory:
  ```bash
  cd app/scraping
  scrapy startproject scrapy_project
  cd ../..
  ```
- Edit `app/scraping/scrapy_project/settings.py`:
  ```python
  BOT_NAME = 'bankperks'
  SPIDER_MODULES = ['app.scraping.scrapy_project.spiders']
  NEWSPIDER_MODULE = 'app.scraping.scrapy_project.spiders'

  USER_AGENT = 'BankperksBot/1.0 (+https://bankperks.net/botinfo)'
  ROBOTSTXT_OBEY = True
  CONCURRENT_REQUESTS = 8
  DOWNLOAD_DELAY = 2.0
  CONCURRENT_REQUESTS_PER_DOMAIN = 2

  # Enable Playwright
  DOWNLOAD_HANDLERS = {
      "http": "scrapy_playwright.handler.ScrapyPlaywrightDownloadHandler",
      "https": "scrapy_playwright.handler.ScrapyPlaywrightDownloadHandler",
  }
  TWISTED_REACTOR = "twisted.internet.asyncioreactor.AsyncioSelectorReactor"
  ```
- Test:
  ```bash
  cd app/scraping/scrapy_project
  scrapy list  # Should show empty list (no spiders yet)
  cd ../../..
  ```

---

### BP-014: Create Base Spider Class

**Type**: Feature
**Priority**: High
**Story Points**: 3
**Sprint**: Sprint 2
**Dependencies**: BP-013

**Description**:
Implement reusable base spider class with common functionality including retry logic, user-agent rotation, error handling, and logging.

**Acceptance Criteria**:
- [ ] `app/scraping/scrapy_project/spiders/base_spider.py` exists
- [ ] BaseSpider class with retry logic
- [ ] Error handling for common issues
- [ ] Logging configured
- [ ] Helper methods for parsing
- [ ] Can be imported and subclassed
- [ ] Includes rate limiting respect

**Implementation Notes**:
- Reference: Section 11.3 Task 3.2 of `docs/enhanced_plan.md`
- Create `app/scraping/scrapy_project/spiders/base_spider.py`:
  ```python
  import scrapy
  import logging
  from scrapy.exceptions import IgnoreRequest

  class BaseSpider(scrapy.Spider):
      """Base spider with common functionality"""

      custom_settings = {
          'RETRY_TIMES': 3,
          'RETRY_HTTP_CODES': [500, 502, 503, 504, 408, 429],
      }

      def __init__(self, *args, **kwargs):
          super().__init__(*args, **kwargs)
          self.logger = logging.getLogger(self.name)

      def handle_error(self, failure):
          """Handle request errors"""
          self.logger.error(f'Request failed: {failure}')

      def parse_amount(self, text):
          """Extract dollar amount from text"""
          import re
          match = re.search(r'\$?([\d,]+(?:\.\d{2})?)', text)
          if match:
              return float(match.group(1).replace(',', ''))
          return None

      def parse_date(self, text):
          """Parse date from various formats"""
          from dateutil import parser
          try:
              return parser.parse(text).date()
          except:
              return None
  ```
- Test by importing:
  ```bash
  python -c "from app.scraping.scrapy_project.spiders.base_spider import BaseSpider; print('OK')"
  ```

---

### BP-015: Implement Doctor of Credit Spider

**Type**: Feature
**Priority**: High
**Story Points**: 5
**Sprint**: Sprint 2
**Dependencies**: BP-014

**Description**:
Create spider for scraping doctorofcredit.com bank bonus page. Implement parsing logic for bonus amount, bank name, requirements, and dates.

**Acceptance Criteria**:
- [ ] `app/scraping/scrapy_project/spiders/doctorofcredit_spider.py` exists
- [ ] Spider extends BaseSpider
- [ ] Parses bonus amounts correctly
- [ ] Extracts bank names
- [ ] Extracts requirements
- [ ] Extracts dates (start, end)
- [ ] Handles pagination
- [ ] Respects robots.txt
- [ ] Can run spider successfully

**Implementation Notes**:
- Reference: Section 11.3 Task 3.3 of `docs/enhanced_plan.md`
- Create `app/scraping/scrapy_project/spiders/doctorofcredit_spider.py`:
  ```python
  from app.scraping.scrapy_project.spiders.base_spider import BaseSpider
  import scrapy

  class DoctorOfCreditSpider(BaseSpider):
      name = 'doctorofcredit'
      allowed_domains = ['doctorofcredit.com']
      start_urls = ['https://www.doctorofcredit.com/best-bank-account-bonuses/']

      def parse(self, response):
          """Parse main bonus listing page"""
          # Extract bonus entries
          for bonus_section in response.css('div.bonus-entry'):
              yield {
                  'bank_name': bonus_section.css('h3::text').get(),
                  'amount': self.parse_amount(bonus_section.css('.amount::text').get()),
                  'description': bonus_section.css('.description::text').get(),
                  'url': bonus_section.css('a::attr(href)').get(),
                  'source_url': response.url,
                  'source_name': 'Doctor of Credit'
              }

          # Handle pagination
          next_page = response.css('a.next::attr(href)').get()
          if next_page:
              yield response.follow(next_page, self.parse)
  ```
- Test spider:
  ```bash
  cd app/scraping/scrapy_project
  scrapy crawl doctorofcredit -o output.json
  cat output.json
  cd ../../..
  ```

---

### BP-016: Implement Data Extraction Pipeline

**Type**: Feature
**Priority**: High
**Story Points**: 5
**Sprint**: Sprint 2
**Dependencies**: BP-015

**Description**:
Create NER and rule-based extractors for identifying entities (amounts, dates, organizations) and patterns (requirements, account types) from scraped HTML.

**Acceptance Criteria**:
- [ ] `app/scraping/extractors/base_extractor.py` exists
- [ ] `app/scraping/extractors/ner_extractor.py` exists with spaCy integration
- [ ] `app/scraping/extractors/rule_extractor.py` exists with pattern matching
- [ ] Can extract monetary amounts
- [ ] Can extract dates
- [ ] Can extract organization names
- [ ] Can identify requirements
- [ ] Can detect account types
- [ ] Accuracy >80% on test data

**Implementation Notes**:
- Reference: Section 11.3 Task 3.4 of `docs/enhanced_plan.md`
- Create `app/scraping/extractors/ner_extractor.py`:
  ```python
  import spacy
  from datetime import datetime

  class NERExtractor:
      def __init__(self):
          self.nlp = spacy.load('en_core_web_sm')

      def extract_money(self, text):
          """Extract monetary amounts"""
          doc = self.nlp(text)
          amounts = []
          for ent in doc.ents:
              if ent.label_ == 'MONEY':
                  # Parse amount
                  amount_str = ent.text.replace('$', '').replace(',', '')
                  try:
                      amounts.append(float(amount_str))
                  except:
                      pass
          return amounts

      def extract_dates(self, text):
          """Extract dates"""
          doc = self.nlp(text)
          dates = []
          for ent in doc.ents:
              if ent.label_ == 'DATE':
                  dates.append(ent.text)
          return dates

      def extract_organizations(self, text):
          """Extract organization names (banks)"""
          doc = self.nlp(text)
          orgs = []
          for ent in doc.ents:
              if ent.label_ == 'ORG':
                  orgs.append(ent.text)
          return orgs
  ```
- Create `app/scraping/extractors/rule_extractor.py` with pattern matching
- Test extractors:
  ```python
  from app.scraping.extractors.ner_extractor import NERExtractor
  extractor = NERExtractor()
  text = "Earn $300 bonus when you open a Chase checking account"
  print(extractor.extract_money(text))  # Should print [300.0]
  print(extractor.extract_organizations(text))  # Should print ['Chase']
  ```

---

### BP-017: Implement Data Cleaning Pipeline

**Type**: Feature
**Priority**: High
**Story Points**: 3
**Sprint**: Sprint 2
**Dependencies**: BP-016

**Description**:
Create data cleaning and validation functions to normalize amounts, parse dates, standardize bank names, and validate data completeness.

**Acceptance Criteria**:
- [ ] `app/scraping/cleaners/bonus_cleaner.py` exists
- [ ] `app/scraping/cleaners/validators.py` exists
- [ ] Can normalize monetary amounts (remove $, commas)
- [ ] Can parse dates in multiple formats
- [ ] Can standardize bank names
- [ ] Can validate data completeness
- [ ] Can detect duplicates
- [ ] Invalid data rejected with clear errors

**Implementation Notes**:
- Reference: Section 11.3 Task 3.5 of `docs/enhanced_plan.md`
- Create `app/scraping/cleaners/bonus_cleaner.py`:
  ```python
  from decimal import Decimal
  from dateutil import parser
  import re

  class BonusCleaner:
      @staticmethod
      def clean_amount(amount_str):
          """Normalize amount string to Decimal"""
          if isinstance(amount_str, (int, float, Decimal)):
              return Decimal(str(amount_str))

          # Remove $, commas, whitespace
          cleaned = re.sub(r'[$,\s]', '', str(amount_str))
          try:
              return Decimal(cleaned)
          except:
              return None

      @staticmethod
      def clean_date(date_str):
          """Parse date from various formats"""
          if not date_str:
              return None
          try:
              return parser.parse(date_str).date()
          except:
              return None

      @staticmethod
      def standardize_bank_name(name):
          """Standardize bank name"""
          if not name:
              return None

          # Remove common suffixes
          name = re.sub(r'\s+(Bank|N\.A\.|National Association)$', '', name, flags=re.IGNORECASE)

          # Capitalize properly
          return name.strip().title()
  ```
- Create `app/scraping/cleaners/validators.py`:
  ```python
  class BonusValidator:
      @staticmethod
      def validate_bonus(data):
          """Validate bonus data completeness"""
          required_fields = ['bank_name', 'amount', 'source_url']
          errors = []

          for field in required_fields:
              if not data.get(field):
                  errors.append(f'Missing required field: {field}')

          if data.get('amount') and data['amount'] <= 0:
              errors.append('Amount must be positive')

          return len(errors) == 0, errors
  ```
- Test cleaners:
  ```python
  from app.scraping.cleaners.bonus_cleaner import BonusCleaner
  print(BonusCleaner.clean_amount('$300.00'))  # Decimal('300.00')
  print(BonusCleaner.clean_date('December 31, 2025'))  # date object
  ```

---

### BP-018: Implement Scrapy Pipeline

**Type**: Feature
**Priority**: High
**Story Points**: 3
**Sprint**: Sprint 2
**Dependencies**: BP-017, BP-008

**Description**:
Create Scrapy pipeline to clean scraped data, check for duplicates, and save to database. Integrate cleaners and validators.

**Acceptance Criteria**:
- [ ] `app/scraping/scrapy_project/pipelines.py` exists
- [ ] BonusPipeline class implemented
- [ ] Data cleaned using cleaners
- [ ] Duplicates detected and handled
- [ ] Data saved to database
- [ ] Existing records updated
- [ ] Pipeline configured in settings.py
- [ ] End-to-end scraping saves to database

**Implementation Notes**:
- Reference: Section 11.3 Task 3.6 of `docs/enhanced_plan.md`
- Create `app/scraping/scrapy_project/pipelines.py`:
  ```python
  from app.extensions import db
  from app.models.bonus import Bonus
  from app.models.bank import Bank
  from app.scraping.cleaners.bonus_cleaner import BonusCleaner
  from app.scraping.cleaners.validators import BonusValidator
  import logging

  class BonusPipeline:
      def __init__(self):
          self.logger = logging.getLogger(__name__)

      def process_item(self, item, spider):
          # Validate
          is_valid, errors = BonusValidator.validate_bonus(item)
          if not is_valid:
              self.logger.error(f'Invalid bonus data: {errors}')
              return item

          # Clean data
          amount = BonusCleaner.clean_amount(item.get('amount'))
          bank_name = BonusCleaner.standardize_bank_name(item.get('bank_name'))

          # Get or create bank
          bank = Bank.query.filter_by(name=bank_name).first()
          if not bank:
              bank = Bank(name=bank_name, slug=bank_name.lower().replace(' ', '-'))
              db.session.add(bank)
              db.session.flush()

          # Check for duplicate
          existing = Bonus.query.filter_by(
              bank_id=bank.id,
              amount=amount,
              source_url=item.get('source_url')
          ).first()

          if existing:
              # Update existing
              existing.description = item.get('description')
              existing.last_verified_date = date.today()
          else:
              # Create new
              bonus = Bonus(
                  bank_id=bank.id,
                  amount=amount,
                  title=item.get('title', f'{bank_name} ${amount} Bonus'),
                  slug=f'{bank.slug}-{amount}-bonus',
                  description=item.get('description'),
                  source_url=item.get('source_url'),
                  source_name=item.get('source_name')
              )
              db.session.add(bonus)

          db.session.commit()
          return item
  ```
- Configure in `settings.py`:
  ```python
  ITEM_PIPELINES = {
      'app.scraping.scrapy_project.pipelines.BonusPipeline': 300,
  }
  ```
- Test end-to-end:
  ```bash
  cd app/scraping/scrapy_project
  scrapy crawl doctorofcredit
  cd ../../..
  # Check database
  psql -U bankperks -d bankperks -c "SELECT COUNT(*) FROM bonuses;"
  ```

---

## Sprint 3: Celery Task Queue

### BP-019: Setup Celery

**Type**: Feature
**Priority**: Critical
**Story Points**: 3
**Sprint**: Sprint 3
**Dependencies**: BP-004, BP-006

**Description**:
Configure Celery with Flask app context, set up Celery beat schedule, and verify workers can start successfully.

**Acceptance Criteria**:
- [ ] `app/tasks/__init__.py` exists with Celery instance
- [ ] Celery configured with Flask app context
- [ ] Celery beat schedule configured
- [ ] Celery worker starts without errors
- [ ] Celery beat starts without errors
- [ ] Can see workers in Flower UI
- [ ] Test task executes successfully

**Implementation Notes**:
- Reference: Section 11.4 Task 4.1 of `docs/enhanced_plan.md`
- Create `app/tasks/__init__.py`:
  ```python
  from celery import Celery
  from celery.schedules import crontab

  celery = Celery('bankperks')

  def init_celery(app):
      """Initialize Celery with Flask app context"""
      celery.conf.update(app.config)

      class ContextTask(celery.Task):
          def __call__(self, *args, **kwargs):
              with app.app_context():
                  return self.run(*args, **kwargs)

      celery.Task = ContextTask

      # Configure beat schedule
      celery.conf.beat_schedule = {
          'scrape-all-sources': {
              'task': 'app.tasks.scraping_tasks.scrape_all_sources',
              'schedule': crontab(hour=2, minute=0),  # Daily at 2 AM
          },
          'expire-old-bonuses': {
              'task': 'app.tasks.maintenance_tasks.expire_old_bonuses',
              'schedule': crontab(hour=3, minute=0),  # Daily at 3 AM
          },
      }

      return celery
  ```
- Update `app/__init__.py` to call `init_celery(app)` (already in Section 10.5)
- Create test task in `app/tasks/__init__.py`:
  ```python
  @celery.task
  def test_task():
      return 'Celery is working!'
  ```
- Start Celery worker:
  ```bash
  celery -A app.tasks.celery worker --loglevel=info
  ```
- Start Celery beat:
  ```bash
  celery -A app.tasks.celery beat --loglevel=info
  ```
- Start Flower:
  ```bash
  celery -A app.tasks.celery flower
  # Access http://localhost:5555
  ```
- Test task:
  ```python
  from app.tasks import test_task
  result = test_task.delay()
  print(result.get())  # Should print 'Celery is working!'
  ```

---

### BP-020: Implement Scraping Tasks

**Type**: Feature
**Priority**: High
**Story Points**: 3
**Sprint**: Sprint 3
**Dependencies**: BP-019, BP-018

**Description**:
Create Celery tasks for automated scraping including scrape all sources, scrape specific source, and discover new sources.

**Acceptance Criteria**:
- [ ] `app/tasks/scraping_tasks.py` exists
- [ ] `scrape_all_sources()` task implemented
- [ ] `scrape_source(source_id)` task implemented
- [ ] `discover_new_sources()` task implemented
- [ ] Tasks execute successfully
- [ ] Periodic tasks run on schedule
- [ ] Errors handled and logged
- [ ] Can trigger tasks manually

**Implementation Notes**:
- Reference: Section 11.4 Task 4.2 of `docs/enhanced_plan.md`
- Create `app/tasks/scraping_tasks.py`:
  ```python
  from app.tasks import celery
  from app.models.scraping_source import ScrapingSource
  from app.extensions import db
  import subprocess
  import logging

  logger = logging.getLogger(__name__)

  @celery.task
  def scrape_all_sources():
      """Scrape all active sources"""
      sources = ScrapingSource.query.filter_by(status='active').all()

      for source in sources:
          try:
              scrape_source.delay(source.id)
          except Exception as e:
              logger.error(f'Failed to queue scraping for source {source.id}: {e}')

      return f'Queued {len(sources)} sources for scraping'

  @celery.task
  def scrape_source(source_id):
      """Scrape a specific source"""
      source = ScrapingSource.query.get(source_id)
      if not source:
          return f'Source {source_id} not found'

      try:
          # Run Scrapy spider
          result = subprocess.run(
              ['scrapy', 'crawl', 'doctorofcredit'],
              cwd='app/scraping/scrapy_project',
              capture_output=True,
              text=True
          )

          if result.returncode == 0:
              source.last_successful_scrape_at = datetime.utcnow()
              source.consecutive_failures = 0
          else:
              source.consecutive_failures += 1
              source.last_error = result.stderr

          source.last_scraped_at = datetime.utcnow()
          source.total_scrapes += 1
          db.session.commit()

          return f'Scraped source {source_id}'
      except Exception as e:
          logger.error(f'Error scraping source {source_id}: {e}')
          source.consecutive_failures += 1
          source.last_error = str(e)
          db.session.commit()
          raise

  @celery.task
  def discover_new_sources():
      """Discover new scraping sources (placeholder)"""
      # TODO: Implement source discovery logic
      return 'Source discovery not yet implemented'
  ```
- Test tasks:
  ```python
  from app.tasks.scraping_tasks import scrape_all_sources
  result = scrape_all_sources.delay()
  print(result.get())
  ```

---

### BP-021: Implement Maintenance Tasks

**Type**: Feature
**Priority**: Medium
**Story Points**: 2
**Sprint**: Sprint 3
**Dependencies**: BP-019

**Description**:
Create Celery tasks for data maintenance including expiring old bonuses, verifying active bonuses, and cleaning up old data.

**Acceptance Criteria**:
- [ ] `app/tasks/maintenance_tasks.py` exists
- [ ] `expire_old_bonuses()` task implemented
- [ ] `verify_active_bonuses()` task implemented
- [ ] `cleanup_old_data()` task implemented
- [ ] Tasks run successfully
- [ ] Database updated correctly
- [ ] Tasks scheduled to run daily
- [ ] Logs show task execution

**Implementation Notes**:
- Reference: Section 11.4 Task 4.3 of `docs/enhanced_plan.md`
- Create `app/tasks/maintenance_tasks.py`:
  ```python
  from app.tasks import celery
  from app.models.bonus import Bonus
  from app.extensions import db
  from datetime import date, timedelta
  import logging

  logger = logging.getLogger(__name__)

  @celery.task
  def expire_old_bonuses():
      """Mark bonuses as expired if past end date"""
      expired_count = Bonus.query.filter(
          Bonus.end_date < date.today(),
          Bonus.status == 'active'
      ).update({'status': 'expired'})

      db.session.commit()
      logger.info(f'Marked {expired_count} bonuses as expired')
      return f'Expired {expired_count} bonuses'

  @celery.task
  def verify_active_bonuses():
      """Re-check validity of active bonuses (placeholder)"""
      # TODO: Implement verification logic
      # Could re-scrape source URLs to verify bonuses still exist
      return 'Verification not yet implemented'

  @celery.task
  def cleanup_old_data():
      """Remove old records"""
      # Delete bonuses expired more than 1 year ago
      cutoff_date = date.today() - timedelta(days=365)
      deleted_count = Bonus.query.filter(
          Bonus.status == 'expired',
          Bonus.end_date < cutoff_date
      ).delete()

      db.session.commit()
      logger.info(f'Deleted {deleted_count} old bonuses')
      return f'Deleted {deleted_count} old bonuses'
  ```
- Update beat schedule in `app/tasks/__init__.py` to include these tasks
- Test:
  ```python
  from app.tasks.maintenance_tasks import expire_old_bonuses
  result = expire_old_bonuses.delay()
  print(result.get())
  ```

---

## Sprint 4: AI Content Generation

### BP-022: Setup LLM Client

**Type**: Feature
**Priority**: High
**Story Points**: 3
**Sprint**: Sprint 4
**Dependencies**: BP-002, BP-005

**Description**:
Create wrapper for LLM APIs (OpenAI and Anthropic) with error handling, retries, token counting, and rate limiting.

**Acceptance Criteria**:
- [ ] `app/ai/llm_client.py` exists
- [ ] LLMClient class supports OpenAI
- [ ] LLMClient class supports Anthropic
- [ ] API errors handled gracefully
- [ ] Retry logic with exponential backoff
- [ ] Token counting implemented
- [ ] Rate limiting works
- [ ] Can generate text successfully

**Implementation Notes**:
- Reference: Section 11.5 Task 5.1 of `docs/enhanced_plan.md`
- Create `app/ai/llm_client.py`:
  ```python
  import openai
  import anthropic
  from app.utils.retry import retry_with_backoff
  import logging

  logger = logging.getLogger(__name__)

  class LLMClient:
      def __init__(self, provider='openai', api_key=None, model=None):
          self.provider = provider
          self.api_key = api_key
          self.model = model

          if provider == 'openai':
              openai.api_key = api_key
          elif provider == 'anthropic':
              self.client = anthropic.Anthropic(api_key=api_key)

      @retry_with_backoff(max_retries=3, base_delay=2, exceptions=(Exception,))
      def generate(self, prompt, max_tokens=2000, temperature=0.7):
          """Generate text using LLM"""
          try:
              if self.provider == 'openai':
                  response = openai.ChatCompletion.create(
                      model=self.model,
                      messages=[{'role': 'user', 'content': prompt}],
                      max_tokens=max_tokens,
                      temperature=temperature
                  )
                  return response.choices[0].message.content

              elif self.provider == 'anthropic':
                  response = self.client.messages.create(
                      model=self.model,
                      max_tokens=max_tokens,
                      temperature=temperature,
                      messages=[{'role': 'user', 'content': prompt}]
                  )
                  return response.content[0].text

          except Exception as e:
              logger.error(f'LLM generation failed: {e}')
              raise

      def count_tokens(self, text):
          """Estimate token count"""
          # Simple estimation: ~4 characters per token
          return len(text) // 4
  ```
- Create `app/utils/retry.py` with content from Section 13.2
- Test:
  ```python
  from app.ai.llm_client import LLMClient
  import os

  client = LLMClient(
      provider='openai',
      api_key=os.getenv('OPENAI_API_KEY'),
      model='gpt-4'
  )
  result = client.generate('Say hello!')
  print(result)
  ```

---

### BP-023: Create Prompt Templates

**Type**: Feature
**Priority**: High
**Story Points**: 3
**Sprint**: Sprint 4
**Dependencies**: BP-022

**Description**:
Design and implement prompt templates for generating bonus reviews, comparison articles, and newsletters with few-shot examples.

**Acceptance Criteria**:
- [ ] `app/ai/prompts/article_prompts.py` exists
- [ ] `app/ai/prompts/newsletter_prompts.py` exists
- [ ] Bonus review template implemented
- [ ] Comparison article template implemented
- [ ] Newsletter template implemented
- [ ] Templates include few-shot examples
- [ ] Templates generate high-quality content
- [ ] No hallucinations in generated content

**Implementation Notes**:
- Reference: Section 11.5 Task 5.2 and Section 16.2 of `docs/enhanced_plan.md`
- Create `app/ai/prompts/article_prompts.py` with exact content from Section 16.2
- Create `app/ai/prompts/newsletter_prompts.py` with newsletter prompt
- Test templates:
  ```python
  from app.ai.prompts.article_prompts import BONUS_REVIEW_PROMPT
  from app.ai.llm_client import LLMClient

  prompt = BONUS_REVIEW_PROMPT.format(
      bank_name='Chase',
      account_type='Checking',
      amount='300',
      requirements='Set up direct deposit of $500+',
      eligibility='New customers only'
  )

  client = LLMClient(provider='openai', api_key=os.getenv('OPENAI_API_KEY'), model='gpt-4')
  article = client.generate(prompt)
  print(article)
  ```

---

### BP-024: Implement Content Generator

**Type**: Feature
**Priority**: High
**Story Points**: 5
**Sprint**: Sprint 4
**Dependencies**: BP-023, BP-008

**Description**:
Create content generation service that uses LLM client and prompt templates to generate articles and newsletters, then saves to database with review status.

**Acceptance Criteria**:
- [ ] `app/ai/content_generator.py` exists
- [ ] ContentGenerator class implemented
- [ ] `generate_article()` method works
- [ ] `generate_newsletter()` method works
- [ ] Content validation implemented
- [ ] Content saved to database with status='review'
- [ ] Metadata tracked (model, prompt, etc.)
- [ ] Can generate articles for bonuses

**Implementation Notes**:
- Reference: Section 11.5 Task 5.3 of `docs/enhanced_plan.md`
- Create `app/ai/content_generator.py`:
  ```python
  from app.ai.llm_client import LLMClient
  from app.ai.prompts.article_prompts import BONUS_REVIEW_PROMPT, COMPARISON_ARTICLE_PROMPT
  from app.models.article import Article
  from app.models.bonus import Bonus
  from app.extensions import db
  from datetime import datetime
  import logging

  logger = logging.getLogger(__name__)

  class ContentGenerator:
      def __init__(self, provider='openai', api_key=None, model=None):
          self.llm_client = LLMClient(provider=provider, api_key=api_key, model=model)
          self.provider = provider
          self.model = model

      def generate_article(self, bonus_ids, article_type='review'):
          """Generate article for given bonuses"""
          bonuses = Bonus.query.filter(Bonus.id.in_(bonus_ids)).all()

          if not bonuses:
              raise ValueError('No bonuses found')

          if article_type == 'review' and len(bonuses) == 1:
              bonus = bonuses[0]
              prompt = BONUS_REVIEW_PROMPT.format(
                  bank_name=bonus.bank.name,
                  account_type=bonus.account_type.name,
                  amount=bonus.amount,
                  requirements=bonus.requirements,
                  eligibility=bonus.eligibility
              )
          elif article_type == 'comparison':
              bonuses_list = '\n\n'.join([
                  f"- {b.bank.name} {b.account_type.name}: ${b.amount}"
                  for b in bonuses
              ])
              prompt = COMPARISON_ARTICLE_PROMPT.format(bonuses_list=bonuses_list)
          else:
              raise ValueError(f'Unsupported article type: {article_type}')

          # Generate content
          content = self.llm_client.generate(prompt)

          # Extract title (first line)
          lines = content.split('\n')
          title = lines[0].replace('#', '').strip()

          # Create article
          article = Article(
              title=title,
              slug=title.lower().replace(' ', '-')[:500],
              content=content,
              excerpt=content[:200] + '...',
              article_type=article_type,
              generated_by_ai=True,
              ai_model=self.model,
              prompt_template_used=article_type,
              generation_metadata={
                  'provider': self.provider,
                  'model': self.model,
                  'generated_at': datetime.utcnow().isoformat()
              },
              related_bonus_ids=bonus_ids,
              status='review'
          )

          db.session.add(article)
          db.session.commit()

          logger.info(f'Generated article {article.id}: {title}')
          return article
  ```
- Test:
  ```python
  from app.ai.content_generator import ContentGenerator
  import os

  generator = ContentGenerator(
      provider='openai',
      api_key=os.getenv('OPENAI_API_KEY'),
      model='gpt-4'
  )

  # Assuming bonus ID 1 exists
  article = generator.generate_article([1], article_type='review')
  print(f'Generated article: {article.title}')
  ```

---

### BP-025: Implement AI Generation Tasks

**Type**: Feature
**Priority**: Medium
**Story Points**: 2
**Sprint**: Sprint 4
**Dependencies**: BP-024, BP-019

**Description**:
Create Celery tasks for automated AI content generation including weekly articles and newsletters.

**Acceptance Criteria**:
- [ ] `app/tasks/ai_tasks.py` exists
- [ ] `generate_weekly_articles()` task implemented
- [ ] `generate_newsletter()` task implemented
- [ ] Tasks generate content automatically
- [ ] Content requires human review before publishing
- [ ] Errors handled appropriately
- [ ] Tasks scheduled in Celery beat

**Implementation Notes**:
- Reference: Section 11.5 Task 5.4 of `docs/enhanced_plan.md`
- Create `app/tasks/ai_tasks.py`:
  ```python
  from app.tasks import celery
  from app.ai.content_generator import ContentGenerator
  from app.models.bonus import Bonus
  from app.config import Config
  import logging

  logger = logging.getLogger(__name__)

  @celery.task
  def generate_weekly_articles():
      """Generate articles for top bonuses"""
      # Get top 5 active bonuses by amount
      top_bonuses = Bonus.query.filter_by(status='active')\
          .order_by(Bonus.amount.desc())\
          .limit(5)\
          .all()

      if not top_bonuses:
          return 'No active bonuses found'

      generator = ContentGenerator(
          provider='openai',
          api_key=Config.OPENAI_API_KEY,
          model=Config.OPENAI_MODEL
      )

      # Generate comparison article
      try:
          article = generator.generate_article(
              [b.id for b in top_bonuses],
              article_type='comparison'
          )
          logger.info(f'Generated weekly article: {article.id}')
          return f'Generated article {article.id}'
      except Exception as e:
          logger.error(f'Failed to generate weekly article: {e}')
          raise

  @celery.task
  def generate_newsletter():
      """Generate weekly newsletter"""
      # TODO: Implement newsletter generation
      return 'Newsletter generation not yet implemented'
  ```
- Update beat schedule in `app/tasks/__init__.py`:
  ```python
  'generate-weekly-articles': {
      'task': 'app.tasks.ai_tasks.generate_weekly_articles',
      'schedule': crontab(day_of_week=1, hour=10, minute=0),  # Monday 10 AM
  }
  ```
- Test:
  ```python
  from app.tasks.ai_tasks import generate_weekly_articles
  result = generate_weekly_articles.delay()
  print(result.get())
  ```

---

## Sprint 5: Testing and QA

### BP-026: Write Unit Tests for Models

**Type**: Test
**Priority**: High
**Story Points**: 5
**Sprint**: Sprint 5
**Dependencies**: BP-008

**Description**:
Create comprehensive unit tests for all SQLAlchemy models including methods, properties, and relationships. Achieve >90% coverage.

**Acceptance Criteria**:
- [ ] `tests/conftest.py` exists with fixtures
- [ ] `tests/unit/test_models.py` exists
- [ ] All model methods tested
- [ ] All properties tested
- [ ] All relationships tested
- [ ] Edge cases covered
- [ ] All tests pass
- [ ] Coverage >90% for models

**Implementation Notes**:
- Reference: Section 11.6 Task 6.1 and Section 12 of `docs/enhanced_plan.md`
- Create `tests/conftest.py` with exact content from Section 12.1.2
- Create `tests/unit/test_models.py` with exact content from Section 12.2.1
- Run tests:
  ```bash
  pytest tests/unit/test_models.py -v
  pytest tests/unit/test_models.py --cov=app/models --cov-report=html
  ```
- View coverage report:
  ```bash
  open htmlcov/index.html
  ```
- Verify all tests pass and coverage >90%

---

### BP-027: Write Integration Tests for API

**Type**: Test
**Priority**: High
**Story Points**: 5
**Sprint**: Sprint 5
**Dependencies**: BP-011, BP-012

**Description**:
Create integration tests for all API endpoints including pagination, filtering, sorting, and error cases.

**Acceptance Criteria**:
- [ ] `tests/integration/test_api_bonuses.py` exists
- [ ] `tests/integration/test_api_banks.py` exists
- [ ] `tests/integration/test_api_articles.py` exists
- [ ] All endpoints tested
- [ ] Pagination tested
- [ ] Filtering tested
- [ ] Sorting tested
- [ ] Error cases tested (404, 400)
- [ ] All tests pass

**Implementation Notes**:
- Reference: Section 11.6 Task 6.2 and Section 12.3 of `docs/enhanced_plan.md`
- Create `tests/integration/test_api_bonuses.py` with exact content from Section 12.3
- Create similar tests for banks and articles
- Run tests:
  ```bash
  pytest tests/integration/ -v
  pytest tests/integration/test_api_bonuses.py::TestBonusAPI::test_list_bonuses -v
  ```
- Verify all tests pass

---

### BP-028: Write Tests for Scraping Pipeline

**Type**: Test
**Priority**: Medium
**Story Points**: 3
**Sprint**: Sprint 5
**Dependencies**: BP-016, BP-017

**Description**:
Create unit tests for scraping extractors and cleaners with sample HTML fixtures.

**Acceptance Criteria**:
- [ ] `tests/fixtures/sample_html/` directory with test HTML
- [ ] `tests/unit/test_extractors.py` exists
- [ ] `tests/unit/test_cleaners.py` exists
- [ ] Extractors tested with various HTML structures
- [ ] Cleaners handle edge cases
- [ ] All tests pass
- [ ] Coverage >80% for scraping modules

**Implementation Notes**:
- Reference: Section 11.6 Task 6.3 and Section 12.2.2 of `docs/enhanced_plan.md`
- Create sample HTML files in `tests/fixtures/sample_html/`:
  - `doctorofcredit_sample.html`
  - `bonus_page_sample.html`
- Create `tests/unit/test_extractors.py` with exact content from Section 12.2.2
- Create `tests/unit/test_cleaners.py`:
  ```python
  import pytest
  from app.scraping.cleaners.bonus_cleaner import BonusCleaner
  from decimal import Decimal

  class TestBonusCleaner:
      def test_clean_amount(self):
          assert BonusCleaner.clean_amount('$300.00') == Decimal('300.00')
          assert BonusCleaner.clean_amount('$1,500') == Decimal('1500')
          assert BonusCleaner.clean_amount('500') == Decimal('500')

      def test_clean_date(self):
          date = BonusCleaner.clean_date('December 31, 2025')
          assert date.year == 2025
          assert date.month == 12
          assert date.day == 31
  ```
- Run tests:
  ```bash
  pytest tests/unit/test_extractors.py -v
  pytest tests/unit/test_cleaners.py -v
  ```

---

## Sprint 6: Deployment and Operations

### BP-029: Create Docker Configuration

**Type**: DevOps
**Priority**: High
**Story Points**: 3
**Sprint**: Sprint 6
**Dependencies**: BP-006

**Description**:
Create Dockerfile, Dockerfile.celery, and docker-compose.yml for containerized deployment.

**Acceptance Criteria**:
- [ ] `docker/Dockerfile` exists
- [ ] `docker/Dockerfile.celery` exists
- [ ] `docker-compose.yml` exists
- [ ] All services defined (postgres, redis, web, celery_worker, celery_beat, flower)
- [ ] Images build successfully
- [ ] All containers start successfully
- [ ] Can access application at http://localhost:5000
- [ ] Can access Flower at http://localhost:5555

**Implementation Notes**:
- Reference: Section 10.6 of `docs/enhanced_plan.md`
- Create `docker/Dockerfile` with exact content from Section 10.6.1
- Create `docker/Dockerfile.celery` with exact content from Section 10.6.2
- Create `docker-compose.yml` with exact content from Section 10.6.3
- Build and start:
  ```bash
  docker-compose up -d --build
  ```
- Check status:
  ```bash
  docker-compose ps
  docker-compose logs web
  ```
- Test:
  ```bash
  curl http://localhost:5000/api/v1/health
  # Open http://localhost:5555 in browser for Flower
  ```

---

### BP-030: Configure Nginx Reverse Proxy

**Type**: DevOps
**Priority**: Medium
**Story Points**: 2
**Sprint**: Sprint 6
**Dependencies**: BP-029

**Description**:
Create Nginx configuration for reverse proxy with SSL, rate limiting, and security headers.

**Acceptance Criteria**:
- [ ] `nginx/nginx.conf` exists
- [ ] SSL configuration included
- [ ] Rate limiting configured
- [ ] Security headers added
- [ ] Static file serving configured
- [ ] Proxy to Flask app configured
- [ ] Can access via Nginx

**Implementation Notes**:
- Reference: Section 14.1.2 of `docs/enhanced_plan.md`
- Create `nginx/nginx.conf` with exact content from Section 14.1.2
- Create SSL directory:
  ```bash
  mkdir -p nginx/ssl
  ```
- For development, create self-signed certificate:
  ```bash
  openssl req -x509 -nodes -days 365 -newkey rsa:2048 \
    -keyout nginx/ssl/privkey.pem \
    -out nginx/ssl/fullchain.pem \
    -subj "/CN=localhost"
  ```
- Update docker-compose.yml to include nginx service (already in Section 10.6.3)
- Test:
  ```bash
  docker-compose up -d nginx
  curl -k https://localhost/api/v1/health
  ```

---

### BP-031: Create Deployment Scripts

**Type**: DevOps
**Priority**: Medium
**Story Points**: 2
**Sprint**: Sprint 6
**Dependencies**: BP-029

**Description**:
Create deployment, backup, and restore scripts for production operations.

**Acceptance Criteria**:
- [ ] `scripts/deploy.sh` exists and is executable
- [ ] `scripts/backup_db.sh` exists and is executable
- [ ] `scripts/restore_db.sh` exists and is executable
- [ ] Deploy script works end-to-end
- [ ] Backup script creates database backup
- [ ] Restore script restores from backup
- [ ] Scripts include error handling

**Implementation Notes**:
- Reference: Section 14.2 and 14.4 of `docs/enhanced_plan.md`
- Create `scripts/deploy.sh` with exact content from Section 14.2
- Create `scripts/backup_db.sh` with exact content from Section 14.4
- Create `scripts/restore_db.sh` with exact content from Section 14.4
- Make executable:
  ```bash
  chmod +x scripts/deploy.sh
  chmod +x scripts/backup_db.sh
  chmod +x scripts/restore_db.sh
  ```
- Test backup:
  ```bash
  mkdir -p /backups/postgres
  ./scripts/backup_db.sh
  ls -lh /backups/postgres/
  ```
- Test deploy (in staging environment):
  ```bash
  ./scripts/deploy.sh
  ```

---

### BP-032: Setup Monitoring and Logging

**Type**: DevOps
**Priority**: Medium
**Story Points**: 3
**Sprint**: Sprint 6
**Dependencies**: BP-006

**Description**:
Configure Sentry for error monitoring and comprehensive logging for application, scraping, and errors.

**Acceptance Criteria**:
- [ ] Sentry integration added to Flask app
- [ ] Sentry integration added to Celery
- [ ] `app/logging_config.py` exists
- [ ] Application log created (logs/app.log)
- [ ] Error log created (logs/error.log)
- [ ] Scraping log created (logs/scraping.log)
- [ ] Log rotation configured
- [ ] Errors sent to Sentry

**Implementation Notes**:
- Reference: Section 14.3 of `docs/enhanced_plan.md`
- Add Sentry to `app/__init__.py` (content from Section 14.3.1)
- Create `app/logging_config.py` with exact content from Section 14.3.2
- Update `app/__init__.py` to call `setup_logging(app)`
- Add SENTRY_DSN to `.env`:
  ```bash
  SENTRY_DSN=https://your-sentry-dsn@sentry.io/project-id
  ```
- Test logging:
  ```python
  import logging
  logger = logging.getLogger('app')
  logger.info('Test info message')
  logger.error('Test error message')
  # Check logs/app.log and logs/error.log
  ```
- Test Sentry:
  ```python
  # Trigger an error
  1 / 0  # Should appear in Sentry dashboard
  ```

---

### BP-033: Implement Security Features

**Type**: Feature
**Priority**: High
**Story Points**: 3
**Sprint**: Sprint 6
**Dependencies**: BP-010

**Description**:
Implement JWT authentication, API key management, and input validation for API endpoints.

**Acceptance Criteria**:
- [ ] `app/auth/jwt_auth.py` exists
- [ ] `app/auth/api_key.py` exists
- [ ] `app/utils/validators.py` exists
- [ ] JWT token generation works
- [ ] JWT token verification works
- [ ] API key generation works
- [ ] API key validation works
- [ ] Input validation with Pydantic works
- [ ] Protected endpoints require authentication

**Implementation Notes**:
- Reference: Section 15 of `docs/enhanced_plan.md`
- Create `app/auth/jwt_auth.py` with exact content from Section 15.1
- Create `app/auth/api_key.py` with exact content from Section 15.2
- Create `app/utils/validators.py` with exact content from Section 15.3
- Add PyJWT to requirements.txt if not already present
- Test JWT:
  ```python
  from app.auth.jwt_auth import generate_token, verify_token
  token = generate_token(user_id=1)
  print(token)
  user_id = verify_token(token)
  print(user_id)  # Should print 1
  ```
- Apply `@require_auth` decorator to admin endpoints:
  ```python
  from app.auth.jwt_auth import require_auth

  @api_v1_bp.route('/bonuses/<int:id>', methods=['PATCH'])
  @require_auth
  def update_bonus(user_id, id):
      # Only authenticated users can update
      pass
  ```

---

### BP-034: Create Initial Seed Data and Documentation

**Type**: Documentation
**Priority**: Medium
**Story Points**: 2
**Sprint**: Sprint 6
**Dependencies**: BP-009

**Description**:
Create seed data for initial scraping sources and comprehensive README documentation.

**Acceptance Criteria**:
- [ ] `database/seed_sources.py` exists
- [ ] Initial scraping sources seeded
- [ ] `README.md` updated with comprehensive documentation
- [ ] Setup instructions included
- [ ] API documentation linked
- [ ] Development workflow documented
- [ ] Deployment instructions included

**Implementation Notes**:
- Reference: Section 16.1 of `docs/enhanced_plan.md`
- Create `database/seed_sources.py` with exact content from Section 16.1
- Run seed sources:
  ```python
  from database.seed_sources import seed_scraping_sources
  from app import create_app

  app = create_app()
  with app.app_context():
      seed_scraping_sources()
  ```
- Update `README.md`:
  ```markdown
  # Bankperks

  Premier platform for bank sign-up bonus information.

  ## Features
  - Automated web scraping of bank bonus data
  - AI-generated content (articles, newsletters)
  - REST API for bonus data
  - Community forum integration
  - Native Android app (coming soon)

  ## Tech Stack
  - Backend: Python 3.11, Flask
  - Database: PostgreSQL 16
  - Task Queue: Celery + Redis
  - Scraping: Scrapy + Playwright
  - AI: OpenAI GPT-4 / Anthropic Claude

  ## Setup

  ### Prerequisites
  - Python 3.11+
  - PostgreSQL 16
  - Redis 7

  ### Installation

  1. Clone repository:
     ```bash
     git clone https://github.com/yourusername/bankperks.git
     cd bankperks
     ```

  2. Create virtual environment:
     ```bash
     python -m venv venv
     source venv/bin/activate  # On Windows: venv\Scripts\activate
     ```

  3. Install dependencies:
     ```bash
     pip install -r requirements-dev.txt
     python -m spacy download en_core_web_sm
     playwright install chromium
     ```

  4. Setup database:
     ```bash
     createdb bankperks
     createuser -P bankperks
     psql -U bankperks -d bankperks < database/init_db.sql
     ```

  5. Configure environment:
     ```bash
     cp .env.example .env
     # Edit .env with your values
     ```

  6. Run migrations:
     ```bash
     flask db upgrade
     flask seed-db
     ```

  7. Start services:
     ```bash
     # Terminal 1: Flask
     flask run

     # Terminal 2: Celery worker
     celery -A app.tasks.celery worker --loglevel=info

     # Terminal 3: Celery beat
     celery -A app.tasks.celery beat --loglevel=info
     ```

  ## API Documentation

  See `docs/enhanced_plan.md` Section 9.3 for complete API specification.

  ### Example Requests

  ```bash
  # List bonuses
  curl http://localhost:5000/api/v1/bonuses

  # Get specific bonus
  curl http://localhost:5000/api/v1/bonuses/1

  # Filter by bank
  curl http://localhost:5000/api/v1/bonuses?bank_id=1

  # Sort by amount
  curl http://localhost:5000/api/v1/bonuses?sort=-amount
  ```

  ## Development

  ### Running Tests

  ```bash
  # All tests
  pytest

  # With coverage
  pytest --cov=app --cov-report=html

  # Specific test file
  pytest tests/unit/test_models.py -v
  ```

  ### Code Quality

  ```bash
  # Linting
  flake8 app/

  # Formatting
  black app/
  isort app/
  ```

  ## Deployment

  ### Docker

  ```bash
  # Build and start
  docker-compose up -d --build

  # View logs
  docker-compose logs -f web

  # Stop
  docker-compose down
  ```

  ### Production

  See `docs/enhanced_plan.md` Section 14 for deployment guide.

  ## License

  MIT License

  ## Contact

  For questions or support, contact: support@bankperks.net
  ```
- Verify documentation is complete and accurate

---

## Ticket Summary

### Sprint Overview

| Sprint | Tickets | Story Points | Focus Area |
|--------|---------|--------------|------------|
| Sprint 0 | BP-001 to BP-006 | 13 | Project setup and initialization |
| Sprint 1 | BP-007 to BP-012 | 23 | Database models and API foundation |
| Sprint 2 | BP-013 to BP-018 | 21 | Web scraping infrastructure |
| Sprint 3 | BP-019 to BP-021 | 8 | Celery task queue |
| Sprint 4 | BP-022 to BP-025 | 13 | AI content generation |
| Sprint 5 | BP-026 to BP-028 | 13 | Testing and QA |
| Sprint 6 | BP-029 to BP-034 | 15 | Deployment and operations |
| **Total** | **34 tickets** | **106 points** | **Complete system** |

### Dependency Graph

```
Sprint 0 (Setup):
  BP-001 (Project Structure) → BP-002 (Dependencies)
  BP-003 (PostgreSQL) [parallel]
  BP-004 (Redis) [parallel]
  BP-001 → BP-005 (Config)
  BP-001, BP-002, BP-005 → BP-006 (Flask App)

Sprint 1 (Database & API):
  BP-003, BP-006 → BP-007 (DB Schema)
  BP-007 → BP-008 (Models)
  BP-008 → BP-009 (Seed Data)
  BP-008 → BP-010 (Base API)
  BP-010 → BP-011 (Bonus API)
  BP-011 → BP-012 (Bank/Article API)

Sprint 2 (Scraping):
  BP-002 → BP-013 (Scrapy Setup)
  BP-013 → BP-014 (Base Spider)
  BP-014 → BP-015 (DOC Spider)
  BP-015 → BP-016 (Extractors)
  BP-016 → BP-017 (Cleaners)
  BP-017, BP-008 → BP-018 (Pipeline)

Sprint 3 (Celery):
  BP-004, BP-006 → BP-019 (Celery Setup)
  BP-019, BP-018 → BP-020 (Scraping Tasks)
  BP-019 → BP-021 (Maintenance Tasks)

Sprint 4 (AI):
  BP-002, BP-005 → BP-022 (LLM Client)
  BP-022 → BP-023 (Prompts)
  BP-023, BP-008 → BP-024 (Content Generator)
  BP-024, BP-019 → BP-025 (AI Tasks)

Sprint 5 (Testing):
  BP-008 → BP-026 (Model Tests)
  BP-011, BP-012 → BP-027 (API Tests)
  BP-016, BP-017 → BP-028 (Scraping Tests)

Sprint 6 (Deployment):
  BP-006 → BP-029 (Docker)
  BP-029 → BP-030 (Nginx)
  BP-029 → BP-031 (Scripts)
  BP-006 → BP-032 (Monitoring)
  BP-010 → BP-033 (Security)
  BP-009 → BP-034 (Documentation)
```

### Priority Breakdown

- **Critical**: 10 tickets (BP-001, BP-002, BP-003, BP-006, BP-007, BP-008, BP-010, BP-011, BP-019)
- **High**: 15 tickets
- **Medium**: 9 tickets
- **Low**: 0 tickets

### Type Breakdown

- **Setup**: 6 tickets (Sprint 0)
- **Feature**: 19 tickets (Sprints 1-4)
- **Test**: 3 tickets (Sprint 5)
- **DevOps**: 5 tickets (Sprint 6)
- **Documentation**: 1 ticket (Sprint 6)

### Estimated Timeline

Based on story points and assuming 1 developer working full-time:

- **Sprint 0**: 2-3 days (13 points)
- **Sprint 1**: 4-5 days (23 points)
- **Sprint 2**: 4-5 days (21 points)
- **Sprint 3**: 2 days (8 points)
- **Sprint 4**: 2-3 days (13 points)
- **Sprint 5**: 2-3 days (13 points)
- **Sprint 6**: 3 days (15 points)

**Total Estimated Time**: 19-26 days (4-5 weeks)

### Implementation Guidelines

1. **Sequential Execution**: Complete tickets in order within each sprint
2. **Parallel Opportunities**: Tickets marked [parallel] can be done simultaneously
3. **Verification**: Check all acceptance criteria before marking ticket complete
4. **Testing**: Run verification commands after each ticket
5. **Documentation**: Update README and docs as you progress
6. **Git Commits**: Commit after each ticket with message format: `[BP-XXX] Ticket title`

### Success Metrics

- [ ] All 34 tickets completed
- [ ] All acceptance criteria met
- [ ] All tests passing (>80% coverage)
- [ ] Application runs successfully
- [ ] API endpoints functional
- [ ] Scraping pipeline working
- [ ] AI content generation working
- [ ] Docker deployment successful
- [ ] Documentation complete

---

## Next Steps

1. **Start with Sprint 0**: Complete BP-001 through BP-006 to set up the project foundation
2. **Verify Each Ticket**: Run all verification commands and check acceptance criteria
3. **Commit Frequently**: Use git to track progress after each ticket
4. **Test Continuously**: Run tests after implementing features
5. **Document Issues**: If you encounter problems, document them for resolution
6. **Review Enhanced Plan**: Refer to `docs/enhanced_plan.md` for detailed specifications

**Ready to begin? Start with BP-001: Initialize Project Structure**

---

*This ticket system was generated from `docs/enhanced_plan.md` and provides a complete roadmap for implementing the Bankperks system. Each ticket is designed to be independently actionable by an AI coding agent.*






