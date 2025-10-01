Of course. Here is a detailed checklist of tiny tasks for each development ticket, structured for a Test-Driven Development (TDD) workflow.

### **Phase 1: Backend Foundation & Data Storage**

**BP-1: Set Up Core Python Flask Application Structure**

* \[ \] BP-1-T1: Initialize a project directory and Git repository.  
* \[ \] BP-1-T2: Create and activate a Python virtual environment.  
* \[ \] BP-1-T3: Install Flask and create a requirements.txt file.  
* \[ \] BP-1-T4: Create the main application entry point and configuration files.  
* \[ \] BP-1-T5: Write a unit test for a basic health check endpoint (/health).  
* \[ \] BP-1-T6: Implement the basic health check endpoint to return a 200 OK status.  
* \[ \] BP-1-T7: Run and pass the health check unit test.  
* \[ \] BP-1-T8: Refactor the application to use the factory pattern.  
* \[ \] BP-1-T9: Write unit tests to verify that Flask Blueprints for api and scraping can be registered.  
* \[ \] BP-1-T10: Implement the Blueprint structures for api and scraping.  
* \[ \] BP-1-T11: Register the Blueprints in the app factory.  
* \[ \] BP-1-T12: Run and pass all Blueprint registration tests.

**BP-2: Configure PostgreSQL Database and SQLAlchemy Integration**

* \[ \] BP-2-T1: Install PostgreSQL server and create the bankperks database and user.  
* \[ \] BP-2-T2: Install psycopg2-binary and SQLAlchemy packages and add to requirements.txt.  
* \[ \] BP-2-T3: Write a unit test to check for a successful database connection from the Flask app.  
* \[ \] BP-2-T4: Implement the database connection logic in the Flask app's configuration.  
* \[ \] BP-2-T5: Run and pass the database connection test.  
* \[ \] BP-2-T6: Set up the SQLAlchemy ORM instance and integrate it with the Flask application factory.  
* \[ \] BP-2-T7: Install and configure Flask-Migrate for handling schema migrations.

**BP-3: Implement Core Database Schema for Bank Bonuses**

* \[ \] BP-3-T1: Write model unit tests for the Bank table structure and its fields.  
* \[ \] BP-3-T2: Define the Bank model using SQLAlchemy.  
* \[ \] BP-3-T3: Run and pass the Bank model unit tests.  
* \[ \] BP-3-T4: Write model unit tests for the Bonus table, ensuring the JSONB column for source data is included.  
* \[ \] BP-3-T5: Define the Bonus model using SQLAlchemy, including its relationship to Bank.  
* \[ \] BP-3-T6: Run and pass the Bonus model unit tests.  
* \[ \] BP-3-T7: Write model unit tests for Requirement and other related tables.  
* \[ \] BP-3-T8: Define the remaining models and their relationships.  
* \[ \] BP-3-T9: Run and pass all remaining model unit tests.  
* \[ \] BP-3-T10: Generate the initial database migration script using Flask-Migrate.  
* \[ \] BP-3-T11: Apply the migration to the development database.

**BP-4: Set Up Celery with Redis for Asynchronous Task Processing**

* \[ \] BP-4-T1: Install Redis server.  
* \[ \] BP-4-T2: Install celery and redis Python packages and add to requirements.txt.  
* \[ \] BP-4-T3: Write a unit test for a simple Celery task (e.g., an add(x, y) function).  
* \[ \] BP-4-T4: Configure Celery to use the Redis broker within the Flask application context.  
* \[ \] BP-4-T5: Implement the simple add(x, y) Celery task.  
* \[ \] BP-4-T6: Run the test by enqueuing the task and asserting the result, ensuring it works asynchronously.

---

### **Phase 2: Information Gathering & Processing Pipeline**

**BP-5: Initialize Scrapy Framework for Web Crawling**

