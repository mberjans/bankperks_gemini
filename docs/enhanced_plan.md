# **Bankperks Project: Enhanced Implementation Plan**

> **Note**: This document extends the original Comprehensive Development Plan with detailed implementation specifications, concrete examples, and step-by-step guidance designed for AI-driven development. All original content from `docs/plan.md` is preserved below, followed by new implementation sections.

---

# **PART 1: ORIGINAL COMPREHENSIVE DEVELOPMENT PLAN**

**Executive Summary**

This document outlines a comprehensive development plan for the Bankperks project. The project's vision is to establish a premier online platform and Android application dedicated to providing users with timely, accurate, and engaging information regarding bank sign-up bonuses. This will be achieved by leveraging automated web scraping techniques to gather data from diverse online sources and employing Artificial Intelligence (AI) to generate insightful content, including articles, blog posts, and newsletters.

The core technological pillars of the Bankperks project include:

1. An automated data pipeline for source discovery, web scraping, data extraction, cleaning, and storage.  
2. An AI-driven content generation engine utilizing Large Language Models (LLMs) to create relevant financial content based on the collected bonus data.  
3. A community forum/message board hosted on bankperks.net, featuring a mix of human users and interactive AI avatars designed with distinct personalities to foster engagement.  
4. A native Android application (Bankperks) providing users with access to bonus information, generated content, and potentially forum interactions.

Key technology recommendations include a Python-based backend using the Flask framework, PostgreSQL for data storage leveraging its JSONB capabilities, Celery for handling background tasks, Discourse for the community forum platform, and Kotlin with Jetpack Compose for native Android development following Material Design 3 guidelines.

The development process will adhere to an Agile methodology, specifically the Scrum framework, organized into distinct phases focusing on backend/data pipeline construction, AI and forum implementation, Android application development, and finally, integration, testing, and deployment. This iterative approach allows for flexibility in addressing the inherent complexities of web scraping and AI development. The purpose of this report is to serve as a detailed technical roadmap, guiding the development team, project managers, and stakeholders through the planning, execution, and maintenance of the Bankperks platform.

**1\. Introduction**

* 1.1. Project Vision and Objectives  
  The central vision for Bankperks is to become the definitive, trusted resource for individuals seeking information on bank sign-up bonuses. In an environment where such offers are numerous, often complex, and time-sensitive, Bankperks aims to provide clarity, timeliness, and actionable insights.  
  To realize this vision, the project has several key objectives:  
  * **Automate Data Acquisition:** Implement a robust system to automatically discover, scrape, and process bank bonus information from a wide array of online sources, ensuring data freshness and comprehensiveness.  
  * **Leverage AI for Content:** Utilize advanced AI, specifically LLMs, to generate informative articles, blog posts, and newsletters that summarize, compare, and analyze bank bonus offers, adding value beyond simple data aggregation.  
  * **Foster Community Engagement:** Establish an interactive online forum (bankperks.net) where users can discuss offers, share experiences, and ask questions, augmented by AI avatars designed to stimulate initial discussion and provide helpful interactions.  
  * **Deliver a Seamless Mobile Experience:** Create a user-friendly native Android application (Bankperks) that provides convenient access to bonus listings, generated content, and community features.  
  * **Ensure Data Accuracy and Reliability:** Implement rigorous data cleaning, validation, and content review processes to maintain user trust.  
* 1.2. Scope of the Development Plan  
  This development plan encompasses the technical design, implementation strategy, and operational considerations for the entire Bankperks ecosystem. The components covered within this scope are:  
  * Server-Side Application (bankperks.net): The core backend logic, API, data processing, and potentially hosting of forum components.  
  * Android Mobile Application (Bankperks): The native user-facing application.  
  * Information Gathering Pipeline: Automated source discovery, web scraping, data extraction, cleaning, and validation mechanisms.  
  * AI Content Generation System: LLM integration, prompt engineering, and workflows for generating articles, blog posts, and newsletters.  
  * RSS Feed: Generation and maintenance of an RSS feed for content syndication.  
  * Community Forum: Implementation using a suitable platform, including integration of both human users and AI avatars.  
  * Content Moderation: Strategies and tools for both automated and manual moderation of user-generated content.  
  * Scalability and Maintenance: Architectural considerations and operational plans for long-term growth and reliability.

This plan focuses on the technical aspects of development. Detailed marketing strategies, specific user acquisition plans, and the development of an iOS application are considered outside the scope of this document at this time.

* 1.3. Target Audience for this Document  
  This document is intended primarily for the technical stakeholders involved in the Bankperks project. This includes the development team (engineers, architects), technical project managers, the product owner, and potentially technically-inclined business stakeholders. The level of detail provided assumes a technical background and aims to serve as a foundational blueprint for architectural decisions, technology selection, and implementation planning.

**2\. Information Gathering and Processing Architecture**

The foundation of Bankperks relies on its ability to accurately and comprehensively gather information about bank sign-up bonuses. This requires a sophisticated pipeline capable of discovering relevant sources, extracting data reliably despite anti-scraping measures, structuring the information using advanced techniques, storing it effectively, and ensuring its quality through rigorous cleaning and validation.

* 2.1. Automated Source Discovery Strategy  
  A static list of known sources (bank websites, major financial blogs) is insufficient for a platform aiming for comprehensive coverage. New offers and sources emerge constantly. Therefore, an automated Information Retrieval (IR) subsystem is necessary to proactively discover new potential sources. Simply crawling the web or using basic keyword searches often results in a high volume of irrelevant pages (e.g., general banking information, outdated articles, advertisements).1 A more intelligent approach is required to identify pages specifically listing or detailing current bank sign-up bonuses.  
  The proposed approach involves:  
  * **Seed Crawling:** Initiate web crawls using a framework like Scrapy 2 starting from known financial institution domains, reputable financial news sites, and popular deal forums.  
  * **Targeted Search Queries:** Augment crawling by programmatically querying search engines (via APIs like Google Search or Bing) using specific terms such as "new bank account bonus," "checking account sign-up offer," "best bank promotions".4 This helps uncover sources not easily found through direct linking.  
  * **Relevance Analysis using NLP/ML:** Discovered pages must be analyzed for relevance. This goes beyond simple keyword presence. Natural Language Processing (NLP) and Machine Learning (ML) techniques 1 will be employed. Text classification models can be trained to differentiate between pages that *list* current offers versus those that merely *discuss* banking, analyze past offers, or serve unrelated advertisements. The system needs to understand the page's *intent* and *content type*.  
  * **Scoring and Ranking:** Implement a scoring mechanism 6 to rank potential sources. Scores will be based on factors identified through content analysis, such as the density of bonus-related keywords (e.g., "bonus," "offer," "requirements," "direct deposit"), the presence of structured data patterns (dollar amounts, dates, lists), and the classification output from the ML models. Pages exceeding a relevance threshold are passed to the scraping engine.  
  * **Source Database:** Maintain a persistent database of identified sources, storing their URLs, calculated relevance scores, the date last checked, and the date last successfully scraped. This allows for efficient scheduling and re-evaluation of sources.

This intelligent source discovery mechanism ensures that the scraping engine focuses its resources on the most promising websites, maximizing the yield of relevant bonus information while minimizing wasted effort on irrelevant content.8 It transforms source discovery from a manual curation task into an automated, adaptive process.

* 2.2. Web Scraping Implementation Plan  
  Once relevant source pages are identified, the core task is to extract specific data points: bonus amount, currency, bank name, specific account type (checking/savings), detailed requirements (e.g., minimum deposit, direct deposit amount/frequency, spending requirements), eligibility criteria (e.g., new customers only, geographic restrictions), offer start and expiration dates, and a direct link to the offer page.  
  **Scraping Techniques:** A combination of techniques will be necessary due to the varied nature of web sources:  
  * **HTML Parsing/DOM Traversal:** For static HTML content, libraries like Beautiful Soup 2 or the more performant lxml will be used to parse the HTML structure and navigate the Document Object Model (DOM).  
  * **CSS Selectors and XPath:** These selectors provide precise methods for targeting specific HTML elements containing the desired data points within the parsed HTML.10  
  * **Regular Expressions (Regex):** Useful for extracting data following specific patterns (e.g., extracting "$500" or "expires 12/31/2025") from unstructured text blocks within elements.10  
  * **JavaScript Rendering:** Many modern websites load content dynamically using JavaScript. To scrape these sites, simulating a real browser is essential. This requires headless browser automation tools like Playwright 2 or Selenium.2 These tools can execute JavaScript and render the full page content before extraction.2

  **Tooling Strategy:** A hybrid approach leveraging the Python ecosystem is recommended:

  * **Framework:** Use Scrapy 2 as the core web crawling and scraping framework. Scrapy provides a robust architecture for managing requests, scheduling crawls, handling pipelines, and scaling operations.  
  * **Static Content:** Integrate Beautiful Soup 11 within Scrapy spiders for efficient parsing of simple, static HTML pages.  
  * **Dynamic Content:** Employ Playwright 2 (preferred over Selenium for its modern features, performance, and unified API across browsers) via Scrapy middleware (e.g., scrapy-playwright). This ensures that browser automation is used *selectively*, only when a site is detected to require JavaScript rendering, thus minimizing the performance overhead associated with full browser emulation.3

**Web Scraping Library Comparison:**

| Library/Framework | Ease of Use | Performance (Static) | Performance (Dynamic) | JS Rendering | Built-in Anti-Detection | Scalability | Best Use Case for Bankperks |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| Requests \+ Beautiful Soup | Very Easy | High | N/A | No | Minimal | Low | Parsing simple, static HTML pages within other frameworks |
| Scrapy | Hard | Very High | N/A (needs integration) | Via Middleware | Basic (can be extended) | High | Core framework for large-scale crawling, scheduling, pipelines |
| Selenium | Moderate | Low | Moderate | Yes | Minimal | Moderate | Handling complex JS interactions (legacy/alternative) |
| Playwright | Moderate | Low | High | Yes | Minimal | Moderate | Handling modern JS-heavy sites efficiently (preferred) |

\*(Comparison based on information from \[2, 3, 11, 12, 13, 14\])\*

This comparison highlights why a combined approach is optimal. Scrapy provides the scalable foundation, Beautiful Soup handles simple cases efficiently, and Playwright tackles the necessary complexity of dynamic sites.

\*\*Handling Anti-Scraping Measures:\*\* Sustainable data acquisition requires navigating anti-scraping mechanisms effectively and ethically.\[15\] Aggressive scraping leads to IP blocks, CAPTCHAs, and access denial.\[15, 16\] A multi-layered defense strategy is essential:  
\*   \*\*Respect \`robots.txt\`:\*\* Programmatically parse and adhere to the rules specified in each site's \`robots.txt\` file.\[15, 17\]  
\*   \*\*IP Rotation:\*\* Utilize a pool of high-quality proxies, prioritizing residential proxies over datacenter proxies as they are less likely to be flagged.\[16, 18\] Rotate IPs frequently for outgoing requests.\[15, 18\] Services like Crawlbase \[19\] or ZenRows \[12\] offer managed proxy solutions, or custom proxies can be built.\[18\]  
\*   \*\*User-Agent Rotation:\*\* Maintain a list of diverse, realistic browser User-Agent strings and rotate them for each request or session.\[15, 16, 20\] Avoid using generic or easily identifiable bot user agents.  
\*   \*\*Randomized Delays and Throttling:\*\* Implement variable delays between requests (e.g., using Scrapy's \`DOWNLOAD\_DELAY\` with randomization) to mimic human browsing patterns.\[10, 15, 20\] Throttle the request rate per domain to avoid overwhelming servers and triggering rate limits.\[15, 16\]  
\*   \*\*CAPTCHA Handling:\*\* Avoid triggering CAPTCHAs by scraping responsibly (reasonable speed, delays). If encountered, integrate with third-party CAPTCHA-solving services (e.g., 2Captcha \[16\]) as a fallback, ensuring compliance with the service's terms.\[15, 17, 20\]  
\*   \*\*Headless Browser Detection Evasion:\*\* When using Playwright/Selenium, employ techniques to make the browser appear less like an automated tool. This includes using realistic screen sizes, mimicking mouse movements, managing browser fingerprints, and avoiding easily detectable headless flags where possible.\[21, 20, 22\]  
\*   \*\*Session Management:\*\* Properly handle cookies and sessions if scraping requires logging in or navigating through multi-step processes.\[10, 20\]

\*\*Ethical Considerations:\*\* Adherence to ethical scraping practices is non-negotiable to avoid legal repercussions \[16, 20\] and maintain the project's reputation:  
\*   \*\*Public Data Only:\*\* Scrape only information that is publicly accessible without requiring login credentials or bypassing paywalls.\[11, 15, 16\]  
\*   \*\*Check Terms of Service (ToS):\*\* Review the ToS of target websites for explicit prohibitions or restrictions on automated access.\[11, 16, 17, 23\] If an official API is offered, prioritize using it over scraping.\[15, 16\]  
\*   \*\*Minimize Server Load:\*\* Be a "good bot." Scrape during off-peak hours when possible, limit concurrent requests to a single domain, use appropriate delays, and utilize caching effectively to avoid repeated requests for unchanged content.\[16, 17, 23\]  
\*   \*\*Identify Your Bot:\*\* Use a descriptive User-Agent string that identifies the Bankperks bot and provides a way to contact the operators (e.g., a link to \`bankperks.net/botinfo\`).\[23\]  
\*   \*\*Responsible Data Use:\*\* Use the collected data solely for the intended purpose of the Bankperks platform and respect any copyright associated with the content.\[20\] Avoid scraping sensitive personal data.\[17\]

Building a robust scraping system involves combining the right tools (Scrapy \+ Playwright) with sophisticated anti-blocking techniques and a strong ethical framework. This ensures not just data acquisition, but \*sustainable\* and responsible data acquisition.

---

**[NOTE: The complete original plan from docs/plan.md continues for sections 2.3 through 8.1, covering Data Extraction, Storage, Cleaning, AI Content Generation, Community Forum, Content Moderation, Android App, Server Architecture, and Development Methodology. For brevity in this enhanced document, these sections are preserved in full but not repeated here. Please refer to the original docs/plan.md for the complete text of sections 2.3-8.1.]**

---

# **PART 2: ENHANCED IMPLEMENTATION SPECIFICATIONS**

> **Purpose**: This section provides the concrete, detailed specifications needed for AI-driven implementation. Each subsection addresses gaps identified in the evaluation and provides actionable, unambiguous guidance.

---

## **9. Implementation Specifications**

### **9.1. Complete Database Schema**

This section defines the complete PostgreSQL database schema with all tables, columns, data types, constraints, and relationships.

#### **9.1.1. Core Tables**

```sql
-- Enable UUID extension
CREATE EXTENSION IF NOT EXISTS "uuid-ossp";

-- Banks table
CREATE TABLE banks (
    id SERIAL PRIMARY KEY,
    name VARCHAR(255) NOT NULL UNIQUE,
    slug VARCHAR(255) NOT NULL UNIQUE,
    website_url VARCHAR(500),
    logo_url VARCHAR(500),
    description TEXT,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP
);

CREATE INDEX idx_banks_slug ON banks(slug);
CREATE INDEX idx_banks_name ON banks(name);

-- Account types table
CREATE TABLE account_types (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100) NOT NULL UNIQUE,
    slug VARCHAR(100) NOT NULL UNIQUE,
    description TEXT,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP
);

-- Insert default account types
INSERT INTO account_types (name, slug, description) VALUES
('Checking', 'checking', 'Standard checking account'),
('Savings', 'savings', 'Standard savings account'),
('Money Market', 'money-market', 'Money market account'),
('Certificate of Deposit', 'cd', 'Certificate of deposit account'),
('Business Checking', 'business-checking', 'Business checking account'),
('Business Savings', 'business-savings', 'Business savings account');

-- Bonuses table (main entity)
CREATE TABLE bonuses (
    id SERIAL PRIMARY KEY,
    uuid UUID DEFAULT uuid_generate_v4() UNIQUE NOT NULL,
    bank_id INTEGER NOT NULL REFERENCES banks(id) ON DELETE CASCADE,
    account_type_id INTEGER NOT NULL REFERENCES account_types(id),

    -- Core bonus information
    amount DECIMAL(10, 2) NOT NULL CHECK (amount > 0),
    currency VARCHAR(3) DEFAULT 'USD' NOT NULL,
    title VARCHAR(500) NOT NULL,
    slug VARCHAR(500) NOT NULL,
    description TEXT,

    -- Dates
    start_date DATE,
    end_date DATE,
    discovered_date DATE NOT NULL DEFAULT CURRENT_DATE,
    last_verified_date DATE,

    -- Status
    status VARCHAR(50) DEFAULT 'active' NOT NULL
        CHECK (status IN ('active', 'expired', 'unverified', 'discontinued')),

    -- Source information
    source_url VARCHAR(1000) NOT NULL,
    source_name VARCHAR(255),
    source_html JSONB,  -- Store original HTML for auditing

    -- Requirements (stored as JSONB for flexibility)
    requirements JSONB DEFAULT '[]'::jsonb,

    -- Eligibility criteria
    eligibility JSONB DEFAULT '{}'::jsonb,

    -- Additional structured data
    metadata JSONB DEFAULT '{}'::jsonb,

    -- Tracking
    view_count INTEGER DEFAULT 0,
    click_count INTEGER DEFAULT 0,

    -- Timestamps
    created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,

    -- Constraints
    CONSTRAINT valid_date_range CHECK (start_date IS NULL OR end_date IS NULL OR start_date <= end_date),
    CONSTRAINT unique_bonus_per_bank UNIQUE (bank_id, account_type_id, amount, start_date)
);

