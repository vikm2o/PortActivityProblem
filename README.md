# Take Home Assessment

## Overview

This is a technical assessment designed to evaluate your ability to work with real-world maritime logistics data. You'll be working with actual port activity data to solve practical forecasting problems that shipping companies face daily.

**Time Allocation:**
- Part 1 (ML Problem):
- Part 2 (System Design):

**What to Submit:**
- Jupyter notebook with your analysis (Part 1)
- Architecture diagrams and design document (Part 2)
- README explaining your approach
- Any supporting code/scripts

---

## Dataset

**Port Activity Datataset** Port_Activity_Dataset.csv:


This dataset contains real port activity metrics across multiple ports over time. Before starting, spend time understanding what's in the data, what's missing, and what the quality issues are.


### Dataset Schema

The dataset contains daily port activity records with the following fields:

#### Temporal Fields
- **date**: Timestamp of the observation (format: YYYY/MM/DD HH:MM:SS+TZ)
- **year**: Year (integer)
- **month**: Month (1-12)
- **day**: Day of month (1-31)

#### Location Fields
- **portid**: Unique port identifier (e.g., "port0", "port1")
- **portname**: Human-readable port name (e.g., "Abbot Point", "Singapore")
- **country**: Country where the port is located
- **ISO3**: Three-letter ISO country code (e.g., "AUS", "SGP")

#### Port Call Metrics (Number of Ships Arriving)
These fields count the number of vessels that called at the port on a given day, broken down by vessel type:

- **portcalls_container**: Number of container ships that arrived
- **portcalls_dry_bulk**: Number of dry bulk carriers (coal, grain, ore)
- **portcalls_general_cargo**: Number of general cargo vessels
- **portcalls_roro**: Number of roll-on/roll-off vessels (car carriers, ferries)
- **portcalls_tanker**: Number of tanker vessels (oil, chemicals, LNG)
- **portcalls_cargo**: Total cargo vessel arrivals (sum of above cargo types)
- **portcalls**: Total vessel arrivals (all types)

#### Import Metrics (Estimated Cargo Volume in Tonnes)
These fields estimate the volume of goods imported (unloaded) at the port, by vessel type:

- **import_container**: Cargo volume from container ships (tonnes)
- **import_dry_bulk**: Cargo volume from dry bulk carriers (tonnes)
- **import_general_cargo**: Cargo volume from general cargo vessels (tonnes)
- **import_roro**: Cargo volume from ro-ro vessels (tonnes)
- **import_tanker**: Cargo volume from tankers (tonnes)
- **import_cargo**: Total cargo imports across all cargo vessel types (tonnes)
- **import**: Total import volume (tonnes)

#### Export Metrics (Estimated Cargo Volume in Tonnes)
These fields estimate the volume of goods exported (loaded) at the port, by vessel type:

- **export_container**: Cargo volume to container ships (tonnes)
- **export_dry_bulk**: Cargo volume to dry bulk carriers (tonnes)
- **export_general_cargo**: Cargo volume to general cargo vessels (tonnes)
- **export_roro**: Cargo volume to ro-ro vessels (tonnes)
- **export_tanker**: Cargo volume to tankers (tonnes)
- **export_cargo**: Total cargo exports across all cargo vessel types (tonnes)
- **export**: Total export volume (tonnes)

#### System Field
- **ObjectId**: Unique record identifier (sequential integer)

---

## Part 1: Forecasting Problem

### The Business Problem

A Southeast Asian logistics company needs to optimize their vessel routing and warehouse operations. They've hired you to build a forecasting model that predicts port congestion 7-14 days in advance.

**Why this matters:**
- Ships can be rerouted to less congested ports, saving fuel and time
- Warehouse staff can be scheduled based on expected arrival volumes
- Customers can be notified of potential delays before they happen

### Your Task

Build a forecasting system that predicts port activity levels 7-14 days ahead for the busiest ports in the dataset.

**What we're looking for:**

1. **Data Quality Assessment**
   - Identify and quantify data quality issues
   - Explain which ports have reliable data vs problematic data
   - Document any patterns in missing data or anomalies
   - Show you understand the domain (what drives port activity?)

2. **Feature Engineering**
   - Create meaningful features from the time series
   - Handle missing data appropriately
   - Consider external factors (you may need to bring in additional data like holidays)
   - Explain your reasoning for feature choices

3. **Modeling Approach**
   - Implement at least 3 different forecasting approaches
   - Compare classical time series methods vs machine learning approaches
   - Justify why you chose these specific methods
   - Use proper time-based validation (no data leakage!)

4. **Evaluation**
   - Define success metrics that make sense for the business
   - Analyze where your model works well and where it fails
   - Discuss the trade-offs between different approaches
   - What would you need to improve performance?

**Key Challenges:**