* \[ \] BP-5-T1: Install Scrapy and add to requirements.txt.  
* \[ \] BP-5-T2: Create a new Scrapy project within the main application directory.  
* \[ \] BP-5-T3: Define the Scrapy Item structure for a bank bonus.  
* \[ \] BP-5-T4: Create a basic spider template.  
* \[ \] BP-5-T5: Configure Scrapy settings for the project (e.g., user agent, request delays).

**BP-6: Implement Static Content Scraper with Beautiful Soup**

* \[ \] BP-6-T1: Install beautifulsoup4 and lxml.  
* \[ \] BP-6-T2: Write a unit test for the extraction logic using a saved, static HTML file as input.  
* \[ \] BP-6-T3: Implement a Scrapy spider that uses Beautiful Soup for parsing the response content.  
* \[ \] BP-6-T4: Implement the item pipeline logic to save the extracted data to the PostgreSQL database.  
* \[ \] BP-6-T5: Run and pass the unit test for the extraction logic.  
* \[ \] BP-6-T6: Conduct an integration test by running the spider against a live target and verifying data in the database.

**BP-7: Integrate Playwright for Dynamic JavaScript-Rendered Content**

* \[ \] BP-7-T1: Install scrapy-playwright and its browser dependencies.  
* \[ \] BP-7-T2: Write a unit test for a spider that targets a JS-heavy page, mocking the Playwright response.  
* \[ \] BP-7-T3: Configure Scrapy settings to enable the Playwright middleware.  
* \[ \] BP-7-T4: Modify a spider to use meta={'playwright': True} for specific requests.  
* \[ \] BP-7-T5: Run and pass the unit test.  
* \[ \] BP-7-T6: Conduct an integration test against a live JS-heavy site to verify content is rendered and extracted correctly.

**BP-8: Implement Anti-Scraping Evasion: IP and User-Agent Rotation**

* \[ \] BP-8-T1: Procure a list of high-quality residential proxies.  
* \[ \] BP-8-T2: Write a unit test for a Scrapy middleware that correctly assigns a User-Agent from a predefined list to a request.  
* \[ \] BP-8-T3: Implement the User-Agent rotation middleware.  
* \[ \] BP-8-T4: Run and pass the User-Agent middleware test.  
* \[ \] BP-8-T5: Write a unit test for a Scrapy middleware that assigns a proxy URL to a request.  
* \[ \] BP-8-T6: Implement the proxy rotation middleware.  
* \[ \] BP-8-T7: Run and pass the proxy middleware test.  
* \[ \] BP-8-T8: Implement and configure randomized download delays in Scrapy settings.

**BP-9: Develop Automated Source Discovery Module**

* \[ \] BP-9-T1: Write unit tests for the seed crawling logic to ensure it discovers and follows links correctly from a sample HTML file.  
* \[ \] BP-9-T2: Implement a Scrapy spider dedicated to seed crawling known financial sites.  
* \[ \] BP-9-T3: Run and pass the seed crawling unit tests.  
* \[ \] BP-9-T4: Write unit tests for a function that programmatically queries search engines via an API.  
* \[ \] BP-9-T5: Implement the search engine query module.  
* \[ \] BP-9-T6: Run and pass the search query unit tests.  
* \[ \] BP-9-T7: Implement a pipeline to store discovered URLs in the database for later analysis.

**BP-10: Implement ML Model for Source Relevance Scoring**

* \[ \] BP-10-T1: Install relevant NLP/ML libraries (e.g., scikit-learn, nltk).  
* \[ \] BP-10-T2: Prepare a labeled dataset of relevant and irrelevant web page text.  
* \[ \] BP-10-T3: Write unit tests for the text preprocessing function (e.g., tokenization, stop-word removal).  
* \[ \] BP-10-T4: Implement the text preprocessing function.  
* \[ \] BP-10-T5: Run and pass the preprocessing tests.  
* \[ \] BP-10-T6: Write tests for the classification model's prediction output on sample data.  
* \[ \] BP-10-T7: Train a text classification model (e.g., Naive Bayes, SVM) to identify relevant pages.  
* \[ \] BP-10-T8: Run and pass the model prediction tests.  
* \[ \] BP-10-T9: Implement a Celery task that takes a URL, scrapes its text, and uses the model to assign a relevance score.