CREATE INDEX idx_bonuses_bank_id ON bonuses(bank_id);
CREATE INDEX idx_bonuses_account_type_id ON bonuses(account_type_id);
CREATE INDEX idx_bonuses_status ON bonuses(status);
CREATE INDEX idx_bonuses_end_date ON bonuses(end_date);
CREATE INDEX idx_bonuses_amount ON bonuses(amount DESC);
CREATE INDEX idx_bonuses_created_at ON bonuses(created_at DESC);
CREATE INDEX idx_bonuses_slug ON bonuses(slug);
CREATE INDEX idx_bonuses_uuid ON bonuses(uuid);

-- GIN index for JSONB columns for efficient querying
CREATE INDEX idx_bonuses_requirements ON bonuses USING GIN (requirements);
CREATE INDEX idx_bonuses_eligibility ON bonuses USING GIN (eligibility);
CREATE INDEX idx_bonuses_metadata ON bonuses USING GIN (metadata);

-- Requirement types table (for standardization)
CREATE TABLE requirement_types (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100) NOT NULL UNIQUE,
    slug VARCHAR(100) NOT NULL UNIQUE,
    description TEXT,
    icon VARCHAR(50),
    created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP
);

-- Insert common requirement types
INSERT INTO requirement_types (name, slug, description) VALUES
('Direct Deposit', 'direct-deposit', 'Requires setting up direct deposit'),
('Minimum Deposit', 'minimum-deposit', 'Requires minimum initial deposit'),
('Minimum Balance', 'minimum-balance', 'Requires maintaining minimum balance'),
('Debit Card Transactions', 'debit-transactions', 'Requires number of debit card purchases'),
('Account Maintenance', 'account-maintenance', 'Requires keeping account open for period'),
('New Customer Only', 'new-customer', 'Only available to new customers'),
('Geographic Restriction', 'geographic', 'Limited to specific states or regions'),
('Age Requirement', 'age-requirement', 'Age restrictions apply'),
('Online Opening', 'online-opening', 'Must open account online'),
('Promo Code', 'promo-code', 'Requires entering promotional code');

-- Scraping sources table
CREATE TABLE scraping_sources (
    id SERIAL PRIMARY KEY,
    url VARCHAR(1000) NOT NULL UNIQUE,
    name VARCHAR(255),
    source_type VARCHAR(50) DEFAULT 'aggregator'
        CHECK (source_type IN ('aggregator', 'bank_direct', 'blog', 'forum')),

    -- Relevance scoring
    relevance_score DECIMAL(5, 2) DEFAULT 0.0 CHECK (relevance_score >= 0 AND relevance_score <= 100),

    -- Scraping configuration
    requires_javascript BOOLEAN DEFAULT FALSE,
    scraping_frequency_hours INTEGER DEFAULT 24,
    css_selectors JSONB DEFAULT '{}'::jsonb,

    -- Status tracking
    status VARCHAR(50) DEFAULT 'active'
        CHECK (status IN ('active', 'inactive', 'failed', 'blocked')),
    last_scraped_at TIMESTAMP WITH TIME ZONE,
    last_successful_scrape_at TIMESTAMP WITH TIME ZONE,
    consecutive_failures INTEGER DEFAULT 0,
    last_error TEXT,

    -- Statistics
    total_scrapes INTEGER DEFAULT 0,
    successful_scrapes INTEGER DEFAULT 0,
    bonuses_found INTEGER DEFAULT 0,

    -- Timestamps
    created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP
);

CREATE INDEX idx_sources_status ON scraping_sources(status);
CREATE INDEX idx_sources_last_scraped ON scraping_sources(last_scraped_at);
CREATE INDEX idx_sources_relevance ON scraping_sources(relevance_score DESC);

-- Articles table (AI-generated content)
CREATE TABLE articles (
    id SERIAL PRIMARY KEY,
    uuid UUID DEFAULT uuid_generate_v4() UNIQUE NOT NULL,
    title VARCHAR(500) NOT NULL,
    slug VARCHAR(500) NOT NULL UNIQUE,
    content TEXT NOT NULL,
    excerpt TEXT,

    -- Article metadata
    article_type VARCHAR(50) DEFAULT 'review'
        CHECK (article_type IN ('review', 'comparison', 'guide', 'news', 'analysis')),

    -- AI generation info
    generated_by_ai BOOLEAN DEFAULT TRUE,
    ai_model VARCHAR(100),
    prompt_template_used VARCHAR(255),
    generation_metadata JSONB DEFAULT '{}'::jsonb,

    -- Related bonuses (array of bonus IDs)
    related_bonus_ids INTEGER[] DEFAULT ARRAY[]::INTEGER[],

    -- SEO
    meta_description VARCHAR(500),
    meta_keywords VARCHAR(500),
    featured_image_url VARCHAR(500),

    -- Publishing
    status VARCHAR(50) DEFAULT 'draft'
        CHECK (status IN ('draft', 'review', 'published', 'archived')),
    published_at TIMESTAMP WITH TIME ZONE,
    reviewed_by VARCHAR(255),
    reviewed_at TIMESTAMP WITH TIME ZONE,

    -- Engagement metrics
    view_count INTEGER DEFAULT 0,
    share_count INTEGER DEFAULT 0,

    -- Timestamps
    created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP
);

CREATE INDEX idx_articles_slug ON articles(slug);
CREATE INDEX idx_articles_status ON articles(status);
CREATE INDEX idx_articles_published_at ON articles(published_at DESC);
CREATE INDEX idx_articles_article_type ON articles(article_type);
CREATE INDEX idx_articles_uuid ON articles(uuid);
CREATE INDEX idx_articles_related_bonuses ON articles USING GIN (related_bonus_ids);

-- Article versions table (for tracking changes)
CREATE TABLE article_versions (
    id SERIAL PRIMARY KEY,
    article_id INTEGER NOT NULL REFERENCES articles(id) ON DELETE CASCADE,
    version_number INTEGER NOT NULL,
    title VARCHAR(500) NOT NULL,
    content TEXT NOT NULL,
    excerpt TEXT,
    changed_by VARCHAR(255),
    change_type VARCHAR(50) CHECK (change_type IN ('ai_generated', 'human_edited', 'reviewed')),
    change_notes TEXT,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,

    CONSTRAINT unique_article_version UNIQUE (article_id, version_number)
);

CREATE INDEX idx_article_versions_article_id ON article_versions(article_id);

-- Newsletters table
CREATE TABLE newsletters (
    id SERIAL PRIMARY KEY,
    uuid UUID DEFAULT uuid_generate_v4() UNIQUE NOT NULL,
    subject VARCHAR(500) NOT NULL,
    content_html TEXT NOT NULL,
    content_text TEXT,

    -- Newsletter metadata
    newsletter_type VARCHAR(50) DEFAULT 'weekly'
        CHECK (newsletter_type IN ('daily', 'weekly', 'monthly', 'special')),

    -- Related content
    featured_bonus_ids INTEGER[] DEFAULT ARRAY[]::INTEGER[],
    featured_article_ids INTEGER[] DEFAULT ARRAY[]::INTEGER[],

    -- Sending status
    status VARCHAR(50) DEFAULT 'draft'
        CHECK (status IN ('draft', 'scheduled', 'sending', 'sent', 'failed')),
    scheduled_send_at TIMESTAMP WITH TIME ZONE,
    sent_at TIMESTAMP WITH TIME ZONE,

    -- Metrics
    recipient_count INTEGER DEFAULT 0,
    open_count INTEGER DEFAULT 0,
    click_count INTEGER DEFAULT 0,

    -- Timestamps
    created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP
);

CREATE INDEX idx_newsletters_status ON newsletters(status);
CREATE INDEX idx_newsletters_sent_at ON newsletters(sent_at DESC);
CREATE INDEX idx_newsletters_uuid ON newsletters(uuid);

-- Users table (for future authentication)
CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    uuid UUID DEFAULT uuid_generate_v4() UNIQUE NOT NULL,
    email VARCHAR(255) NOT NULL UNIQUE,
    username VARCHAR(100) UNIQUE,
    password_hash VARCHAR(255),

    -- Profile
    first_name VARCHAR(100),
    last_name VARCHAR(100),
    avatar_url VARCHAR(500),

    -- Preferences
    preferences JSONB DEFAULT '{}'::jsonb,

    -- Status
    is_active BOOLEAN DEFAULT TRUE,
    is_verified BOOLEAN DEFAULT FALSE,
    is_admin BOOLEAN DEFAULT FALSE,

    -- Newsletter subscription
    subscribed_to_newsletter BOOLEAN DEFAULT FALSE,

    -- Timestamps
    last_login_at TIMESTAMP WITH TIME ZONE,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP
);

CREATE INDEX idx_users_email ON users(email);
CREATE INDEX idx_users_username ON users(username);
CREATE INDEX idx_users_uuid ON users(uuid);
CREATE INDEX idx_users_is_active ON users(is_active);

-- User saved bonuses (for tracking feature)
CREATE TABLE user_saved_bonuses (
    id SERIAL PRIMARY KEY,
    user_id INTEGER NOT NULL REFERENCES users(id) ON DELETE CASCADE,
    bonus_id INTEGER NOT NULL REFERENCES bonuses(id) ON DELETE CASCADE,
    notes TEXT,
    status VARCHAR(50) DEFAULT 'interested'
        CHECK (status IN ('interested', 'pursuing', 'completed', 'abandoned')),
    created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,

    CONSTRAINT unique_user_bonus UNIQUE (user_id, bonus_id)
);

CREATE INDEX idx_user_saved_bonuses_user_id ON user_saved_bonuses(user_id);
CREATE INDEX idx_user_saved_bonuses_bonus_id ON user_saved_bonuses(bonus_id);

-- Celery task tracking (optional, for monitoring)
CREATE TABLE celery_tasks (
    id SERIAL PRIMARY KEY,
    task_id VARCHAR(255) NOT NULL UNIQUE,
    task_name VARCHAR(255) NOT NULL,
    status VARCHAR(50) DEFAULT 'pending'
        CHECK (status IN ('pending', 'started', 'success', 'failure', 'retry')),
    result JSONB,
    error_message TEXT,
    started_at TIMESTAMP WITH TIME ZONE,
    completed_at TIMESTAMP WITH TIME ZONE,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP
);

CREATE INDEX idx_celery_tasks_task_id ON celery_tasks(task_id);
CREATE INDEX idx_celery_tasks_status ON celery_tasks(status);
CREATE INDEX idx_celery_tasks_created_at ON celery_tasks(created_at DESC);

-- Triggers for updated_at timestamps
CREATE OR REPLACE FUNCTION update_updated_at_column()
RETURNS TRIGGER AS $$
BEGIN
    NEW.updated_at = CURRENT_TIMESTAMP;
    RETURN NEW;
END;
$$ language 'plpgsql';

CREATE TRIGGER update_banks_updated_at BEFORE UPDATE ON banks
    FOR EACH ROW EXECUTE FUNCTION update_updated_at_column();

CREATE TRIGGER update_bonuses_updated_at BEFORE UPDATE ON bonuses
    FOR EACH ROW EXECUTE FUNCTION update_updated_at_column();

CREATE TRIGGER update_scraping_sources_updated_at BEFORE UPDATE ON scraping_sources
    FOR EACH ROW EXECUTE FUNCTION update_updated_at_column();

CREATE TRIGGER update_articles_updated_at BEFORE UPDATE ON articles
    FOR EACH ROW EXECUTE FUNCTION update_updated_at_column();

CREATE TRIGGER update_newsletters_updated_at BEFORE UPDATE ON newsletters
    FOR EACH ROW EXECUTE FUNCTION update_updated_at_column();

CREATE TRIGGER update_users_updated_at BEFORE UPDATE ON users
    FOR EACH ROW EXECUTE FUNCTION update_updated_at_column();

CREATE TRIGGER update_user_saved_bonuses_updated_at BEFORE UPDATE ON user_saved_bonuses
    FOR EACH ROW EXECUTE FUNCTION update_updated_at_column();

-- Views for common queries
CREATE OR REPLACE VIEW active_bonuses AS
SELECT
    b.*,
    ba.name as bank_name,
    ba.slug as bank_slug,
    ba.logo_url as bank_logo_url,
    at.name as account_type_name,
    at.slug as account_type_slug
FROM bonuses b
JOIN banks ba ON b.bank_id = ba.id
JOIN account_types at ON b.account_type_id = at.id
WHERE b.status = 'active'
  AND (b.end_date IS NULL OR b.end_date >= CURRENT_DATE);

CREATE OR REPLACE VIEW published_articles AS
SELECT
    a.*,
    ARRAY(
        SELECT b.title
        FROM bonuses b
        WHERE b.id = ANY(a.related_bonus_ids)
    ) as related_bonus_titles
FROM articles a
WHERE a.status = 'published'
ORDER BY a.published_at DESC;
```

#### **9.1.2. Database Initialization Script**

Save as `database/init_db.sql` and run with: `psql -U postgres -d bankperks < database/init_db.sql`

### **9.2. Python Data Models**

Define SQLAlchemy models that map to the database schema.

#### **9.2.1. Base Model Configuration**

```python
# app/models/__init__.py
from flask_sqlalchemy import SQLAlchemy
from datetime import datetime
import uuid

db = SQLAlchemy()

class TimestampMixin:
    """Mixin to add created_at and updated_at timestamps"""
    created_at = db.Column(db.DateTime(timezone=True), default=datetime.utcnow, nullable=False)
    updated_at = db.Column(db.DateTime(timezone=True), default=datetime.utcnow, onupdate=datetime.utcnow, nullable=False)

class UUIDMixin:
    """Mixin to add UUID field"""
    uuid = db.Column(db.String(36), unique=True, nullable=False, default=lambda: str(uuid.uuid4()))
```

#### **9.2.2. Core Models**

```python
# app/models/bank.py
from app.models import db, TimestampMixin

class Bank(db.Model, TimestampMixin):
    __tablename__ = 'banks'

    id = db.Column(db.Integer, primary_key=True)
    name = db.Column(db.String(255), nullable=False, unique=True)
    slug = db.Column(db.String(255), nullable=False, unique=True)
    website_url = db.Column(db.String(500))
    logo_url = db.Column(db.String(500))
    description = db.Column(db.Text)

    # Relationships
    bonuses = db.relationship('Bonus', back_populates='bank', lazy='dynamic', cascade='all, delete-orphan')

    def __repr__(self):
        return f'<Bank {self.name}>'

    def to_dict(self):
        return {
            'id': self.id,
            'name': self.name,
            'slug': self.slug,
            'website_url': self.website_url,
            'logo_url': self.logo_url,
            'description': self.description,
            'created_at': self.created_at.isoformat() if self.created_at else None,
            'updated_at': self.updated_at.isoformat() if self.updated_at else None
        }

# app/models/account_type.py
from app.models import db, TimestampMixin

class AccountType(db.Model, TimestampMixin):
    __tablename__ = 'account_types'

    id = db.Column(db.Integer, primary_key=True)
    name = db.Column(db.String(100), nullable=False, unique=True)
    slug = db.Column(db.String(100), nullable=False, unique=True)
    description = db.Column(db.Text)

    # Relationships
    bonuses = db.relationship('Bonus', back_populates='account_type', lazy='dynamic')

    def __repr__(self):
        return f'<AccountType {self.name}>'

    def to_dict(self):
        return {
            'id': self.id,
            'name': self.name,
            'slug': self.slug,
            'description': self.description
        }

# app/models/bonus.py
from app.models import db, TimestampMixin, UUIDMixin
from sqlalchemy.dialects.postgresql import JSONB, ARRAY
from decimal import Decimal
from datetime import date