This isn't a clean, well-behaved dataset. You'll need to deal with:
- Missing data and irregular reporting
- Different patterns for different ports
- Seasonal effects and trends
- External shocks (COVID? Port strikes?)
- Limited historical data for some ports



### Deliverables

1. **Jupyter Notebook** with:
   - Thorough exploratory data analysis
   - Feature engineering pipeline
   - Model training and comparison
   - Results analysis and recommendations

2. **Python code** (optional but encouraged):
   - Reusable modules for preprocessing, features, models
   - Should be clean and documented

3. **Brief summary** (in README or notebook):
   - What did you find in the data?
   - What approach did you take and why?
   - What are the limitations?
   - What would you do differently with more time/data?

---

## Part 2: System Design

### The Business Context

Your forecasting prototype from Part 1 worked well. Now the company wants to productionize it as a SaaS platform. They plan to:
- Monitor 200+ ports globally
- Serve 500+ enterprise customers (shipping lines, freight forwarders)
- Provide real-time dashboard and API access
- Send automated alerts when congestion is predicted

### Your Task

Design the production ML system that will power this platform.

**System Requirements:**

**Functional:**
- Ingest daily port activity data from multiple sources
- Generate forecasts for all ports every day
- Provide REST API for forecast queries (<2 second response time)
- Customer dashboard showing forecasts and historical accuracy
- Automated alerts via email/SMS/Slack when thresholds exceeded
- Support A/B testing of new models

**Non-Functional:**
- Handle 200 ports initially, scale to 1,000 ports
- Serve 10,000+ API requests per day
- 99.9% uptime SLA
- Data freshness: forecasts updated by 6 AM daily
- Must be cost-effective (the company is watching burn rate)

**What we're evaluating:**

1. **Data Architecture** (25%)
   - How do you ingest, validate, and store data from multiple sources?
   - What databases/storage do you use for raw data, features, predictions?
   - How do you handle late-arriving or missing data?
   - What's your data retention and versioning strategy?

2. **ML Pipeline** (30%)
   - How do you orchestrate feature engineering and model training?
   - How do you handle training models for 200 different ports?
   - Batch predictions vs real-time inference - what's your strategy?
   - How do you version and deploy models safely?
   - What's your retraining strategy?

3. **Application Layer** (20%)
   - What does the API look like? (endpoints, authentication, rate limiting)
   - How do you design the dashboard? (what do users need to see?)
   - How does the alert system work? (triggers, notifications, escalation)

4. **Monitoring & Reliability** (15%)
   - How do you monitor model performance with delayed labels?
   - How do you detect and handle model drift?
   - What system metrics do you track?
   - What happens when things break? (failover, rollback strategies)

5. **Scalability & Cost** (10%)
   - How does your system scale from 200 to 1,000 ports?
   - What are the cost drivers? (compute, storage, external APIs)
   - Provide a rough monthly cost estimate
   - Where would you optimize to reduce costs?

**Challenges to Address:**

- The dataset is updated daily, but some ports report late
- Different ports have different data quality
- Weather and external events affect predictions but aren't in the dataset
- Customers want explanations for predictions
- False alerts are expensive (unnecessary rerouting costs $2K per ship)
- Must support different forecast horizons (7-day vs 14-day)

### Deliverables

1. **Architecture Diagrams**:
   - High-level system architecture
   - Data pipeline (ingestion to serving)
   - ML training and deployment pipeline
   - Include technology choices on diagrams

2. **Design Document**:
   - System overview and design principles
   - Component details with technology justification
   - Trade-offs and alternative approaches considered
   - Scaling strategy
   - Cost breakdown
   - Failure modes and how you handle them

3. **API Specification** (optional but good to include):
   - Key endpoints with request/response examples
   - Authentication approach


---

## Evaluation Criteria

We're looking for:

**Technical Depth:**
- Do you understand the data and domain?
- Are your modeling choices appropriate and justified?
- Is your system design practical and scalable?

**Problem-Solving:**
- How do you handle messy, real-world data?
- Can you make reasonable trade-offs?
- Do you identify and address edge cases?

**Communication:**
- Can you explain your thinking clearly?
- Do you focus on what matters to the business?
- Are your recommendations actionable?

**Pragmatism:**
- Do you deliver working solutions, not just ideas?
- Do you consider constraints (time, cost, data availability)?
- Do you acknowledge limitations honestly?

---

## Submission Instructions

Create a private GitHub repository with:
```
/
├── README.md                          # Your summary and setup instructions
├── notebooks/
│   └── forecasting_analysis.ipynb    # Part 1 main notebook
├── src/                               # Supporting code (optional)
│   ├── data/
│   ├── features/
│   └── models/
├── design/
│   ├── architecture_diagrams/        # Part 2 diagrams
│   └── design_document.pdf           # Part 2 write-up
└── requirements.txt                   # Dependencies
```

Share access with: [vikm2o]

**Questions?** 
Email: [vikranj@gmail.com]