**BP-11: Implement ML-Based Data Extraction with NER**

* \[ \] BP-11-T1: Install spaCy or transformers library.  
* \[ \] BP-11-T2: Download a pre-trained language model.  
* \[ \] BP-11-T3: Write unit tests with sample text snippets containing bonus details (e.g., "Get a $300 bonus...") and assert that the NER model correctly identifies MONEY and ORGANIZATION entities.  
* \[ \] BP-11-T4: Implement a function that takes unstructured text and uses the NER model to extract structured entities.  
* \[ \] BP-11-T5: Run and pass the NER extraction unit tests.  
* \[ \] BP-11-T6: Integrate this function into the Scrapy item pipeline to process text from scraped pages.

**BP-12: Create Automated Data Cleaning and Validation Pipeline**

* \[ \] BP-12-T1: Install pandas.  
* \[ \] BP-12-T2: Write unit tests for a function that handles missing values in a sample Pandas DataFrame.  
* \[ \] BP-12-T3: Implement the missing value handling logic.  
* \[ \] BP-12-T4: Run and pass the missing value tests.  
* \[ \] BP-12-T5: Write unit tests for a function that standardizes data formats (e.g., converting '$500.00' to a numeric 500, 'Dec. 31, 2025' to '2025-12-31').  
* \[ \] BP-12-T6: Implement the data standardization logic.  
* \[ \] BP-12-T7: Run and pass the standardization tests.  
* \[ \] BP-12-T8: Write unit tests to check for and remove duplicate records based on key fields.  
* \[ \] BP-12-T9: Implement the duplicate removal logic.  
* \[ \] BP-12-T10: Run and pass the duplicate removal tests.  
* \[ \] BP-12-T11: Integrate all cleaning steps into a single pipeline function that is called after extraction and before storage.

---

### **Phase 3: AI Content & Forum Implementation**

**BP-13: Integrate with External Large Language Model (LLM) API**

* \[ \] BP-13-T1: Sign up for an LLM API (e.g., OpenAI) and obtain API keys.  
* \[ \] BP-13-T2: Install the required client library (e.g., openai).  
* \[ \] BP-13-T3: Write a unit test that mocks the LLM API client and verifies that a service function correctly formats a request and handles a mock response.  
* \[ \] BP-13-T4: Implement a service module (llm\_service.py) to encapsulate all interactions with the LLM API.  
* \[ \] BP-13-T5: Run and pass the LLM service unit tests.

**BP-14: Develop Prompt Engineering Framework**

* \[ \] BP-14-T1: Design a data structure (e.g., JSON or YAML files) to store prompt templates.  
* \[ \] BP-14-T2: Write a unit test for a function that loads a template and correctly injects dynamic data (e.g., bonus details) into it.  
* \[ \] BP-14-T3: Implement the prompt template loading and formatting function.  
* \[ \] BP-14-T4: Run and pass the prompt formatting tests.  
* \[ \] BP-14-T5: Create initial prompt templates for a detailed bonus review article and a short newsletter summary.

**BP-15: Create Celery Task for Asynchronous Article/Blog Post Generation**

* \[ \] BP-15-T1: Define the database table/model for storing generated content, including status ('draft', 'published') and content fields. Generate and apply migration.  
* \[ \] BP-15-T2: Write a unit test for the Celery task, mocking the database and LLM service calls.  
* \[ \] BP-15-T3: Implement the Celery task that takes bonus IDs, fetches data from the DB, uses the prompt framework (BP-14) and LLM service (BP-13), and saves the result as a 'draft' in the new content table.  
* \[ \] BP-15-T4: Run and pass the unit test for the generation task.

**BP-16: Implement Automated Newsletter Generation Process**