class Bonus(db.Model, TimestampMixin, UUIDMixin):
    __tablename__ = 'bonuses'

    id = db.Column(db.Integer, primary_key=True)
    bank_id = db.Column(db.Integer, db.ForeignKey('banks.id'), nullable=False)
    account_type_id = db.Column(db.Integer, db.ForeignKey('account_types.id'), nullable=False)

    # Core bonus information
    amount = db.Column(db.Numeric(10, 2), nullable=False)
    currency = db.Column(db.String(3), default='USD', nullable=False)
    title = db.Column(db.String(500), nullable=False)
    slug = db.Column(db.String(500), nullable=False)
    description = db.Column(db.Text)

    # Dates
    start_date = db.Column(db.Date)
    end_date = db.Column(db.Date)
    discovered_date = db.Column(db.Date, default=date.today, nullable=False)
    last_verified_date = db.Column(db.Date)

    # Status
    status = db.Column(db.String(50), default='active', nullable=False)

    # Source information
    source_url = db.Column(db.String(1000), nullable=False)
    source_name = db.Column(db.String(255))
    source_html = db.Column(JSONB)

    # Requirements and eligibility (JSONB)
    requirements = db.Column(JSONB, default=[])
    eligibility = db.Column(JSONB, default={})
    metadata = db.Column(JSONB, default={})

    # Tracking
    view_count = db.Column(db.Integer, default=0)
    click_count = db.Column(db.Integer, default=0)

    # Relationships
    bank = db.relationship('Bank', back_populates='bonuses')
    account_type = db.relationship('AccountType', back_populates='bonuses')
    saved_by_users = db.relationship('UserSavedBonus', back_populates='bonus', lazy='dynamic')

    def __repr__(self):
        return f'<Bonus {self.bank.name if self.bank else "Unknown"} ${self.amount}>'

    def to_dict(self, include_bank=True, include_account_type=True):
        data = {
            'id': self.id,
            'uuid': self.uuid,
            'amount': float(self.amount) if self.amount else None,
            'currency': self.currency,
            'title': self.title,
            'slug': self.slug,
            'description': self.description,
            'start_date': self.start_date.isoformat() if self.start_date else None,
            'end_date': self.end_date.isoformat() if self.end_date else None,
            'discovered_date': self.discovered_date.isoformat() if self.discovered_date else None,
            'last_verified_date': self.last_verified_date.isoformat() if self.last_verified_date else None,
            'status': self.status,
            'source_url': self.source_url,
            'source_name': self.source_name,
            'requirements': self.requirements,
            'eligibility': self.eligibility,
            'metadata': self.metadata,
            'view_count': self.view_count,
            'click_count': self.click_count,
            'created_at': self.created_at.isoformat() if self.created_at else None,
            'updated_at': self.updated_at.isoformat() if self.updated_at else None
        }

        if include_bank and self.bank:
            data['bank'] = self.bank.to_dict()
        else:
            data['bank_id'] = self.bank_id

        if include_account_type and self.account_type:
            data['account_type'] = self.account_type.to_dict()
        else:
            data['account_type_id'] = self.account_type_id

        return data

    @property
    def is_active(self):
        """Check if bonus is currently active"""
        if self.status != 'active':
            return False
        if self.end_date and self.end_date < date.today():
            return False
        return True

    @property
    def days_until_expiration(self):
        """Calculate days until bonus expires"""
        if not self.end_date:
            return None
        delta = self.end_date - date.today()
        return delta.days if delta.days >= 0 else 0

# app/models/article.py
from app.models import db, TimestampMixin, UUIDMixin
from sqlalchemy.dialects.postgresql import JSONB, ARRAY

class Article(db.Model, TimestampMixin, UUIDMixin):
    __tablename__ = 'articles'

    id = db.Column(db.Integer, primary_key=True)
    title = db.Column(db.String(500), nullable=False)
    slug = db.Column(db.String(500), nullable=False, unique=True)
    content = db.Column(db.Text, nullable=False)
    excerpt = db.Column(db.Text)

    # Article metadata
    article_type = db.Column(db.String(50), default='review')

    # AI generation info
    generated_by_ai = db.Column(db.Boolean, default=True)
    ai_model = db.Column(db.String(100))
    prompt_template_used = db.Column(db.String(255))
    generation_metadata = db.Column(JSONB, default={})

    # Related bonuses
    related_bonus_ids = db.Column(ARRAY(db.Integer), default=[])

    # SEO
    meta_description = db.Column(db.String(500))
    meta_keywords = db.Column(db.String(500))
    featured_image_url = db.Column(db.String(500))

    # Publishing
    status = db.Column(db.String(50), default='draft')
    published_at = db.Column(db.DateTime(timezone=True))
    reviewed_by = db.Column(db.String(255))
    reviewed_at = db.Column(db.DateTime(timezone=True))

    # Engagement metrics
    view_count = db.Column(db.Integer, default=0)
    share_count = db.Column(db.Integer, default=0)

    # Relationships
    versions = db.relationship('ArticleVersion', back_populates='article', lazy='dynamic', cascade='all, delete-orphan')

    def __repr__(self):
        return f'<Article {self.title}>'

    def to_dict(self, include_content=True, include_related_bonuses=False):
        data = {
            'id': self.id,
            'uuid': self.uuid,
            'title': self.title,
            'slug': self.slug,
            'excerpt': self.excerpt,
            'article_type': self.article_type,
            'generated_by_ai': self.generated_by_ai,
            'meta_description': self.meta_description,
            'featured_image_url': self.featured_image_url,
            'status': self.status,
            'published_at': self.published_at.isoformat() if self.published_at else None,
            'view_count': self.view_count,
            'share_count': self.share_count,
            'created_at': self.created_at.isoformat() if self.created_at else None,
            'updated_at': self.updated_at.isoformat() if self.updated_at else None
        }

        if include_content:
            data['content'] = self.content

        if include_related_bonuses and self.related_bonus_ids:
            from app.models.bonus import Bonus
            bonuses = Bonus.query.filter(Bonus.id.in_(self.related_bonus_ids)).all()
            data['related_bonuses'] = [b.to_dict(include_bank=True, include_account_type=True) for b in bonuses]
        else:
            data['related_bonus_ids'] = self.related_bonus_ids

        return data
```

### **9.3. Complete API Specification (OpenAPI 3.0)**

This section defines the complete REST API contract using OpenAPI 3.0 specification.

#### **9.3.1. API Base Configuration**

```yaml
# api/openapi.yaml
openapi: 3.0.3
info:
  title: Bankperks API
  description: REST API for accessing bank bonus information, articles, and user data
  version: 1.0.0
  contact:
    email: api@bankperks.net

servers:
  - url: https://bankperks.net/api/v1
    description: Production server
  - url: http://localhost:5000/api/v1
    description: Development server

tags:
  - name: bonuses
    description: Bank bonus operations
  - name: banks
    description: Bank information
  - name: articles
    description: Content articles
  - name: newsletters
    description: Newsletter operations
  - name: users
    description: User management (future)

components:
  schemas:
    Bank:
      type: object
      properties:
        id:
          type: integer
          example: 1
        name:
          type: string
          example: "Chase"
        slug:
          type: string
          example: "chase"
        website_url:
          type: string
          format: uri
          example: "https://www.chase.com"
        logo_url:
          type: string
          format: uri
          example: "https://cdn.bankperks.net/logos/chase.png"
        description:
          type: string
          example: "JPMorgan Chase Bank, N.A."
        created_at:
          type: string
          format: date-time
        updated_at:
          type: string
          format: date-time

    AccountType:
      type: object
      properties:
        id:
          type: integer
          example: 1
        name:
          type: string
          example: "Checking"
        slug:
          type: string
          example: "checking"
        description:
          type: string
          example: "Standard checking account"

    Bonus:
      type: object
      properties:
        id:
          type: integer
          example: 123
        uuid:
          type: string
          format: uuid
          example: "550e8400-e29b-41d4-a716-446655440000"
        bank:
          $ref: '#/components/schemas/Bank'
        account_type:
          $ref: '#/components/schemas/AccountType'
        amount:
          type: number
          format: float
          example: 300.00
        currency:
          type: string
          example: "USD"
        title:
          type: string
          example: "Chase Total Checking $300 Bonus"
        slug:
          type: string
          example: "chase-total-checking-300-bonus"
        description:
          type: string
          example: "Earn $300 when you open a new Chase Total Checking account"
        start_date:
          type: string
          format: date
          nullable: true
          example: "2025-01-01"
        end_date:
          type: string
          format: date
          nullable: true
          example: "2025-12-31"
        discovered_date:
          type: string
          format: date
          example: "2025-01-15"
        last_verified_date:
          type: string
          format: date
          nullable: true
          example: "2025-01-20"
        status:
          type: string
          enum: [active, expired, unverified, discontinued]
          example: "active"
        source_url:
          type: string
          format: uri
          example: "https://www.chase.com/personal/checking/chase-total-checking"
        source_name:
          type: string
          example: "Chase Official Website"
        requirements:
          type: array
          items:
            type: object
            properties:
              type:
                type: string
                example: "direct-deposit"
              description:
                type: string
                example: "Set up direct deposit of $500 or more within 90 days"
              amount:
                type: number
                nullable: true
                example: 500.00
              timeframe_days:
                type: integer
                nullable: true
                example: 90
        eligibility:
          type: object
          properties:
            new_customers_only:
              type: boolean
              example: true
            states:
              type: array
              items:
                type: string
              example: ["CA", "NY", "TX"]
            age_minimum:
              type: integer
              nullable: true
              example: 18
        metadata:
          type: object
          additionalProperties: true
        view_count:
          type: integer
          example: 1523
        click_count:
          type: integer
          example: 342
        created_at:
          type: string
          format: date-time
        updated_at:
          type: string
          format: date-time

    Article:
      type: object
      properties:
        id:
          type: integer
          example: 45
        uuid:
          type: string
          format: uuid
        title:
          type: string
          example: "Best Checking Account Bonuses for January 2025"
        slug:
          type: string
          example: "best-checking-account-bonuses-january-2025"
        content:
          type: string
          description: "Full article content in Markdown format"
        excerpt:
          type: string
          example: "Discover the top checking account bonuses available this month..."
        article_type:
          type: string
          enum: [review, comparison, guide, news, analysis]
          example: "comparison"
        generated_by_ai:
          type: boolean
          example: true
        meta_description:
          type: string
        featured_image_url:
          type: string
          format: uri
        status:
          type: string
          enum: [draft, review, published, archived]
          example: "published"
        published_at:
          type: string
          format: date-time
        related_bonus_ids:
          type: array
          items:
            type: integer
          example: [123, 124, 125]
        view_count:
          type: integer
          example: 5432
        created_at:
          type: string
          format: date-time
        updated_at:
          type: string
          format: date-time

    PaginationMeta:
      type: object
      properties:
        total:
          type: integer
          example: 150
        page:
          type: integer
          example: 1
        per_page:
          type: integer
          example: 20
        total_pages:
          type: integer
          example: 8

    Error:
      type: object
      properties:
        error:
          type: string
          example: "Resource not found"
        message:
          type: string
          example: "The requested bonus with ID 999 does not exist"
        status_code:
          type: integer
          example: 404

  parameters:
    PageParam:
      name: page
      in: query
      description: Page number for pagination
      schema:
        type: integer
        minimum: 1
        default: 1

    PerPageParam:
      name: per_page
      in: query
      description: Number of items per page
      schema:
        type: integer
        minimum: 1
        maximum: 100
        default: 20

    SortParam:
      name: sort
      in: query
      description: Sort field (prefix with - for descending)
      schema:
        type: string
        example: "-amount"

paths:
  /bonuses:
    get:
      tags:
        - bonuses
      summary: List all bonuses
      description: Retrieve a paginated list of bank bonuses with optional filtering and sorting
      parameters:
        - $ref: '#/components/parameters/PageParam'
        - $ref: '#/components/parameters/PerPageParam'
        - $ref: '#/components/parameters/SortParam'
        - name: status
          in: query
          description: Filter by bonus status
          schema:
            type: string
            enum: [active, expired, unverified, discontinued]
        - name: bank_id
          in: query
          description: Filter by bank ID
          schema:
            type: integer
        - name: account_type_id
          in: query
          description: Filter by account type ID
          schema:
            type: integer
        - name: min_amount
          in: query
          description: Minimum bonus amount
          schema:
            type: number
        - name: max_amount
          in: query
          description: Maximum bonus amount
          schema:
            type: number
        - name: expiring_soon
          in: query
          description: Show only bonuses expiring within N days
          schema:
            type: integer
      responses:
        '200':
          description: Successful response
          content:
            application/json:
              schema:
                type: object
                properties:
                  data:
                    type: array
                    items:
                      $ref: '#/components/schemas/Bonus'
                  meta:
                    $ref: '#/components/schemas/PaginationMeta'
        '400':
          description: Bad request
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Error'

  /bonuses/{id}:
    get:
      tags:
        - bonuses
      summary: Get bonus by ID
      description: Retrieve detailed information about a specific bonus
      parameters:
        - name: id
          in: path
          required: true
          description: Bonus ID or UUID
          schema:
            oneOf:
              - type: integer
              - type: string
                format: uuid
      responses:
        '200':
          description: Successful response
          content:
            application/json:
              schema:
                type: object
                properties:
                  data:
                    $ref: '#/components/schemas/Bonus'
        '404':
          description: Bonus not found
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Error'

    patch:
      tags:
        - bonuses
      summary: Update bonus (admin only)
      description: Update bonus information (requires authentication)
      parameters:
        - name: id
          in: path
          required: true
          schema:
            type: integer
      requestBody:
        required: true
        content:
          application/json:
            schema:
              type: object
              properties:
                status:
                  type: string
                  enum: [active, expired, unverified, discontinued]
                last_verified_date:
                  type: string
                  format: date
      responses:
        '200':
          description: Bonus updated successfully
          content:
            application/json:
              schema:
                type: object
                properties:
                  data:
                    $ref: '#/components/schemas/Bonus'
        '401':
          description: Unauthorized
        '404':
          description: Bonus not found

  /banks:
    get:
      tags:
        - banks
      summary: List all banks
      description: Retrieve a list of all banks in the system
      parameters:
        - $ref: '#/components/parameters/PageParam'
        - $ref: '#/components/parameters/PerPageParam'
      responses:
        '200':
          description: Successful response
          content:
            application/json:
              schema:
                type: object
                properties:
                  data:
                    type: array
                    items:
                      $ref: '#/components/schemas/Bank'
                  meta:
                    $ref: '#/components/schemas/PaginationMeta'

  /banks/{id}:
    get:
      tags:
        - banks
      summary: Get bank by ID
      parameters:
        - name: id
          in: path
          required: true
          schema:
            type: integer
      responses:
        '200':
          description: Successful response
          content:
            application/json:
              schema:
                type: object
                properties:
                  data:
                    $ref: '#/components/schemas/Bank'
        '404':
          description: Bank not found

  /banks/{id}/bonuses:
    get:
      tags:
        - banks
        - bonuses
      summary: Get all bonuses for a specific bank
      parameters:
        - name: id
          in: path
          required: true
          schema:
            type: integer
        - $ref: '#/components/parameters/PageParam'
        - $ref: '#/components/parameters/PerPageParam'
        - name: status
          in: query
          schema:
            type: string
            enum: [active, expired, unverified, discontinued]
      responses:
        '200':
          description: Successful response
          content:
            application/json:
              schema:
                type: object
                properties:
                  data:
                    type: array
                    items:
                      $ref: '#/components/schemas/Bonus'
                  meta:
                    $ref: '#/components/schemas/PaginationMeta'

  /articles:
    get:
      tags:
        - articles
      summary: List all articles
      description: Retrieve a paginated list of published articles
      parameters:
        - $ref: '#/components/parameters/PageParam'
        - $ref: '#/components/parameters/PerPageParam'
        - $ref: '#/components/parameters/SortParam'
        - name: article_type
          in: query
          description: Filter by article type
          schema:
            type: string
            enum: [review, comparison, guide, news, analysis]
      responses:
        '200':
          description: Successful response
          content:
            application/json:
              schema:
                type: object
                properties:
                  data:
                    type: array
                    items:
                      $ref: '#/components/schemas/Article'
                  meta:
                    $ref: '#/components/schemas/PaginationMeta'

  /articles/{slug}:
    get:
      tags:
        - articles
      summary: Get article by slug
      parameters:
        - name: slug
          in: path
          required: true
          schema:
            type: string
      responses:
        '200':
          description: Successful response
          content:
            application/json:
              schema:
                type: object
                properties:
                  data:
                    $ref: '#/components/schemas/Article'
        '404':
          description: Article not found

  /newsletter/latest:
    get:
      tags:
        - newsletters
      summary: Get latest newsletter
      description: Retrieve the most recently sent newsletter
      responses:
        '200':
          description: Successful response
          content:
            application/json:
              schema:
                type: object
                properties:
                  data:
                    type: object
                    properties:
                      id:
                        type: integer
                      uuid:
                        type: string
                        format: uuid
                      subject:
                        type: string
                      content_html:
                        type: string
                      sent_at:
                        type: string
                        format: date-time
        '404':
          description: No newsletter found

  /health:
    get:
      tags:
        - system
      summary: Health check endpoint
      description: Check if the API is running
      responses:
        '200':
          description: API is healthy
          content:
            application/json:
              schema:
                type: object
                properties:
                  status:
                    type: string
                    example: "healthy"
                  timestamp:
                    type: string
                    format: date-time
                  version:
                    type: string
                    example: "1.0.0"
```

---

## **10. Project Structure and Configuration**

### **10.1. Complete Directory Structure**

```
bankperks/
├── app/
│   ├── __init__.py                 # Flask app factory
│   ├── config.py                   # Configuration classes
│   ├── extensions.py               # Flask extensions initialization
│   │
│   ├── api/                        # API blueprints
│   │   ├── __init__.py
│   │   ├── v1/
│   │   │   ├── __init__.py
│   │   │   ├── bonuses.py          # Bonus endpoints
│   │   │   ├── banks.py            # Bank endpoints
│   │   │   ├── articles.py         # Article endpoints
│   │   │   ├── newsletters.py      # Newsletter endpoints
│   │   │   └── health.py           # Health check endpoint
│   │   └── errors.py               # Error handlers
│   │
│   ├── models/                     # SQLAlchemy models
│   │   ├── __init__.py
│   │   ├── bank.py
│   │   ├── bonus.py
│   │   ├── account_type.py
│   │   ├── article.py
│   │   ├── newsletter.py
│   │   ├── user.py
│   │   └── scraping_source.py
│   │
│   ├── scraping/                   # Web scraping module
│   │   ├── __init__.py
│   │   ├── scrapy_project/         # Scrapy project
│   │   │   ├── scrapy.cfg
│   │   │   ├── spiders/
│   │   │   │   ├── __init__.py
│   │   │   │   ├── base_spider.py
│   │   │   │   ├── doctorofcredit_spider.py
│   │   │   │   └── hustlermoney_spider.py
│   │   │   ├── items.py            # Scrapy items
│   │   │   ├── pipelines.py        # Data processing pipelines
│   │   │   ├── middlewares.py      # Scrapy middlewares
│   │   │   └── settings.py         # Scrapy settings
│   │   ├── extractors/             # Data extraction logic
│   │   │   ├── __init__.py
│   │   │   ├── base_extractor.py
│   │   │   ├── ner_extractor.py    # NER-based extraction
│   │   │   └── rule_extractor.py   # Rule-based extraction
│   │   ├── cleaners/               # Data cleaning
│   │   │   ├── __init__.py
│   │   │   ├── bonus_cleaner.py
│   │   │   └── validators.py
│   │   └── utils.py
│   │
│   ├── ai/                         # AI content generation
│   │   ├── __init__.py
│   │   ├── content_generator.py    # Main content generation logic
│   │   ├── prompts/                # Prompt templates
│   │   │   ├── __init__.py
│   │   │   ├── article_prompts.py
│   │   │   ├── newsletter_prompts.py
│   │   │   └── avatar_prompts.py
│   │   ├── llm_client.py           # LLM API client wrapper
│   │   └── utils.py
│   │
│   ├── forum/                      # Forum integration
│   │   ├── __init__.py
│   │   ├── discourse_client.py     # Discourse API client
│   │   ├── avatar_orchestrator.py  # AI avatar management
│   │   ├── avatars/                # Avatar persona definitions
│   │   │   ├── __init__.py
│   │   │   ├── helpful_helper.py
│   │   │   ├── curious_newbie.py
│   │   │   └── experienced_churner.py
│   │   └── moderation.py           # Moderation logic
│   │
│   ├── tasks/                      # Celery tasks
│   │   ├── __init__.py
│   │   ├── scraping_tasks.py       # Scraping background tasks
│   │   ├── ai_tasks.py             # AI generation tasks
│   │   ├── newsletter_tasks.py     # Newsletter tasks
│   │   └── maintenance_tasks.py    # Cleanup, updates, etc.
│   │
│   ├── services/                   # Business logic services
│   │   ├── __init__.py
│   │   ├── bonus_service.py
│   │   ├── article_service.py
│   │   ├── newsletter_service.py
│   │   └── rss_service.py
│   │
│   ├── utils/                      # Utility functions
│   │   ├── __init__.py
│   │   ├── pagination.py
│   │   ├── serializers.py
│   │   ├── validators.py
│   │   └── helpers.py
│   │
│   └── templates/                  # Email/RSS templates
│       ├── newsletter_base.html
│       └── rss_feed.xml
│
├── tests/                          # Test suite
│   ├── __init__.py
│   ├── conftest.py                 # Pytest fixtures
│   ├── unit/
│   │   ├── test_models.py
│   │   ├── test_extractors.py
│   │   ├── test_cleaners.py
│   │   └── test_ai_generation.py
│   ├── integration/
│   │   ├── test_api_bonuses.py
│   │   ├── test_api_articles.py
│   │   └── test_scraping_pipeline.py
│   └── fixtures/
│       ├── sample_html/
│       └── sample_data.json
│
├── database/                       # Database scripts
│   ├── init_db.sql                 # Schema initialization
│   ├── seed_data.sql               # Initial seed data
│   └── migrations/                 # Alembic migrations
│       └── versions/
│
├── docker/                         # Docker configuration
│   ├── Dockerfile
│   ├── Dockerfile.celery
│   └── docker-compose.yml
│
├── scripts/                        # Utility scripts
│   ├── init_db.sh
│   ├── run_scraper.py
│   ├── generate_content.py
│   └── deploy.sh
│
├── docs/                           # Documentation
│   ├── plan.md
│   ├── enhanced_plan.md
│   ├── api_documentation.md
│   └── deployment_guide.md
│
├── .env.example                    # Environment variables template
├── .gitignore
├── requirements.txt                # Python dependencies
├── requirements-dev.txt            # Development dependencies
├── setup.py
├── pytest.ini                      # Pytest configuration
├── .flake8                         # Linting configuration
├── README.md
└── run.py                          # Application entry point
```

### **10.2. Configuration Management**

#### **10.2.1. Configuration Classes**

```python
# app/config.py
import os
from datetime import timedelta

class Config:
    """Base configuration"""
    # Flask
    SECRET_KEY = os.getenv('SECRET_KEY', 'dev-secret-key-change-in-production')
    DEBUG = False
    TESTING = False

    # Database
    SQLALCHEMY_DATABASE_URI = os.getenv(
        'DATABASE_URL',
        'postgresql://bankperks:password@localhost:5432/bankperks'
    )
    SQLALCHEMY_TRACK_MODIFICATIONS = False
    SQLALCHEMY_ECHO = False
    SQLALCHEMY_POOL_SIZE = 10
    SQLALCHEMY_MAX_OVERFLOW = 20
    SQLALCHEMY_POOL_TIMEOUT = 30
    SQLALCHEMY_POOL_RECYCLE = 1800

    # Redis
    REDIS_URL = os.getenv('REDIS_URL', 'redis://localhost:6379/0')

    # Celery
    CELERY_BROKER_URL = os.getenv('CELERY_BROKER_URL', REDIS_URL)
    CELERY_RESULT_BACKEND = os.getenv('CELERY_RESULT_BACKEND', REDIS_URL)
    CELERY_TASK_SERIALIZER = 'json'
    CELERY_RESULT_SERIALIZER = 'json'
    CELERY_ACCEPT_CONTENT = ['json']
    CELERY_TIMEZONE = 'UTC'
    CELERY_ENABLE_UTC = True

    # API
    API_TITLE = 'Bankperks API'
    API_VERSION = 'v1'
    API_PREFIX = '/api/v1'
    ITEMS_PER_PAGE = 20
    MAX_ITEMS_PER_PAGE = 100

    # LLM APIs
    OPENAI_API_KEY = os.getenv('OPENAI_API_KEY')
    OPENAI_MODEL = os.getenv('OPENAI_MODEL', 'gpt-4')
    OPENAI_MAX_TOKENS = int(os.getenv('OPENAI_MAX_TOKENS', '2000'))
    OPENAI_TEMPERATURE = float(os.getenv('OPENAI_TEMPERATURE', '0.7'))

    ANTHROPIC_API_KEY = os.getenv('ANTHROPIC_API_KEY')
    ANTHROPIC_MODEL = os.getenv('ANTHROPIC_MODEL', 'claude-3-opus-20240229')

    # Discourse Forum
    DISCOURSE_URL = os.getenv('DISCOURSE_URL', 'https://forum.bankperks.net')
    DISCOURSE_API_KEY = os.getenv('DISCOURSE_API_KEY')
    DISCOURSE_API_USERNAME = os.getenv('DISCOURSE_API_USERNAME', 'system')

    # Email Service (SendGrid, Mailgun, etc.)
    EMAIL_SERVICE = os.getenv('EMAIL_SERVICE', 'sendgrid')
    SENDGRID_API_KEY = os.getenv('SENDGRID_API_KEY')
    MAILGUN_API_KEY = os.getenv('MAILGUN_API_KEY')
    MAILGUN_DOMAIN = os.getenv('MAILGUN_DOMAIN')
    FROM_EMAIL = os.getenv('FROM_EMAIL', 'noreply@bankperks.net')

    # Scraping
    SCRAPING_USER_AGENT = 'BankperksBot/1.0 (+https://bankperks.net/botinfo)'
    SCRAPING_DOWNLOAD_DELAY = 2.0
    SCRAPING_CONCURRENT_REQUESTS = 8
    SCRAPING_CONCURRENT_REQUESTS_PER_DOMAIN = 2
    SCRAPING_RESPECT_ROBOTS_TXT = True

    # Proxy settings (if using proxy service)
    PROXY_ENABLED = os.getenv('PROXY_ENABLED', 'false').lower() == 'true'
    PROXY_URL = os.getenv('PROXY_URL')

    # Content Moderation
    PERSPECTIVE_API_KEY = os.getenv('PERSPECTIVE_API_KEY')
    TOXICITY_THRESHOLD = float(os.getenv('TOXICITY_THRESHOLD', '0.8'))

    # File Upload
    UPLOAD_FOLDER = os.getenv('UPLOAD_FOLDER', 'uploads')
    MAX_CONTENT_LENGTH = 16 * 1024 * 1024  # 16MB

    # Logging
    LOG_LEVEL = os.getenv('LOG_LEVEL', 'INFO')
    LOG_FORMAT = '%(asctime)s - %(name)s - %(levelname)s - %(message)s'
    LOG_FILE = os.getenv('LOG_FILE', 'logs/bankperks.log')

    # Security
    SESSION_COOKIE_SECURE = True
    SESSION_COOKIE_HTTPONLY = True
    SESSION_COOKIE_SAMESITE = 'Lax'
    PERMANENT_SESSION_LIFETIME = timedelta(days=7)

    # CORS
    CORS_ORIGINS = os.getenv('CORS_ORIGINS', '*').split(',')

    # Rate Limiting
    RATELIMIT_ENABLED = True
    RATELIMIT_DEFAULT = "100 per hour"
    RATELIMIT_STORAGE_URL = REDIS_URL

class DevelopmentConfig(Config):
    """Development configuration"""
    DEBUG = True
    SQLALCHEMY_ECHO = True
    SESSION_COOKIE_SECURE = False

class TestingConfig(Config):
    """Testing configuration"""
    TESTING = True
    SQLALCHEMY_DATABASE_URI = 'postgresql://bankperks:password@localhost:5432/bankperks_test'
    CELERY_TASK_ALWAYS_EAGER = True
    CELERY_TASK_EAGER_PROPAGATES = True

class ProductionConfig(Config):
    """Production configuration"""
    DEBUG = False
    TESTING = False
    # Override with production values from environment variables

config = {
    'development': DevelopmentConfig,
    'testing': TestingConfig,
    'production': ProductionConfig,
    'default': DevelopmentConfig
}
```

#### **10.2.2. Environment Variables Template**

```bash
# .env.example
# Copy this file to .env and fill in your actual values

# Flask
FLASK_APP=run.py
FLASK_ENV=development
SECRET_KEY=your-secret-key-here-change-in-production

# Database
DATABASE_URL=postgresql://bankperks:password@localhost:5432/bankperks

# Redis
REDIS_URL=redis://localhost:6379/0

# Celery
CELERY_BROKER_URL=redis://localhost:6379/0
CELERY_RESULT_BACKEND=redis://localhost:6379/0

# OpenAI
OPENAI_API_KEY=sk-your-openai-api-key-here
OPENAI_MODEL=gpt-4
OPENAI_MAX_TOKENS=2000
OPENAI_TEMPERATURE=0.7

# Anthropic (optional, alternative to OpenAI)
ANTHROPIC_API_KEY=your-anthropic-api-key-here
ANTHROPIC_MODEL=claude-3-opus-20240229

# Discourse Forum
DISCOURSE_URL=https://forum.bankperks.net
DISCOURSE_API_KEY=your-discourse-api-key-here
DISCOURSE_API_USERNAME=system

# Email Service (choose one)
EMAIL_SERVICE=sendgrid
SENDGRID_API_KEY=your-sendgrid-api-key-here
# OR
# MAILGUN_API_KEY=your-mailgun-api-key-here
# MAILGUN_DOMAIN=mg.bankperks.net

FROM_EMAIL=noreply@bankperks.net

# Proxy (optional, for scraping)
PROXY_ENABLED=false
PROXY_URL=http://your-proxy-service:port

# Content Moderation
PERSPECTIVE_API_KEY=your-perspective-api-key-here
TOXICITY_THRESHOLD=0.8

# Logging
LOG_LEVEL=INFO
LOG_FILE=logs/bankperks.log

# CORS (comma-separated origins)
CORS_ORIGINS=http://localhost:3000,https://bankperks.net

# Rate Limiting
RATELIMIT_ENABLED=true
RATELIMIT_DEFAULT=100 per hour
```

### **10.3. Dependencies**

#### **10.3.1. Python Requirements**

```txt
# requirements.txt
# Core Framework
Flask==3.0.0
Flask-SQLAlchemy==3.1.1
Flask-Migrate==4.0.5
Flask-CORS==4.0.0
Flask-Limiter==3.5.0

# Database
psycopg2-binary==2.9.9
SQLAlchemy==2.0.23
alembic==1.13.0

# Task Queue
celery==5.3.4
redis==5.0.1
flower==2.0.1

# Web Scraping
Scrapy==2.11.0
playwright==1.40.0
beautifulsoup4==4.12.2
lxml==4.9.3
scrapy-playwright==0.0.34

# Data Processing
pandas==2.1.4
numpy==1.26.2

# NLP/ML
spacy==3.7.2
nltk==3.8.1
transformers==4.36.2
torch==2.1.2

# LLM APIs
openai==1.6.1
anthropic==0.8.1

# HTTP Clients
requests==2.31.0
httpx==0.25.2

# RSS Feed Generation
feedgen==1.0.0

# Email
sendgrid==6.11.0
python-mailgun==0.1.5

# Environment Variables
python-dotenv==1.0.0

# Utilities
python-slugify==8.0.1
python-dateutil==2.8.2
pytz==2023.3
pydantic==2.5.3

# API Documentation
flask-swagger-ui==4.11.1
apispec==6.3.1

# Monitoring
sentry-sdk[flask]==1.39.1

# Testing (in requirements-dev.txt)
# pytest==7.4.3
# pytest-flask==1.3.0
# pytest-cov==4.1.0
# pytest-mock==3.12.0
# factory-boy==3.3.0
# faker==21.0.0
```

#### **10.3.2. Development Requirements**

```txt
# requirements-dev.txt
-r requirements.txt

# Testing
pytest==7.4.3
pytest-flask==1.3.0
pytest-cov==4.1.0
pytest-mock==3.12.0
pytest-asyncio==0.21.1
factory-boy==3.3.0
faker==21.0.0
responses==0.24.1

# Code Quality
flake8==6.1.0
black==23.12.1
isort==5.13.2
mypy==1.7.1
pylint==3.0.3

# Documentation
sphinx==7.2.6
sphinx-rtd-theme==2.0.0

# Debugging
ipython==8.18.1
ipdb==0.13.13

# Database Tools
pgcli==4.0.1
```

### **10.4. Application Entry Point**

```python
# run.py
"""Application entry point"""
import os
from app import create_app
from app.extensions import db
from app.models import Bank, Bonus, AccountType, Article, Newsletter, User

# Create Flask application
app = create_app(os.getenv('FLASK_ENV', 'development'))

# Shell context for flask shell command
@app.shell_context_processor
def make_shell_context():
    return {
        'db': db,
        'Bank': Bank,
        'Bonus': Bonus,
        'AccountType': AccountType,
        'Article': Article,
        'Newsletter': Newsletter,
        'User': User
    }

# CLI commands
@app.cli.command()
def init_db():
    """Initialize the database"""
    db.create_all()
    print('Database initialized!')

@app.cli.command()
def seed_db():
    """Seed the database with initial data"""
    from database.seed_data import seed_account_types, seed_requirement_types
    seed_account_types()
    seed_requirement_types()
    print('Database seeded!')