* \[ \] BP-16-T1: Sign up for an email service provider (e.g., SendGrid) and get API keys.  
* \[ \] BP-16-T2: Install the required client library (e.g., sendgrid).  
* \[ \] BP-16-T3: Write a unit test for the newsletter generation logic, mocking DB, LLM, and email service calls.  
* \[ \] BP-16-T4: Implement a scheduled Celery task that filters for recent content, generates summaries using the LLM, assembles an HTML template, and calls the email service API to send it.  
* \[ \] BP-16-T5: Run and pass the newsletter generation unit test.

**BP-17: Implement RSS Feed Generation**

* \[ \] BP-17-T1: Install the feedgen library.  
* \[ \] BP-17-T2: Write a unit test for the RSS feed endpoint. It should mock the database query and assert that the generated XML output is well-formed and contains the correct data.  
* \[ \] BP-17-T3: Implement the Flask view/endpoint at /rss.xml that queries for the latest published content and uses feedgen to build the RSS feed.  
* \[ \] BP-17-T4: Run and pass the RSS feed unit tests.

**BP-18: Deploy and Configure Discourse Forum**

* \[ \] BP-18-T1: Provision a server or virtual machine for the Discourse forum.  
* \[ \] BP-18-T2: Manually install all Discourse dependencies: Ruby, PostgreSQL, Redis, Nginx.  
* \[ \] BP-18-T3: Follow Discourse's official "from source" installation guide to clone the repository and set up the application.  
* \[ \] BP-18-T4: Configure the Discourse discourse.conf file with database credentials, domain name, and email settings.  
* \[ \] BP-18-T5: Run the installation and bootstrap the application.  
* \[ \] BP-18-T6: Create an admin user account and generate an All Users API key for the orchestrator service.

**BP-19: Develop AI Avatar Orchestrator Service**

* \[ \] BP-19-T1: Write unit tests for a service that can read new posts from the Discourse API (using a mocked API client).  
* \[ \] BP-19-T2: Implement the forum monitoring logic that polls the Discourse API for new posts.  
* \[ \] BP-19-T3: Run and pass the forum monitoring tests.  
* \[ \] BP-19-T4: Write unit tests for the avatar's response logic, which should take post context, select a persona prompt, and call the LLM service.  
* \[ \] BP-19-T5: Implement the core response logic for the orchestrator.  
* \[ \] BP-19-T6: Run and pass the response logic tests.  
* \[ \] BP-19-T7: Implement the logic to post the generated response back to Discourse via its API.

**BP-20: Implement Automated Topic Seeding by AI Avatars**

* \[ \] BP-20-T1: Write a unit test for a function that, when triggered, correctly formats a "new topic" request for the Discourse API.  
* \[ \] BP-20-T2: Implement a trigger mechanism (e.g., a database hook or a periodic Celery task) that starts the topic seeding process when a new bonus is found.  
* \[ \] BP-20-T3: Implement the logic within the orchestrator to call the LLM to generate an engaging opening post, then use the Discourse API to create the new topic.  
* \[ \] BP-20-T4: Run and pass the topic seeding unit tests.

**BP-21: Implement Content Moderation with Perspective API**

* \[ \] BP-21-T1: Sign up for Perspective API and get an API key.  
* \[ \] BP-21-T2: Install the Google API client library.  
* \[ \] BP-21-T3: Write a unit test for a function that sends text to a mocked Perspective API and correctly flags content based on the mock response score.  
* \[ \] BP-21-T4: Implement a service to interact with the Perspective API.  
* \[ \] BP-21-T5: Run and pass the service unit tests.  
* \[ \] BP-21-T6: Configure a Discourse webhook that sends new posts to an endpoint in our Flask app, which then uses the service to moderate the content and flag it in Discourse if necessary.

---

### **Phase 4: API & Android Application**

**BP-22: Design and Implement Public REST API v1**

* \[ \] BP-22-T1: Write unit tests for the GET /api/v1/bonuses endpoint, testing for correct data structure, pagination, filtering, and sorting.  
* \[ \] BP-22-T2: Implement the /api/v1/bonuses endpoint using Flask-RESTful or similar.  
* \[ \] BP-22-T3: Run and pass the /bonuses endpoint tests.  
* \[ \] BP-22-T4: Write unit tests for the GET /api/v1/articles endpoint.  
* \[ \] BP-22-T5: Implement the /api/v1/articles endpoint.  
* \[ \] BP-22-T6: Run and pass the /articles endpoint tests.  
* \[ \] BP-22-T7: Repeat the TDD cycle for all other required v1 endpoints (/bonuses/{id}, /articles/{id}, etc.).

**BP-23: Set Up Native Android Project with Kotlin & Jetpack Compose**

* \[ \] BP-23-T1: Create a new project in Android Studio, selecting the "Empty Compose Activity" template.  
* \[ \] BP-23-T2: Configure the build.gradle file with necessary dependencies for Jetpack Compose, Navigation, ViewModel, and Coroutines.  
* \[ \] BP-23-T3: Set up the project package structure (e.g., ui, data, viewmodel, networking).  
* \[ \] BP-23-T4: Create a basic "Hello World" Composable and run the app on an emulator or device to ensure the build environment is correct.

**BP-24: Implement Material Design 3 Theming**

* \[ \] BP-24-T1: Define the primary, secondary, and tertiary colors in ui/theme/Color.kt for both light and dark themes.  
* \[ \] BP-24-T2: Define the typography styles (headline, body, label, etc.) in ui/theme/Type.kt.  
* \[ \] BP-24-T3: Create the AppTheme Composable that applies the defined colors and typography.  
* \[ \] BP-24-T4: Build a simple UI screen with several M3 components (Button, Card, TextField) to visually verify the theme is applied correctly in both light and dark modes.

**BP-25: Implement Server Communication Layer with Retrofit**

* \[ \] BP-25-T1: Add Retrofit, OkHttp, and a JSON converter (e.g., Gson) dependencies to build.gradle.  
* \[ \] BP-25-T2: Create Kotlin data classes that exactly match the JSON structure of the API responses from BP-22.  
* \[ \] BP-25-T3: Write unit tests for the data layer (Repository) that use a mock web server (like MockWebServer) to test the API service.  
* \[ \] BP-25-T4: Define the Retrofit API service interface with suspend functions for each endpoint (e.g., getBonuses()).  
* \[ \] BP-25-T5: Implement the Repository class that uses the Retrofit service to fetch data.  
* \[ \] BP-25-T6: Run and pass all repository unit tests.

**BP-26: Build Bonus Discovery Screens (List & Detail)**

* \[ \] BP-26-T1: Write unit tests for the BonusListViewModel to verify it correctly calls the repository, handles loading/success/error states, and exposes them via StateFlow.  
* \[ \] BP-26-T2: Implement the BonusListViewModel.  
* \[ \] BP-26-T3: Run and pass the ViewModel unit tests.  
* \[ \] BP-26-T4: Write UI tests using Compose Test rules for the BonusListScreen, asserting that it displays a loading indicator, a list of items, or an error message based on the ViewModel state.  
* \[ \] BP-26-T5: Implement the BonusListScreen and BonusListItem Composables.  
* \[ \] BP-26-T6: Run and pass all UI tests.  
* \[ \] BP-26-T7: Repeat the TDD cycle for the BonusDetailViewModel and BonusDetailScreen.

**BP-27: Build Content Consumption Screens (Articles & Newsletter)**

* \[ \] BP-27-T1: Write unit tests for the ArticleListViewModel.  
* \[ \] BP-27-T2: Implement the ArticleListViewModel.  
* \[ \] BP-27-T3: Run and pass the ViewModel tests.  
* \[ \] BP-27-T4: Write UI tests for the ArticleListScreen Composable.  
* \[ \] BP-27-T5: Implement the ArticleListScreen and ArticleListItem Composables.  
* \[ \] BP-27-T6: Run and pass the UI tests.  
* \[ \] BP-27-T7: Repeat the TDD cycle for the ArticleDetailScreen and the dedicated NewsletterScreen.