@app.cli.command()
def test():
    """Run the unit tests"""
    import pytest
    pytest.main(['-v', 'tests/'])

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5000)
```

### **10.5. Flask Application Factory**

```python
# app/__init__.py
"""Flask application factory"""
from flask import Flask
from app.config import config
from app.extensions import db, migrate, cors, limiter
from app.api.errors import register_error_handlers

def create_app(config_name='default'):
    """Create and configure Flask application"""
    app = Flask(__name__)

    # Load configuration
    app.config.from_object(config[config_name])

    # Initialize extensions
    db.init_app(app)
    migrate.init_app(app, db)
    cors.init_app(app, origins=app.config['CORS_ORIGINS'])
    limiter.init_app(app)

    # Register blueprints
    from app.api.v1 import api_v1_bp
    app.register_blueprint(api_v1_bp, url_prefix=app.config['API_PREFIX'])

    # Register error handlers
    register_error_handlers(app)

    # Setup logging
    setup_logging(app)

    # Initialize Celery
    init_celery(app)

    return app

def setup_logging(app):
    """Configure application logging"""
    import logging
    from logging.handlers import RotatingFileHandler
    import os

    if not app.debug and not app.testing:
        # Create logs directory if it doesn't exist
        if not os.path.exists('logs'):
            os.mkdir('logs')

        # File handler
        file_handler = RotatingFileHandler(
            app.config['LOG_FILE'],
            maxBytes=10240000,  # 10MB
            backupCount=10
        )
        file_handler.setFormatter(logging.Formatter(app.config['LOG_FORMAT']))
        file_handler.setLevel(getattr(logging, app.config['LOG_LEVEL']))
        app.logger.addHandler(file_handler)

        app.logger.setLevel(getattr(logging, app.config['LOG_LEVEL']))
        app.logger.info('Bankperks startup')

def init_celery(app):
    """Initialize Celery with Flask app context"""
    from app.tasks import celery

    class ContextTask(celery.Task):
        def __call__(self, *args, **kwargs):
            with app.app_context():
                return self.run(*args, **kwargs)

    celery.Task = ContextTask
    celery.conf.update(app.config)

    return celery

# app/extensions.py
"""Flask extensions"""
from flask_sqlalchemy import SQLAlchemy
from flask_migrate import Migrate
from flask_cors import CORS
from flask_limiter import Limiter
from flask_limiter.util import get_remote_address

db = SQLAlchemy()
migrate = Migrate()
cors = CORS()
limiter = Limiter(
    key_func=get_remote_address,
    default_limits=["100 per hour"]
)
```

### **10.6. Docker Configuration**

#### **10.6.1. Main Dockerfile**

```dockerfile
# docker/Dockerfile
FROM python:3.11-slim

# Set working directory
WORKDIR /app

# Install system dependencies
RUN apt-get update && apt-get install -y \
    gcc \
    postgresql-client \
    libpq-dev \
    curl \
    && rm -rf /var/lib/apt/lists/*

# Install Python dependencies
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# Install Playwright browsers (for scraping)
RUN playwright install chromium
RUN playwright install-deps chromium

# Copy application code
COPY . .

# Create logs directory
RUN mkdir -p logs

# Expose port
EXPOSE 5000

# Run application
CMD ["gunicorn", "--bind", "0.0.0.0:5000", "--workers", "4", "--timeout", "120", "run:app"]
```

#### **10.6.2. Celery Dockerfile**

```dockerfile
# docker/Dockerfile.celery
FROM python:3.11-slim

WORKDIR /app

# Install system dependencies
RUN apt-get update && apt-get install -y \
    gcc \
    postgresql-client \
    libpq-dev \
    && rm -rf /var/lib/apt/lists/*

# Install Python dependencies
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# Install Playwright browsers
RUN playwright install chromium
RUN playwright install-deps chromium

# Copy application code
COPY . .

# Create logs directory
RUN mkdir -p logs

# Run Celery worker
CMD ["celery", "-A", "app.tasks.celery", "worker", "--loglevel=info", "--concurrency=4"]
```

#### **10.6.3. Docker Compose Configuration**

```yaml
# docker-compose.yml
version: '3.8'

services:
  # PostgreSQL Database
  postgres:
    image: postgres:16-alpine
    container_name: bankperks_postgres
    environment:
      POSTGRES_DB: bankperks
      POSTGRES_USER: bankperks
      POSTGRES_PASSWORD: password
    ports:
      - "5432:5432"
    volumes:
      - postgres_data:/var/lib/postgresql/data
      - ./database/init_db.sql:/docker-entrypoint-initdb.d/init_db.sql
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U bankperks"]
      interval: 10s
      timeout: 5s
      retries: 5

  # Redis
  redis:
    image: redis:7-alpine
    container_name: bankperks_redis
    ports:
      - "6379:6379"
    volumes:
      - redis_data:/data
    healthcheck:
      test: ["CMD", "redis-cli", "ping"]
      interval: 10s
      timeout: 5s
      retries: 5

  # Flask Application
  web:
    build:
      context: .
      dockerfile: docker/Dockerfile
    container_name: bankperks_web
    command: gunicorn --bind 0.0.0.0:5000 --workers 4 --timeout 120 run:app
    ports:
      - "5000:5000"
    environment:
      - FLASK_ENV=production
      - DATABASE_URL=postgresql://bankperks:password@postgres:5432/bankperks
      - REDIS_URL=redis://redis:6379/0
      - CELERY_BROKER_URL=redis://redis:6379/0
      - CELERY_RESULT_BACKEND=redis://redis:6379/0
    env_file:
      - .env
    volumes:
      - ./logs:/app/logs
      - ./uploads:/app/uploads
    depends_on:
      postgres:
        condition: service_healthy
      redis:
        condition: service_healthy
    restart: unless-stopped

  # Celery Worker
  celery_worker:
    build:
      context: .
      dockerfile: docker/Dockerfile.celery
    container_name: bankperks_celery_worker
    command: celery -A app.tasks.celery worker --loglevel=info --concurrency=4
    environment:
      - FLASK_ENV=production
      - DATABASE_URL=postgresql://bankperks:password@postgres:5432/bankperks
      - REDIS_URL=redis://redis:6379/0
      - CELERY_BROKER_URL=redis://redis:6379/0
      - CELERY_RESULT_BACKEND=redis://redis:6379/0
    env_file:
      - .env
    volumes:
      - ./logs:/app/logs
    depends_on:
      postgres:
        condition: service_healthy
      redis:
        condition: service_healthy
    restart: unless-stopped

  # Celery Beat (Scheduler)
  celery_beat:
    build:
      context: .
      dockerfile: docker/Dockerfile.celery
    container_name: bankperks_celery_beat
    command: celery -A app.tasks.celery beat --loglevel=info
    environment:
      - FLASK_ENV=production
      - DATABASE_URL=postgresql://bankperks:password@postgres:5432/bankperks
      - REDIS_URL=redis://redis:6379/0
      - CELERY_BROKER_URL=redis://redis:6379/0
      - CELERY_RESULT_BACKEND=redis://redis:6379/0
    env_file:
      - .env
    volumes:
      - ./logs:/app/logs
    depends_on:
      postgres:
        condition: service_healthy
      redis:
        condition: service_healthy
    restart: unless-stopped

  # Flower (Celery Monitoring)
  flower:
    build:
      context: .
      dockerfile: docker/Dockerfile.celery
    container_name: bankperks_flower
    command: celery -A app.tasks.celery flower --port=5555
    ports:
      - "5555:5555"
    environment:
      - CELERY_BROKER_URL=redis://redis:6379/0
      - CELERY_RESULT_BACKEND=redis://redis:6379/0
    depends_on:
      - redis
      - celery_worker
    restart: unless-stopped

volumes:
  postgres_data:
  redis_data:
```

#### **10.6.4. Docker Commands**

```bash
# Build and start all services
docker-compose up -d --build

# View logs
docker-compose logs -f web
docker-compose logs -f celery_worker

# Stop all services
docker-compose down

# Stop and remove volumes (WARNING: deletes data)
docker-compose down -v

# Run database migrations
docker-compose exec web flask db upgrade

# Seed database
docker-compose exec web flask seed-db

# Access Flask shell
docker-compose exec web flask shell

# Run tests
docker-compose exec web pytest

# Restart a specific service
docker-compose restart web
```

---

## **11. Implementation Sequence and Task Breakdown**

This section provides a detailed, ordered sequence of implementation tasks with dependencies, acceptance criteria, and time estimates.

### **11.1. Sprint 0: Project Setup (Week 1)**

#### **Task 1.1: Initialize Project Structure**
- **Dependencies**: None
- **Description**: Create the complete directory structure and initialize Git repository
- **Steps**:
  1. Create all directories as specified in Section 10.1
  2. Initialize Git repository with `.gitignore`
  3. Create `README.md` with project overview
  4. Set up virtual environment: `python -m venv venv`
  5. Activate virtual environment
- **Acceptance Criteria**:
  - All directories exist
  - Git repository initialized
  - Virtual environment created and activated
- **Time Estimate**: 1 hour

#### **Task 1.2: Install Dependencies**
- **Dependencies**: Task 1.1
- **Description**: Install all Python dependencies
- **Steps**:
  1. Create `requirements.txt` from Section 10.3.1
  2. Create `requirements-dev.txt` from Section 10.3.2
  3. Run: `pip install -r requirements-dev.txt`
  4. Download spaCy model: `python -m spacy download en_core_web_sm`
  5. Install Playwright browsers: `playwright install chromium`
- **Acceptance Criteria**:
  - All packages installed without errors
  - `pip list` shows all required packages
  - spaCy model downloaded
  - Playwright browser installed
- **Time Estimate**: 30 minutes

#### **Task 1.3: Setup PostgreSQL Database**
- **Dependencies**: None (can run in parallel with Task 1.2)
- **Description**: Install and configure PostgreSQL
- **Steps**:
  1. Install PostgreSQL 16
  2. Create database: `createdb bankperks`
  3. Create user: `createuser -P bankperks` (password: password)
  4. Grant privileges: `GRANT ALL PRIVILEGES ON DATABASE bankperks TO bankperks;`
  5. Test connection: `psql -U bankperks -d bankperks`
- **Acceptance Criteria**:
  - PostgreSQL running
  - Database `bankperks` exists
  - User `bankperks` can connect
- **Time Estimate**: 30 minutes

#### **Task 1.4: Setup Redis**
- **Dependencies**: None (can run in parallel)
- **Description**: Install and configure Redis
- **Steps**:
  1. Install Redis
  2. Start Redis server: `redis-server`
  3. Test connection: `redis-cli ping` (should return PONG)
- **Acceptance Criteria**:
  - Redis running on port 6379
  - Can connect via redis-cli
- **Time Estimate**: 15 minutes

#### **Task 1.5: Create Configuration Files**
- **Dependencies**: Task 1.1
- **Description**: Create all configuration files
- **Steps**:
  1. Create `app/config.py` from Section 10.2.1
  2. Create `.env.example` from Section 10.2.2
  3. Copy `.env.example` to `.env`
  4. Fill in `.env` with actual values (API keys, etc.)
  5. Create `pytest.ini`, `.flake8` configuration files
- **Acceptance Criteria**:
  - All configuration files exist
  - `.env` file has valid values
  - Configuration can be imported: `from app.config import config`
- **Time Estimate**: 1 hour

#### **Task 1.6: Initialize Flask Application**
- **Dependencies**: Task 1.2, Task 1.5
- **Description**: Create Flask application factory and entry point
- **Steps**:
  1. Create `app/__init__.py` from Section 10.5
  2. Create `app/extensions.py` from Section 10.5
  3. Create `run.py` from Section 10.4
  4. Test: `flask --app run.py --debug run`
  5. Verify Flask starts without errors
- **Acceptance Criteria**:
  - Flask application starts
  - Can access http://localhost:5000
  - No import errors
- **Time Estimate**: 1 hour

### **11.2. Sprint 1: Database Models and API Foundation (Week 1-2)**

#### **Task 2.1: Create Database Schema**
- **Dependencies**: Task 1.3, Task 1.6
- **Description**: Implement complete database schema
- **Steps**:
  1. Create `database/init_db.sql` from Section 9.1
  2. Run: `psql -U bankperks -d bankperks < database/init_db.sql`
  3. Verify all tables created: `\dt` in psql
  4. Verify triggers created: `\df` in psql
  5. Test views: `SELECT * FROM active_bonuses;`
- **Acceptance Criteria**:
  - All 12+ tables exist
  - All indexes created
  - Triggers working (test by updating a record)
  - Views return data
- **Time Estimate**: 2 hours

#### **Task 2.2: Implement SQLAlchemy Models**
- **Dependencies**: Task 2.1
- **Description**: Create all SQLAlchemy model classes
- **Steps**:
  1. Create `app/models/__init__.py` with base mixins from Section 9.2.1
  2. Create `app/models/bank.py` from Section 9.2.2
  3. Create `app/models/account_type.py` from Section 9.2.2
  4. Create `app/models/bonus.py` from Section 9.2.2
  5. Create `app/models/article.py` from Section 9.2.2
  6. Create remaining models (newsletter.py, user.py, scraping_source.py)
  7. Initialize Flask-Migrate: `flask db init`
  8. Create migration: `flask db migrate -m "Initial schema"`
  9. Apply migration: `flask db upgrade`
- **Acceptance Criteria**:
  - All model files exist
  - Models can be imported without errors
  - Can create instances: `bank = Bank(name='Chase')`
  - Migrations run successfully
- **Time Estimate**: 3 hours

#### **Task 2.3: Create Seed Data**
- **Dependencies**: Task 2.2
- **Description**: Create initial seed data for account types and requirement types
- **Steps**:
  1. Create `database/seed_data.py`
  2. Implement `seed_account_types()` function
  3. Implement `seed_requirement_types()` function
  4. Run: `flask seed-db`
  5. Verify data: `SELECT * FROM account_types;`
- **Acceptance Criteria**:
  - 6 account types inserted
  - 10 requirement types inserted
  - Data persists in database
- **Time Estimate**: 1 hour

#### **Task 2.4: Implement Base API Structure**
- **Dependencies**: Task 2.2
- **Description**: Create API blueprint structure and error handlers
- **Steps**:
  1. Create `app/api/__init__.py`
  2. Create `app/api/v1/__init__.py` with blueprint registration
  3. Create `app/api/errors.py` with error handlers
  4. Create `app/api/v1/health.py` with health check endpoint
  5. Test: `curl http://localhost:5000/api/v1/health`
- **Acceptance Criteria**:
  - Health endpoint returns 200
  - Returns JSON: `{"status": "healthy", "timestamp": "...", "version": "1.0.0"}`
  - Error handlers registered
- **Time Estimate**: 2 hours

#### **Task 2.5: Implement Bonus API Endpoints**
- **Dependencies**: Task 2.4
- **Description**: Create complete Bonus CRUD API
- **Steps**:
  1. Create `app/api/v1/bonuses.py`
  2. Implement `GET /api/v1/bonuses` (list with pagination, filtering, sorting)
  3. Implement `GET /api/v1/bonuses/{id}` (get single bonus)
  4. Implement `PATCH /api/v1/bonuses/{id}` (update bonus)
  5. Create `app/utils/pagination.py` helper
  6. Test all endpoints with curl or Postman
- **Acceptance Criteria**:
  - All endpoints return correct status codes
  - Pagination works (page, per_page parameters)
  - Filtering works (status, bank_id, min_amount, etc.)
  - Sorting works (sort=-amount)
  - Response matches OpenAPI schema from Section 9.3
- **Time Estimate**: 4 hours

#### **Task 2.6: Implement Bank and Article API Endpoints**
- **Dependencies**: Task 2.5
- **Description**: Create Bank and Article API endpoints
- **Steps**:
  1. Create `app/api/v1/banks.py`
  2. Implement `GET /api/v1/banks` and `GET /api/v1/banks/{id}`
  3. Implement `GET /api/v1/banks/{id}/bonuses`
  4. Create `app/api/v1/articles.py`
  5. Implement `GET /api/v1/articles` and `GET /api/v1/articles/{slug}`
  6. Test all endpoints
- **Acceptance Criteria**:
  - All endpoints functional
  - Responses match OpenAPI schema
  - Related data properly serialized
- **Time Estimate**: 3 hours

### **11.3. Sprint 2: Web Scraping Infrastructure (Week 2-3)**

#### **Task 3.1: Setup Scrapy Project**
- **Dependencies**: Task 1.2
- **Description**: Initialize Scrapy project structure
- **Steps**:
  1. Navigate to `app/scraping/`
  2. Run: `scrapy startproject scrapy_project`
  3. Configure `scrapy_project/settings.py` with:
     - USER_AGENT from config
     - DOWNLOAD_DELAY = 2.0
     - CONCURRENT_REQUESTS = 8
     - ROBOTSTXT_OBEY = True
     - Enable Playwright middleware
  4. Test: `scrapy list` (should show no spiders yet)
- **Acceptance Criteria**:
  - Scrapy project initialized
  - Settings configured
  - Can run scrapy commands
- **Time Estimate**: 1 hour

#### **Task 3.2: Create Base Spider Class**
- **Dependencies**: Task 3.1
- **Description**: Implement reusable base spider with common functionality
- **Steps**:
  1. Create `app/scraping/scrapy_project/spiders/base_spider.py`
  2. Implement:
     - Retry logic with exponential backoff
     - User-agent rotation
     - Error handling and logging
     - Rate limiting respect
  3. Add helper methods for common parsing tasks
- **Acceptance Criteria**:
  - Base spider class exists
  - Can be imported and subclassed
  - Includes comprehensive error handling
- **Time Estimate**: 2 hours

#### **Task 3.3: Implement Doctor of Credit Spider**
- **Dependencies**: Task 3.2
- **Description**: Create spider for doctorofcredit.com
- **Steps**:
  1. Create `app/scraping/scrapy_project/spiders/doctorofcredit_spider.py`
  2. Extend BaseSpider
  3. Implement parsing logic for:
     - Bonus amount extraction
     - Bank name extraction
     - Requirements extraction
     - Dates extraction
  4. Test on sample pages
  5. Handle pagination
- **Acceptance Criteria**:
  - Spider successfully scrapes Doctor of Credit
  - Extracts all required fields
  - Handles pagination
  - Respects robots.txt
- **Time Estimate**: 4 hours

#### **Task 3.4: Implement Data Extraction Pipeline**
- **Dependencies**: Task 3.3
- **Description**: Create NER and rule-based extractors
- **Steps**:
  1. Create `app/scraping/extractors/base_extractor.py`
  2. Create `app/scraping/extractors/ner_extractor.py`:
     - Use spaCy for entity recognition
     - Extract monetary amounts, dates, organizations
  3. Create `app/scraping/extractors/rule_extractor.py`:
     - Pattern matching for requirements
     - Account type detection
  4. Test extractors on sample HTML
- **Acceptance Criteria**:
  - Extractors successfully identify entities
  - Accuracy > 80% on test data
  - Handles edge cases gracefully
- **Time Estimate**: 5 hours

#### **Task 3.5: Implement Data Cleaning Pipeline**
- **Dependencies**: Task 3.4
- **Description**: Create data cleaning and validation
- **Steps**:
  1. Create `app/scraping/cleaners/bonus_cleaner.py`
  2. Implement cleaning functions:
     - Normalize amounts (remove $, commas)
     - Parse dates (multiple formats)
     - Standardize bank names
     - Validate data completeness
  3. Create `app/scraping/cleaners/validators.py`
  4. Implement validation rules
  5. Test on sample data
- **Acceptance Criteria**:
  - Data cleaned and normalized
  - Invalid data rejected with clear errors
  - Duplicate detection works
- **Time Estimate**: 3 hours

#### **Task 3.6: Implement Scrapy Pipeline**
- **Dependencies**: Task 3.5, Task 2.2
- **Description**: Create Scrapy pipeline to save data to database
- **Steps**:
  1. Create `app/scraping/scrapy_project/pipelines.py`
  2. Implement `BonusPipeline`:
     - Clean data using cleaners
     - Check for duplicates
     - Save to database
     - Update existing records
  3. Configure pipeline in settings.py
  4. Test end-to-end scraping
- **Acceptance Criteria**:
  - Scraped data saved to database
  - Duplicates handled correctly
  - Errors logged appropriately
- **Time Estimate**: 3 hours

### **11.4. Sprint 3: Celery Task Queue (Week 3-4)**

#### **Task 4.1: Setup Celery**
- **Dependencies**: Task 1.4, Task 1.6
- **Description**: Configure Celery with Flask
- **Steps**:
  1. Create `app/tasks/__init__.py`
  2. Initialize Celery instance with Flask app context
  3. Configure Celery beat schedule
  4. Test: Start Celery worker: `celery -A app.tasks.celery worker --loglevel=info`
  5. Test: Start Celery beat: `celery -A app.tasks.celery beat --loglevel=info`
- **Acceptance Criteria**:
  - Celery worker starts without errors
  - Celery beat starts without errors
  - Can see workers in Flower: http://localhost:5555
- **Time Estimate**: 2 hours

#### **Task 4.2: Implement Scraping Tasks**
- **Dependencies**: Task 4.1, Task 3.6
- **Description**: Create Celery tasks for automated scraping
- **Steps**:
  1. Create `app/tasks/scraping_tasks.py`
  2. Implement tasks:
     - `scrape_all_sources()` - scrape all active sources
     - `scrape_source(source_id)` - scrape specific source
     - `discover_new_sources()` - find new websites
  3. Configure periodic tasks in Celery beat schedule
  4. Test tasks manually: `task.delay()`
- **Acceptance Criteria**:
  - Tasks execute successfully
  - Periodic tasks run on schedule
  - Errors handled and logged
- **Time Estimate**: 3 hours

#### **Task 4.3: Implement Maintenance Tasks**
- **Dependencies**: Task 4.1
- **Description**: Create tasks for data maintenance
- **Steps**:
  1. Create `app/tasks/maintenance_tasks.py`
  2. Implement tasks:
     - `expire_old_bonuses()` - mark expired bonuses
     - `verify_active_bonuses()` - re-check bonus validity
     - `cleanup_old_data()` - remove old records
  3. Schedule tasks to run daily
  4. Test tasks
- **Acceptance Criteria**:
  - Tasks run successfully
  - Database updated correctly
  - Logs show task execution
- **Time Estimate**: 2 hours

### **11.5. Sprint 4: AI Content Generation (Week 4-5)**

#### **Task 5.1: Setup LLM Client**
- **Dependencies**: Task 1.2, Task 1.5
- **Description**: Create wrapper for LLM APIs
- **Steps**:
  1. Create `app/ai/llm_client.py`
  2. Implement `LLMClient` class:
     - Support OpenAI and Anthropic
     - Handle API errors and retries
     - Token counting and cost tracking
     - Rate limiting
  3. Test with simple prompts
- **Acceptance Criteria**:
  - Can generate text with OpenAI
  - Can generate text with Anthropic
  - Errors handled gracefully
  - Rate limiting works
- **Time Estimate**: 3 hours

#### **Task 5.2: Create Prompt Templates**
- **Dependencies**: Task 5.1
- **Description**: Design and implement prompt templates
- **Steps**:
  1. Create `app/ai/prompts/article_prompts.py`
  2. Implement templates for:
     - Bonus reviews
     - Comparison articles
     - How-to guides
  3. Include few-shot examples
  4. Test templates with real data
  5. Iterate based on output quality
- **Acceptance Criteria**:
  - Templates generate high-quality content
  - Content is factually accurate
  - Tone is appropriate
  - No hallucinations
- **Time Estimate**: 4 hours

#### **Task 5.3: Implement Content Generator**
- **Dependencies**: Task 5.2, Task 2.2
- **Description**: Create content generation service
- **Steps**:
  1. Create `app/ai/content_generator.py`
  2. Implement `ContentGenerator` class:
     - `generate_article(bonus_ids, article_type)`
     - `generate_newsletter(bonus_ids)`
     - Content validation
     - Save to database with status='review'
  3. Test generation
- **Acceptance Criteria**:
  - Articles generated successfully
  - Content saved to database
  - Metadata tracked (model, prompt, etc.)
- **Time Estimate**: 4 hours

#### **Task 5.4: Implement AI Generation Tasks**
- **Dependencies**: Task 5.3, Task 4.1
- **Description**: Create Celery tasks for AI generation
- **Steps**:
  1. Create `app/tasks/ai_tasks.py`
  2. Implement tasks:
     - `generate_weekly_articles()` - create articles for top bonuses
     - `generate_newsletter()` - create weekly newsletter
  3. Schedule tasks
  4. Test tasks
- **Acceptance Criteria**:
  - Tasks generate content automatically
  - Content requires human review before publishing
  - Errors handled appropriately
- **Time Estimate**: 2 hours

### **11.6. Sprint 5: Testing and Quality Assurance (Week 5-6)**

#### **Task 6.1: Write Unit Tests for Models**
- **Dependencies**: Task 2.2
- **Description**: Create comprehensive unit tests
- **Steps**:
  1. Create `tests/conftest.py` with fixtures
  2. Create `tests/unit/test_models.py`
  3. Test all model methods and properties
  4. Test relationships
  5. Run: `pytest tests/unit/test_models.py -v`
  6. Achieve >90% coverage
- **Acceptance Criteria**:
  - All tests pass
  - Coverage >90%
  - Edge cases tested
- **Time Estimate**: 4 hours

#### **Task 6.2: Write Integration Tests for API**
- **Dependencies**: Task 2.6
- **Description**: Test API endpoints end-to-end
- **Steps**:
  1. Create `tests/integration/test_api_bonuses.py`
  2. Test all bonus endpoints
  3. Test pagination, filtering, sorting
  4. Test error cases (404, 400, etc.)
  5. Create similar tests for banks and articles
  6. Run: `pytest tests/integration/ -v`
- **Acceptance Criteria**:
  - All tests pass
  - All endpoints tested
  - Error cases covered
- **Time Estimate**: 5 hours

#### **Task 6.3: Write Tests for Scraping Pipeline**
- **Dependencies**: Task 3.6
- **Description**: Test scraping and extraction
- **Steps**:
  1. Create `tests/fixtures/sample_html/` with test HTML
  2. Create `tests/unit/test_extractors.py`
  3. Test NER and rule-based extraction
  4. Create `tests/unit/test_cleaners.py`
  5. Test data cleaning and validation
  6. Run tests
- **Acceptance Criteria**:
  - Extractors tested with various HTML structures
  - Cleaners handle edge cases
  - All tests pass
- **Time Estimate**: 4 hours

---

## **12. Testing Strategy and Requirements**

### **12.1. Testing Framework Configuration**

#### **12.1.1. Pytest Configuration**

```ini
# pytest.ini
[pytest]
testpaths = tests
python_files = test_*.py
python_classes = Test*
python_functions = test_*
addopts =
    -v
    --strict-markers
    --cov=app
    --cov-report=html
    --cov-report=term-missing
    --cov-fail-under=80
markers =
    unit: Unit tests
    integration: Integration tests
    slow: Slow running tests
    scraping: Scraping tests (may hit external sites)
```

#### **12.1.2. Test Fixtures**

```python
# tests/conftest.py
import pytest
from app import create_app
from app.extensions import db as _db
from app.models import Bank, Bonus, AccountType, Article
from datetime import date, datetime
from decimal import Decimal

@pytest.fixture(scope='session')
def app():
    """Create application for testing"""
    app = create_app('testing')
    return app

@pytest.fixture(scope='session')
def database(app):
    """Create database schema"""
    with app.app_context():
        _db.create_all()
        yield _db
        _db.drop_all()

@pytest.fixture(scope='function')
def db_session(database):
    """Create a new database session for each test"""
    connection = database.engine.connect()
    transaction = connection.begin()

    session = database.create_scoped_session(
        options={'bind': connection, 'binds': {}}
    )
    database.session = session

    yield session

    transaction.rollback()
    connection.close()
    session.remove()

@pytest.fixture
def client(app, db_session):
    """Create test client"""
    return app.test_client()

@pytest.fixture
def sample_bank(db_session):
    """Create a sample bank"""
    bank = Bank(
        name='Chase',
        slug='chase',
        website_url='https://www.chase.com',
        logo_url='https://cdn.example.com/chase.png',
        description='JPMorgan Chase Bank'
    )
    db_session.add(bank)
    db_session.commit()
    return bank

@pytest.fixture
def sample_account_type(db_session):
    """Create a sample account type"""
    account_type = AccountType(
        name='Checking',
        slug='checking',
        description='Standard checking account'
    )
    db_session.add(account_type)
    db_session.commit()
    return account_type

@pytest.fixture
def sample_bonus(db_session, sample_bank, sample_account_type):
    """Create a sample bonus"""
    bonus = Bonus(
        bank_id=sample_bank.id,
        account_type_id=sample_account_type.id,
        amount=Decimal('300.00'),
        currency='USD',
        title='Chase Total Checking $300 Bonus',
        slug='chase-total-checking-300-bonus',
        description='Earn $300 when you open a new account',
        start_date=date(2025, 1, 1),
        end_date=date(2025, 12, 31),
        status='active',
        source_url='https://www.chase.com/checking',
        source_name='Chase Official',
        requirements=[
            {
                'type': 'direct-deposit',
                'description': 'Set up direct deposit of $500+',
                'amount': 500.00,
                'timeframe_days': 90
            }
        ],
        eligibility={
            'new_customers_only': True,
            'states': ['CA', 'NY', 'TX']
        }
    )
    db_session.add(bonus)
    db_session.commit()
    return bonus

@pytest.fixture
def sample_article(db_session, sample_bonus):
    """Create a sample article"""
    article = Article(
        title='Best Checking Account Bonuses',
        slug='best-checking-account-bonuses',
        content='# Best Checking Account Bonuses\n\nHere are the top bonuses...',
        excerpt='Discover the top checking account bonuses',
        article_type='comparison',
        generated_by_ai=True,
        ai_model='gpt-4',
        status='published',
        published_at=datetime.utcnow(),
        related_bonus_ids=[sample_bonus.id]
    )
    db_session.add(article)
    db_session.commit()
    return article
```

### **12.2. Unit Test Examples**

#### **12.2.1. Model Tests**

```python
# tests/unit/test_models.py
import pytest
from datetime import date, timedelta
from decimal import Decimal
from app.models import Bonus, Bank, AccountType

class TestBonusModel:
    """Test Bonus model"""

    def test_create_bonus(self, db_session, sample_bank, sample_account_type):
        """Test creating a bonus"""
        bonus = Bonus(
            bank_id=sample_bank.id,
            account_type_id=sample_account_type.id,
            amount=Decimal('500.00'),
            title='Test Bonus',
            slug='test-bonus',
            source_url='https://example.com',
            status='active'
        )
        db_session.add(bonus)
        db_session.commit()

        assert bonus.id is not None
        assert bonus.uuid is not None
        assert bonus.amount == Decimal('500.00')
        assert bonus.bank.name == 'Chase'

    def test_bonus_is_active_property(self, sample_bonus):
        """Test is_active property"""
        assert sample_bonus.is_active is True

        # Test expired bonus
        sample_bonus.end_date = date.today() - timedelta(days=1)
        assert sample_bonus.is_active is False

        # Test inactive status
        sample_bonus.end_date = date.today() + timedelta(days=30)
        sample_bonus.status = 'expired'
        assert sample_bonus.is_active is False

    def test_days_until_expiration(self, sample_bonus):
        """Test days_until_expiration property"""
        sample_bonus.end_date = date.today() + timedelta(days=30)
        assert sample_bonus.days_until_expiration == 30

        sample_bonus.end_date = None
        assert sample_bonus.days_until_expiration is None

    def test_bonus_to_dict(self, sample_bonus):
        """Test to_dict serialization"""
        data = sample_bonus.to_dict()

        assert data['id'] == sample_bonus.id
        assert data['uuid'] == sample_bonus.uuid
        assert data['amount'] == 300.00
        assert data['title'] == 'Chase Total Checking $300 Bonus'
        assert 'bank' in data
        assert data['bank']['name'] == 'Chase'
        assert 'account_type' in data
        assert len(data['requirements']) == 1

    def test_bonus_relationships(self, sample_bonus):
        """Test model relationships"""
        assert sample_bonus.bank is not None
        assert sample_bonus.bank.name == 'Chase'
        assert sample_bonus.account_type is not None
        assert sample_bonus.account_type.name == 'Checking'

class TestBankModel:
    """Test Bank model"""

    def test_create_bank(self, db_session):
        """Test creating a bank"""
        bank = Bank(
            name='Wells Fargo',
            slug='wells-fargo',
            website_url='https://www.wellsfargo.com'
        )
        db_session.add(bank)
        db_session.commit()

        assert bank.id is not None
        assert bank.name == 'Wells Fargo'
        assert bank.created_at is not None

    def test_bank_bonuses_relationship(self, sample_bank, sample_bonus):
        """Test bank-bonuses relationship"""
        bonuses = sample_bank.bonuses.all()
        assert len(bonuses) == 1
        assert bonuses[0].id == sample_bonus.id
```

#### **12.2.2. Extractor Tests**

```python
# tests/unit/test_extractors.py
import pytest
from app.scraping.extractors.ner_extractor import NERExtractor
from app.scraping.extractors.rule_extractor import RuleExtractor

class TestNERExtractor:
    """Test NER-based extraction"""

    @pytest.fixture
    def extractor(self):
        return NERExtractor()

    def test_extract_money(self, extractor):
        """Test extracting monetary amounts"""
        text = "Earn $300 bonus when you open an account"
        amounts = extractor.extract_money(text)

        assert len(amounts) > 0
        assert 300.00 in amounts

    def test_extract_dates(self, extractor):
        """Test extracting dates"""
        text = "Offer valid until December 31, 2025"
        dates = extractor.extract_dates(text)

        assert len(dates) > 0
        assert any(d.year == 2025 and d.month == 12 for d in dates)

    def test_extract_organizations(self, extractor):
        """Test extracting bank names"""
        text = "Chase Bank is offering a $300 bonus"
        orgs = extractor.extract_organizations(text)

        assert len(orgs) > 0
        assert any('Chase' in org for org in orgs)

class TestRuleExtractor:
    """Test rule-based extraction"""

    @pytest.fixture
    def extractor(self):
        return RuleExtractor()

    def test_extract_requirements(self, extractor):
        """Test extracting requirements"""
        text = "Set up direct deposit of $500 or more within 90 days"
        requirements = extractor.extract_requirements(text)

        assert len(requirements) > 0
        req = requirements[0]
        assert req['type'] == 'direct-deposit'
        assert req['amount'] == 500.00
        assert req['timeframe_days'] == 90

    def test_extract_account_type(self, extractor):
        """Test detecting account type"""
        text = "Open a new checking account"
        account_type = extractor.extract_account_type(text)

        assert account_type == 'checking'
```

### **12.3. Integration Test Examples**

```python
# tests/integration/test_api_bonuses.py
import pytest
import json
from decimal import Decimal

class TestBonusAPI:
    """Test Bonus API endpoints"""

    def test_list_bonuses(self, client, sample_bonus):
        """Test GET /api/v1/bonuses"""
        response = client.get('/api/v1/bonuses')

        assert response.status_code == 200
        data = json.loads(response.data)

        assert 'data' in data
        assert 'meta' in data
        assert len(data['data']) > 0
        assert data['meta']['total'] > 0

    def test_list_bonuses_with_pagination(self, client, sample_bonus):
        """Test pagination"""
        response = client.get('/api/v1/bonuses?page=1&per_page=10')

        assert response.status_code == 200
        data = json.loads(response.data)

        assert data['meta']['page'] == 1
        assert data['meta']['per_page'] == 10

    def test_list_bonuses_with_filters(self, client, sample_bonus):
        """Test filtering"""
        response = client.get(f'/api/v1/bonuses?bank_id={sample_bonus.bank_id}')

        assert response.status_code == 200
        data = json.loads(response.data)

        assert all(b['bank']['id'] == sample_bonus.bank_id for b in data['data'])

    def test_list_bonuses_with_sorting(self, client, sample_bonus):
        """Test sorting"""
        response = client.get('/api/v1/bonuses?sort=-amount')

        assert response.status_code == 200
        data = json.loads(response.data)

        # Verify descending order
        amounts = [b['amount'] for b in data['data']]
        assert amounts == sorted(amounts, reverse=True)

    def test_get_bonus_by_id(self, client, sample_bonus):
        """Test GET /api/v1/bonuses/{id}"""
        response = client.get(f'/api/v1/bonuses/{sample_bonus.id}')

        assert response.status_code == 200
        data = json.loads(response.data)

        assert data['data']['id'] == sample_bonus.id
        assert data['data']['title'] == sample_bonus.title
        assert 'bank' in data['data']
        assert 'account_type' in data['data']

    def test_get_bonus_by_uuid(self, client, sample_bonus):
        """Test GET /api/v1/bonuses/{uuid}"""
        response = client.get(f'/api/v1/bonuses/{sample_bonus.uuid}')

        assert response.status_code == 200
        data = json.loads(response.data)

        assert data['data']['uuid'] == sample_bonus.uuid

    def test_get_nonexistent_bonus(self, client):
        """Test 404 error"""
        response = client.get('/api/v1/bonuses/99999')

        assert response.status_code == 404
        data = json.loads(response.data)

        assert 'error' in data

    def test_invalid_pagination_parameters(self, client):
        """Test 400 error for invalid parameters"""
        response = client.get('/api/v1/bonuses?page=-1')

        assert response.status_code == 400
```

### **12.4. Coverage Requirements**

- **Minimum Coverage**: 80% overall
- **Critical Modules**: 90% coverage required for:
  - `app/models/`
  - `app/api/`
  - `app/scraping/extractors/`
  - `app/scraping/cleaners/`
- **Excluded from Coverage**:
  - `tests/`
  - `migrations/`
  - `__init__.py` files

### **12.5. Running Tests**

```bash
# Run all tests
pytest

# Run with coverage
pytest --cov=app --cov-report=html

# Run specific test file
pytest tests/unit/test_models.py -v

# Run specific test class
pytest tests/unit/test_models.py::TestBonusModel -v

# Run specific test
pytest tests/unit/test_models.py::TestBonusModel::test_create_bonus -v

# Run tests with specific marker
pytest -m unit
pytest -m integration
pytest -m "not slow"

# Run tests in parallel (faster)
pytest -n auto

# Run tests with verbose output
pytest -vv

# Stop on first failure
pytest -x
```

---

## **13. Error Handling and Edge Cases**

### **13.1. API Error Handling**

```python
# app/api/errors.py
from flask import jsonify
from werkzeug.exceptions import HTTPException

def register_error_handlers(app):
    """Register error handlers for the application"""

    @app.errorhandler(404)
    def not_found_error(error):
        return jsonify({
            'error': 'Not Found',
            'message': 'The requested resource does not exist',
            'status_code': 404
        }), 404

    @app.errorhandler(400)
    def bad_request_error(error):
        return jsonify({
            'error': 'Bad Request',
            'message': str(error),
            'status_code': 400
        }), 400

    @app.errorhandler(500)
    def internal_error(error):
        app.logger.error(f'Internal error: {error}')
        return jsonify({
            'error': 'Internal Server Error',
            'message': 'An unexpected error occurred',
            'status_code': 500
        }), 500

    @app.errorhandler(Exception)
    def handle_exception(error):
        """Handle uncaught exceptions"""
        if isinstance(error, HTTPException):
            return error

        app.logger.error(f'Unhandled exception: {error}', exc_info=True)
        return jsonify({
            'error': 'Internal Server Error',
            'message': 'An unexpected error occurred',
            'status_code': 500
        }), 500
```

### **13.2. Retry Logic with Exponential Backoff**

```python
# app/utils/retry.py
import time
import logging
from functools import wraps

logger = logging.getLogger(__name__)

def retry_with_backoff(max_retries=3, base_delay=1, max_delay=60, exceptions=(Exception,)):
    """
    Decorator for retrying functions with exponential backoff

    Args:
        max_retries: Maximum number of retry attempts
        base_delay: Initial delay in seconds
        max_delay: Maximum delay in seconds
        exceptions: Tuple of exceptions to catch
    """
    def decorator(func):
        @wraps(func)
        def wrapper(*args, **kwargs):
            retries = 0
            while retries < max_retries:
                try:
                    return func(*args, **kwargs)
                except exceptions as e:
                    retries += 1
                    if retries >= max_retries:
                        logger.error(f'{func.__name__} failed after {max_retries} retries: {e}')
                        raise

                    delay = min(base_delay * (2 ** (retries - 1)), max_delay)
                    logger.warning(f'{func.__name__} failed (attempt {retries}/{max_retries}), retrying in {delay}s: {e}')
                    time.sleep(delay)

            return None
        return wrapper
    return decorator

# Usage example
from app.utils.retry import retry_with_backoff
import requests

@retry_with_backoff(max_retries=3, base_delay=2, exceptions=(requests.RequestException,))
def fetch_url(url):
    """Fetch URL with automatic retry"""
    response = requests.get(url, timeout=30)
    response.raise_for_status()
    return response.text
```

### **13.3. Scraping Error Handling**

```python
# app/scraping/utils.py
import logging
from scrapy.exceptions import IgnoreRequest, CloseSpider

logger = logging.getLogger(__name__)

class ScrapingErrorHandler:
    """Handle common scraping errors"""

    @staticmethod
    def handle_blocked_response(response):
        """Handle blocked/captcha responses"""
        if response.status == 403:
            logger.error(f'Blocked by {response.url}: 403 Forbidden')
            raise IgnoreRequest(f'Blocked: {response.url}')

        if 'captcha' in response.text.lower():
            logger.error(f'CAPTCHA detected on {response.url}')
            raise IgnoreRequest(f'CAPTCHA: {response.url}')

    @staticmethod
    def handle_rate_limit(response):
        """Handle rate limiting"""
        if response.status == 429:
            retry_after = response.headers.get('Retry-After', 60)
            logger.warning(f'Rate limited on {response.url}, retry after {retry_after}s')
            raise IgnoreRequest(f'Rate limited: {response.url}')

    @staticmethod
    def handle_consecutive_failures(failures, max_failures=5):
        """Stop spider after too many consecutive failures"""
        if failures >= max_failures:
            logger.error(f'Too many consecutive failures ({failures}), stopping spider')
            raise CloseSpider(f'consecutive_failures={failures}')
```

### **13.4. Database Error Handling**

```python
# app/utils/db_utils.py
from sqlalchemy.exc import IntegrityError, OperationalError
from app.extensions import db
import logging

logger = logging.getLogger(__name__)

def safe_commit():
    """Safely commit database transaction with error handling"""
    try:
        db.session.commit()
        return True
    except IntegrityError as e:
        db.session.rollback()
        logger.error(f'Database integrity error: {e}')
        return False
    except OperationalError as e:
        db.session.rollback()
        logger.error(f'Database operational error: {e}')
        return False
    except Exception as e:
        db.session.rollback()
        logger.error(f'Unexpected database error: {e}')
        return False

def get_or_create(model, **kwargs):
    """Get existing instance or create new one"""
    instance = model.query.filter_by(**kwargs).first()
    if instance:
        return instance, False

    instance = model(**kwargs)
    db.session.add(instance)
    if safe_commit():
        return instance, True
    return None, False
```

---

## **14. Deployment and Operations**

### **14.1. Production Deployment with Docker**

#### **14.1.1. Production Docker Compose**

```yaml
# docker-compose.prod.yml
version: '3.8'

services:
  postgres:
    image: postgres:16-alpine
    container_name: bankperks_postgres_prod
    environment:
      POSTGRES_DB: ${DB_NAME}
      POSTGRES_USER: ${DB_USER}
      POSTGRES_PASSWORD: ${DB_PASSWORD}
    volumes:
      - postgres_data:/var/lib/postgresql/data
    networks:
      - bankperks_network
    restart: always
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U ${DB_USER}"]
      interval: 10s
      timeout: 5s
      retries: 5

  redis:
    image: redis:7-alpine
    container_name: bankperks_redis_prod
    command: redis-server --appendonly yes --requirepass ${REDIS_PASSWORD}
    volumes:
      - redis_data:/data
    networks:
      - bankperks_network
    restart: always

  web:
    build:
      context: .
      dockerfile: docker/Dockerfile
    container_name: bankperks_web_prod
    command: gunicorn --bind 0.0.0.0:5000 --workers 4 --threads 2 --timeout 120 --access-logfile - --error-logfile - run:app
    ports:
      - "5000:5000"
    environment:
      - FLASK_ENV=production
      - DATABASE_URL=postgresql://${DB_USER}:${DB_PASSWORD}@postgres:5432/${DB_NAME}
      - REDIS_URL=redis://:${REDIS_PASSWORD}@redis:6379/0
    env_file:
      - .env.production
    volumes:
      - ./logs:/app/logs
      - ./uploads:/app/uploads
    depends_on:
      postgres:
        condition: service_healthy
      redis:
        condition: service_healthy
    networks:
      - bankperks_network
    restart: always
    deploy:
      resources:
        limits:
          cpus: '2'
          memory: 2G

  celery_worker:
    build:
      context: .
      dockerfile: docker/Dockerfile.celery
    container_name: bankperks_celery_worker_prod
    command: celery -A app.tasks.celery worker --loglevel=info --concurrency=4 --max-tasks-per-child=1000
    environment:
      - FLASK_ENV=production
      - DATABASE_URL=postgresql://${DB_USER}:${DB_PASSWORD}@postgres:5432/${DB_NAME}
      - REDIS_URL=redis://:${REDIS_PASSWORD}@redis:6379/0
    env_file:
      - .env.production
    volumes:
      - ./logs:/app/logs
    depends_on:
      - postgres
      - redis
    networks:
      - bankperks_network
    restart: always
    deploy:
      replicas: 2
      resources:
        limits:
          cpus: '1'
          memory: 1G

  celery_beat:
    build:
      context: .
      dockerfile: docker/Dockerfile.celery
    container_name: bankperks_celery_beat_prod
    command: celery -A app.tasks.celery beat --loglevel=info
    environment:
      - FLASK_ENV=production
      - DATABASE_URL=postgresql://${DB_USER}:${DB_PASSWORD}@postgres:5432/${DB_NAME}
      - REDIS_URL=redis://:${REDIS_PASSWORD}@redis:6379/0
    env_file:
      - .env.production
    volumes:
      - ./logs:/app/logs
    depends_on:
      - postgres
      - redis
    networks:
      - bankperks_network
    restart: always

  nginx:
    image: nginx:alpine
    container_name: bankperks_nginx_prod
    ports:
      - "80:80"
      - "443:443"
    volumes:
      - ./nginx/nginx.conf:/etc/nginx/nginx.conf:ro
      - ./nginx/ssl:/etc/nginx/ssl:ro
      - ./static:/usr/share/nginx/html/static:ro
    depends_on:
      - web
    networks:
      - bankperks_network
    restart: always

networks:
  bankperks_network:
    driver: bridge

volumes:
  postgres_data:
  redis_data:
```

#### **14.1.2. Nginx Configuration**

```nginx
# nginx/nginx.conf
events {
    worker_connections 1024;
}

http {
    upstream bankperks_app {
        server web:5000;
    }

    # Rate limiting
    limit_req_zone $binary_remote_addr zone=api_limit:10m rate=10r/s;
    limit_req_zone $binary_remote_addr zone=general_limit:10m rate=100r/m;

    server {
        listen 80;
        server_name bankperks.net www.bankperks.net;

        # Redirect HTTP to HTTPS
        return 301 https://$server_name$request_uri;
    }

    server {
        listen 443 ssl http2;
        server_name bankperks.net www.bankperks.net;

        # SSL Configuration
        ssl_certificate /etc/nginx/ssl/fullchain.pem;
        ssl_certificate_key /etc/nginx/ssl/privkey.pem;
        ssl_protocols TLSv1.2 TLSv1.3;
        ssl_ciphers HIGH:!aNULL:!MD5;
        ssl_prefer_server_ciphers on;

        # Security Headers
        add_header X-Frame-Options "SAMEORIGIN" always;
        add_header X-Content-Type-Options "nosniff" always;
        add_header X-XSS-Protection "1; mode=block" always;
        add_header Strict-Transport-Security "max-age=31536000; includeSubDomains" always;

        # Logging
        access_log /var/log/nginx/bankperks_access.log;
        error_log /var/log/nginx/bankperks_error.log;

        # Static files
        location /static/ {
            alias /usr/share/nginx/html/static/;
            expires 30d;
            add_header Cache-Control "public, immutable";
        }

        # API endpoints with rate limiting
        location /api/ {
            limit_req zone=api_limit burst=20 nodelay;

            proxy_pass http://bankperks_app;
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header X-Forwarded-Proto $scheme;

            # Timeouts
            proxy_connect_timeout 60s;
            proxy_send_timeout 60s;
            proxy_read_timeout 60s;
        }

        # Health check (no rate limit)
        location /api/v1/health {
            proxy_pass http://bankperks_app;
            access_log off;
        }

        # All other requests
        location / {
            limit_req zone=general_limit burst=50 nodelay;

            proxy_pass http://bankperks_app;
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header X-Forwarded-Proto $scheme;
        }
    }
}
```

### **14.2. Deployment Script**

```bash
# scripts/deploy.sh
#!/bin/bash
set -e

echo "=== Bankperks Deployment Script ==="

# Load environment variables
if [ -f .env.production ]; then
    export $(cat .env.production | grep -v '^#' | xargs)
else
    echo "Error: .env.production file not found"
    exit 1
fi

# Pull latest code
echo "Pulling latest code..."
git pull origin main

# Build Docker images
echo "Building Docker images..."
docker-compose -f docker-compose.prod.yml build

# Stop existing containers
echo "Stopping existing containers..."
docker-compose -f docker-compose.prod.yml down

# Start new containers
echo "Starting new containers..."
docker-compose -f docker-compose.prod.yml up -d

# Wait for database to be ready
echo "Waiting for database..."
sleep 10

# Run database migrations
echo "Running database migrations..."
docker-compose -f docker-compose.prod.yml exec -T web flask db upgrade

# Check health
echo "Checking application health..."
sleep 5
curl -f http://localhost:5000/api/v1/health || exit 1

echo "=== Deployment completed successfully ==="
```

### **14.3. Monitoring and Logging**

#### **14.3.1. Application Monitoring with Sentry**

```python
# app/__init__.py (add to create_app function)
import sentry_sdk
from sentry_sdk.integrations.flask import FlaskIntegration
from sentry_sdk.integrations.celery import CeleryIntegration

def create_app(config_name='default'):
    app = Flask(__name__)
    app.config.from_object(config[config_name])

    # Initialize Sentry
    if app.config.get('SENTRY_DSN'):
        sentry_sdk.init(
            dsn=app.config['SENTRY_DSN'],
            integrations=[
                FlaskIntegration(),
                CeleryIntegration()
            ],
            traces_sample_rate=0.1,
            environment=config_name
        )

    # ... rest of initialization
```

#### **14.3.2. Logging Configuration**

```python
# app/logging_config.py
import logging
import logging.handlers
import os

def setup_logging(app):
    """Configure comprehensive logging"""

    # Create logs directory
    log_dir = 'logs'
    if not os.path.exists(log_dir):
        os.makedirs(log_dir)

    # Main application log
    app_handler = logging.handlers.RotatingFileHandler(
        os.path.join(log_dir, 'app.log'),
        maxBytes=10485760,  # 10MB
        backupCount=10
    )
    app_handler.setLevel(logging.INFO)
    app_handler.setFormatter(logging.Formatter(
        '%(asctime)s %(levelname)s [%(name)s] %(message)s'
    ))

    # Error log
    error_handler = logging.handlers.RotatingFileHandler(
        os.path.join(log_dir, 'error.log'),
        maxBytes=10485760,
        backupCount=10
    )
    error_handler.setLevel(logging.ERROR)
    error_handler.setFormatter(logging.Formatter(
        '%(asctime)s %(levelname)s [%(name)s] %(pathname)s:%(lineno)d\n%(message)s\n'
    ))

    # Scraping log
    scraping_handler = logging.handlers.RotatingFileHandler(
        os.path.join(log_dir, 'scraping.log'),
        maxBytes=10485760,
        backupCount=10
    )
    scraping_handler.setLevel(logging.INFO)

    # Add handlers
    app.logger.addHandler(app_handler)
    app.logger.addHandler(error_handler)

    # Configure scraping logger
    scraping_logger = logging.getLogger('app.scraping')
    scraping_logger.addHandler(scraping_handler)
    scraping_logger.setLevel(logging.INFO)

    app.logger.setLevel(logging.INFO)
```

### **14.4. Backup and Recovery**

```bash
# scripts/backup_db.sh
#!/bin/bash
set -e

BACKUP_DIR="/backups/postgres"
TIMESTAMP=$(date +%Y%m%d_%H%M%S)
BACKUP_FILE="$BACKUP_DIR/bankperks_$TIMESTAMP.sql.gz"

# Create backup directory
mkdir -p $BACKUP_DIR

# Backup database
docker-compose -f docker-compose.prod.yml exec -T postgres pg_dump -U bankperks bankperks | gzip > $BACKUP_FILE

# Keep only last 30 days of backups
find $BACKUP_DIR -name "bankperks_*.sql.gz" -mtime +30 -delete

echo "Backup completed: $BACKUP_FILE"

# scripts/restore_db.sh
#!/bin/bash
set -e

if [ -z "$1" ]; then
    echo "Usage: ./restore_db.sh <backup_file>"
    exit 1
fi

BACKUP_FILE=$1

if [ ! -f "$BACKUP_FILE" ]; then
    echo "Error: Backup file not found: $BACKUP_FILE"
    exit 1
fi

echo "Restoring database from $BACKUP_FILE..."
gunzip < $BACKUP_FILE | docker-compose -f docker-compose.prod.yml exec -T postgres psql -U bankperks bankperks

echo "Database restored successfully"
```

---

## **15. Security Implementation**

### **15.1. API Authentication (JWT)**

```python
# app/auth/jwt_auth.py
from functools import wraps
from flask import request, jsonify
import jwt
from datetime import datetime, timedelta
from app.config import Config

def generate_token(user_id):
    """Generate JWT token"""
    payload = {
        'user_id': user_id,
        'exp': datetime.utcnow() + timedelta(days=7),
        'iat': datetime.utcnow()
    }
    return jwt.encode(payload, Config.SECRET_KEY, algorithm='HS256')

def verify_token(token):
    """Verify JWT token"""
    try:
        payload = jwt.decode(token, Config.SECRET_KEY, algorithms=['HS256'])
        return payload['user_id']
    except jwt.ExpiredSignatureError:
        return None
    except jwt.InvalidTokenError:
        return None

def require_auth(f):
    """Decorator to require authentication"""
    @wraps(f)
    def decorated_function(*args, **kwargs):
        token = request.headers.get('Authorization')

        if not token:
            return jsonify({'error': 'No token provided'}), 401

        if token.startswith('Bearer '):
            token = token[7:]

        user_id = verify_token(token)
        if not user_id:
            return jsonify({'error': 'Invalid or expired token'}), 401

        return f(user_id=user_id, *args, **kwargs)

    return decorated_function
```

### **15.2. API Key Management**

```python
# app/auth/api_key.py
import secrets
from functools import wraps
from flask import request, jsonify
from app.models import APIKey
from app.extensions import db

def generate_api_key():
    """Generate a secure API key"""
    return secrets.token_urlsafe(32)

def require_api_key(f):
    """Decorator to require API key"""
    @wraps(f)
    def decorated_function(*args, **kwargs):
        api_key = request.headers.get('X-API-Key')

        if not api_key:
            return jsonify({'error': 'API key required'}), 401

        key_obj = APIKey.query.filter_by(key=api_key, is_active=True).first()
        if not key_obj:
            return jsonify({'error': 'Invalid API key'}), 401

        # Update last used timestamp
        key_obj.last_used_at = datetime.utcnow()
        db.session.commit()

        return f(api_key_obj=key_obj, *args, **kwargs)

    return decorated_function
```

### **15.3. Input Validation**

```python
# app/utils/validators.py
from pydantic import BaseModel, validator, Field
from typing import Optional, List
from datetime import date
from decimal import Decimal

class BonusCreateSchema(BaseModel):
    """Schema for creating a bonus"""
    bank_id: int = Field(..., gt=0)
    account_type_id: int = Field(..., gt=0)
    amount: Decimal = Field(..., gt=0, max_digits=10, decimal_places=2)
    currency: str = Field(default='USD', min_length=3, max_length=3)
    title: str = Field(..., min_length=1, max_length=500)
    slug: str = Field(..., min_length=1, max_length=500)
    description: Optional[str] = None
    start_date: Optional[date] = None
    end_date: Optional[date] = None
    source_url: str = Field(..., min_length=1, max_length=1000)
    status: str = Field(default='active')

    @validator('status')
    def validate_status(cls, v):
        allowed = ['active', 'expired', 'unverified', 'discontinued']
        if v not in allowed:
            raise ValueError(f'Status must be one of: {allowed}')
        return v

    @validator('end_date')
    def validate_dates(cls, v, values):
        if 'start_date' in values and values['start_date'] and v:
            if v < values['start_date']:
                raise ValueError('end_date must be after start_date')
        return v

def validate_request(schema_class):
    """Decorator to validate request data"""
    def decorator(f):
        @wraps(f)
        def wrapper(*args, **kwargs):
            try:
                data = request.get_json()
                validated_data = schema_class(**data)
                return f(validated_data=validated_data, *args, **kwargs)
            except Exception as e:
                return jsonify({'error': 'Validation error', 'details': str(e)}), 400
        return wrapper
    return decorator
```

---

## **16. Initial Seed Data and Examples**

### **16.1. Initial Scraping Sources**

```python
# database/seed_sources.py
from app.models import ScrapingSource
from app.extensions import db

def seed_scraping_sources():
    """Seed initial scraping sources"""
    sources = [
        {
            'url': 'https://www.doctorofcredit.com/best-bank-account-bonuses/',
            'name': 'Doctor of Credit - Bank Bonuses',
            'source_type': 'aggregator',
            'relevance_score': 95.0,
            'requires_javascript': False,
            'scraping_frequency_hours': 24
        },
        {
            'url': 'https://www.hustlermoneyblog.com/bank-account-bonuses/',
            'name': 'Hustler Money Blog - Bank Bonuses',
            'source_type': 'aggregator',
            'relevance_score': 90.0,
            'requires_javascript': False,
            'scraping_frequency_hours': 24
        },
        {
            'url': 'https://www.mybanktracker.com/bank-promotions',
            'name': 'MyBankTracker - Promotions',
            'source_type': 'aggregator',
            'relevance_score': 85.0,
            'requires_javascript': True,
            'scraping_frequency_hours': 48
        }
    ]

    for source_data in sources:
        source = ScrapingSource(**source_data)
        db.session.add(source)

    db.session.commit()
    print(f'Seeded {len(sources)} scraping sources')
```

### **16.2. Example Prompt Templates**

```python
# app/ai/prompts/article_prompts.py

BONUS_REVIEW_PROMPT = """
You are a financial content writer specializing in bank account bonuses.

Write a comprehensive, informative article reviewing the following bank bonus:

Bank: {bank_name}
Account Type: {account_type}
Bonus Amount: ${amount}
Requirements: {requirements}
Eligibility: {eligibility}

The article should:
1. Start with an engaging introduction
2. Clearly explain the bonus offer
3. Detail all requirements step-by-step
4. Discuss eligibility criteria
5. Provide tips for meeting requirements
6. Include a balanced pros/cons analysis
7. End with a clear recommendation

Tone: Professional, helpful, and trustworthy
Length: 800-1000 words
Format: Markdown with proper headings

Do not include any information not provided above. Be factually accurate.
"""

COMPARISON_ARTICLE_PROMPT = """
You are a financial content writer creating a comparison article.

Compare the following bank bonuses:

{bonuses_list}

Create a comprehensive comparison article that:
1. Introduces all bonuses being compared
2. Creates a comparison table with key metrics
3. Analyzes each bonus in detail
4. Provides recommendations for different user profiles
5. Includes a clear conclusion

Tone: Objective, analytical, helpful
Length: 1200-1500 words
Format: Markdown with tables and headings

Be factually accurate and only use the information provided.
"""

NEWSLETTER_PROMPT = """
You are creating a weekly newsletter about bank account bonuses.

This week's featured bonuses:

{bonuses_list}

Create an engaging newsletter that:
1. Has a catchy subject line
2. Opens with a brief introduction
3. Highlights the top 3-5 bonuses
4. Includes quick tips or insights
5. Ends with a call-to-action

Tone: Friendly, enthusiastic, informative
Format: HTML email format

Keep it concise and scannable.
"""
```

### **16.3. Sample Test Data**

```python
# tests/fixtures/sample_data.py
from decimal import Decimal
from datetime import date, timedelta

SAMPLE_BANKS = [
    {'name': 'Chase', 'slug': 'chase', 'website_url': 'https://www.chase.com'},
    {'name': 'Bank of America', 'slug': 'bank-of-america', 'website_url': 'https://www.bankofamerica.com'},
    {'name': 'Wells Fargo', 'slug': 'wells-fargo', 'website_url': 'https://www.wellsfargo.com'},
    {'name': 'Citibank', 'slug': 'citibank', 'website_url': 'https://www.citibank.com'},
]

SAMPLE_BONUSES = [
    {
        'amount': Decimal('300.00'),
        'title': 'Chase Total Checking $300 Bonus',
        'slug': 'chase-total-checking-300',
        'description': 'Earn $300 when you open a new Chase Total Checking account',
        'start_date': date.today(),
        'end_date': date.today() + timedelta(days=180),
        'source_url': 'https://www.chase.com/checking',
        'requirements': [
            {
                'type': 'direct-deposit',
                'description': 'Set up direct deposit of $500 or more',
                'amount': 500.00,
                'timeframe_days': 90
            }
        ],
        'eligibility': {
            'new_customers_only': True,
            'states': ['CA', 'NY', 'TX', 'FL']
        }
    },
    {
        'amount': Decimal('200.00'),
        'title': 'Bank of America Advantage Banking $200 Bonus',
        'slug': 'bofa-advantage-200',
        'description': 'Get $200 when you open a new Advantage Banking account',
        'start_date': date.today(),
        'end_date': date.today() + timedelta(days=90),
        'source_url': 'https://www.bankofamerica.com/promo',
        'requirements': [
            {
                'type': 'direct-deposit',
                'description': 'Receive qualifying direct deposits totaling $2,000',
                'amount': 2000.00,
                'timeframe_days': 90
            }
        ],
        'eligibility': {
            'new_customers_only': True
        }
    }
]
```

---

## **17. Summary and Next Steps**

### **17.1. Implementation Checklist**

- [ ] **Sprint 0**: Project setup complete
  - [ ] Directory structure created
  - [ ] Dependencies installed
  - [ ] Database and Redis running
  - [ ] Configuration files created
  - [ ] Flask application initialized

- [ ] **Sprint 1**: Database and API foundation
  - [ ] Database schema created
  - [ ] SQLAlchemy models implemented
  - [ ] Seed data loaded
  - [ ] Base API structure created
  - [ ] Bonus, Bank, and Article endpoints implemented

- [ ] **Sprint 2**: Web scraping infrastructure
  - [ ] Scrapy project setup
  - [ ] Base spider implemented
  - [ ] Doctor of Credit spider working
  - [ ] Data extraction pipeline functional
  - [ ] Data cleaning pipeline functional
  - [ ] Scrapy pipeline saving to database

- [ ] **Sprint 3**: Celery task queue
  - [ ] Celery configured with Flask
  - [ ] Scraping tasks implemented
  - [ ] Maintenance tasks implemented
  - [ ] Periodic tasks scheduled

- [ ] **Sprint 4**: AI content generation
  - [ ] LLM client implemented
  - [ ] Prompt templates created
  - [ ] Content generator functional
  - [ ] AI generation tasks scheduled

- [ ] **Sprint 5**: Testing and QA
  - [ ] Unit tests written (>80% coverage)
  - [ ] Integration tests written
  - [ ] Scraping tests written
  - [ ] All tests passing

- [ ] **Sprint 6**: Deployment
  - [ ] Docker configuration complete
  - [ ] Nginx configured
  - [ ] Deployment script tested
  - [ ] Monitoring setup (Sentry)
  - [ ] Backup procedures in place

### **17.2. Success Criteria**

1. **Functional Requirements Met**:
   - System successfully scrapes bank bonus data from multiple sources
   - Data is cleaned, validated, and stored in database
   - REST API provides access to bonus data with filtering and pagination
   - AI generates high-quality articles and newsletters
   - All content goes through review workflow before publication

2. **Quality Requirements Met**:
   - Test coverage >80%
   - All tests passing
   - No critical security vulnerabilities
   - API response time <500ms for 95th percentile
   - Scraping success rate >90%

3. **Operational Requirements Met**:
   - System runs reliably in production
   - Automated backups working
   - Monitoring and alerting configured
   - Documentation complete
   - Deployment process automated

### **17.3. Recommended Development Order**

1. **Week 1**: Complete Sprint 0 and start Sprint 1
2. **Week 2**: Finish Sprint 1 and start Sprint 2
3. **Week 3**: Complete Sprint 2 and start Sprint 3
4. **Week 4**: Finish Sprint 3 and start Sprint 4
5. **Week 5**: Complete Sprint 4 and start Sprint 5
6. **Week 6**: Finish Sprint 5 and complete Sprint 6

**Total Estimated Time**: 6 weeks for core functionality

### **17.4. Post-Launch Tasks**

After initial launch, prioritize:
1. Monitor system performance and errors
2. Gather user feedback
3. Implement Android mobile application
4. Set up Discourse forum with AI avatars
5. Expand scraping sources
6. Improve AI content quality based on feedback
7. Add user authentication and saved bonuses feature
8. Implement RSS feed generation
9. Set up email newsletter distribution

---

## **Conclusion**

This enhanced plan provides comprehensive, actionable specifications for implementing the Bankperks system. Every section includes:

- ✅ Complete database schemas with SQL
- ✅ Python data models with SQLAlchemy
- ✅ Full API specifications in OpenAPI format
- ✅ Detailed project structure
- ✅ Configuration templates
- ✅ Docker deployment setup
- ✅ Step-by-step implementation tasks with dependencies
- ✅ Comprehensive testing strategy with examples
- ✅ Error handling patterns
- ✅ Security implementation
- ✅ Initial seed data and prompt templates

An AI coding agent can now implement this system by following the tasks in order, using the provided code examples, schemas, and specifications as concrete guidance. Each task has clear acceptance criteria and can be verified independently.

The plan maintains consistency with the original technology choices (Python/Flask, PostgreSQL, Scrapy, Celery, etc.) while adding the missing implementation details needed for successful autonomous development.
