# Data Analytics Pipeline

Automated ETL (Extract, Transform, Load) pipeline for collecting, processing, and analyzing website traffic data. Ingests data from multiple sources and provides actionable insights through dashboards and SQL queries.

## ✨ Features

- ✅ **Real-time Data Collection** - Pulls data from GA4, Google Analytics, Cloudflare
- ✅ **Automated Pipeline** - Scheduled daily and hourly runs
- ✅ **Data Quality Checks** - Validates data integrity and freshness
- ✅ **Data Warehouse** - Centralized storage in PostgreSQL/BigQuery
- ✅ **Pre-built Dashboards** - Metabase, Looker, and Tableau compatible
- ✅ **SQL Queries Library** - Common analytics queries included
- ✅ **Scalable Architecture** - Handles millions of records
- ✅ **Error Monitoring** - Slack notifications for pipeline failures

## 🔧 Tech Stack

| Component | Technology |
|-----------|------------|
| **Language** | Python 3.8+ |
| **ETL Framework** | Apache Airflow (optional) |
| **Data Processing** | Pandas, NumPy |
| **Database** | PostgreSQL / BigQuery / Snowflake |
| **Visualization** | Metabase / Looker / Tableau |
| **Orchestration** | Docker |
| **Monitoring** | GitHub Actions / CloudWatch |

## 📋 Data Sources

- **Google Analytics 4** (GA4)
- **Google Analytics** (Universal Analytics)
- **Cloudflare** (CDN & Security)
- **Custom Events** (from your applications)
- **Server Logs** (optional)

## 📋 Prerequisites

**Option 1: Docker (Recommended)**
- Docker 20.10+
- Docker Compose 1.29+

**Option 2: Python**
- Python 3.8 or higher
- pip (Python package manager)
- PostgreSQL 12+ (for local data warehouse)

**Option 3: Cloud**
- Google Cloud Project (for BigQuery)
- AWS Account (for S3, Lambda)
- Snowflake Account (for data warehouse)

## 🚀 Quick Start (Docker)

### 1. Clone Repository
```bash
git clone https://github.com/ClearTax/data-analytics-pipeline.git
cd data-analytics-pipeline
```

### 2. Setup Environment
```bash
# Copy environment template
cp .env.example .env

# Edit configuration
nano .env
# or
code .env
```

### 3. Start Pipeline
```bash
# Build Docker image
docker build -t data-pipeline .

# Run pipeline
docker run --env-file .env data-pipeline python src/pipeline.py

# Or use Docker Compose for full stack
docker-compose up
```

### 4. Check Results
```bash
# Connect to database
psql $DATABASE_URL

# View tables
\dt

# Query results
SELECT COUNT(*) FROM processed.daily_traffic_summary;
```

## 🚀 Quick Start (Local Python)

### 1. Clone Repository
```bash
git clone https://github.com/ClearTax/data-analytics-pipeline.git
cd data-analytics-pipeline
```

### 2. Create Virtual Environment
```bash
python3 -m venv venv
source venv/bin/activate
# On Windows: venv\Scripts\activate
```

### 3. Install Dependencies
```bash
pip install -r requirements.txt
```

### 4. Setup Environment
```bash
cp .env.example .env
# Edit .env with your credentials
```

### 5. Initialize Database
```bash
python src/loaders/database.py --create-schema
```

### 6. Run Pipeline
```bash
# Run once
python src/pipeline.py

# Run for specific date
python src/pipeline.py --date 2026-11-01

# Full refresh (slower)
python src/pipeline.py --full-refresh
```

## 📁 Repository Structure

```
data-analytics-pipeline/
├── README.md                    # This file
├── LICENSE                      # MIT License
├── .gitignore                  # Git ignore rules
├── .env.example                # Environment variables template
├── requirements.txt            # Python dependencies
├── Dockerfile                  # Docker configuration
├── docker-compose.yml          # Multi-container setup
│
├── src/                         # Main pipeline code
│   ├── __init__.py
│   ├── config.py              # Configuration loader
│   ├── pipeline.py            # Main pipeline orchestrator
│   │
│   ├── extractors/            # Data extraction layer
│   │   ├── __init__.py
│   │   ├── base_extractor.py  # Abstract base class
│   │   ├── google_analytics.py    # GA (Universal Analytics)
│   │   ├── ga4_extractor.py   # GA4 extraction
│   │   ├── cloudflare.py      # Cloudflare extraction
│   │   └── custom_events.py   # Custom event tracking
│   │
│   ├── transformers/          # Data transformation layer
│   │   ├── __init__.py
│   │   ├── data_cleaner.py    # Clean & validate
│   │   ├── aggregators.py     # Aggregate to summaries
│   │   └── enrichment.py      # Enrich with metadata
│   │
│   ├── loaders/               # Data loading layer
│   │   ├── __init__.py
│   │   ├── database.py        # PostgreSQL loader
│   │   ├── bigquery.py        # BigQuery loader
│   │   └── data_warehouse.py  # Generic warehouse
│   │
│   ├── validation/            # Data quality checks
│   │   ├── __init__.py
│   │   ├── schema_validator.py    # Schema checks
│   │   ├── completeness.py    # Completeness checks
│   │   └── anomaly_detector.py    # Anomaly detection
│   │
│   └── utils/                 # Helper functions
│       ├── __init__.py
│       ├── logger.py          # Logging setup
│       ├── metrics.py         # Monitoring metrics
│       └── errors.py          # Custom exceptions
│
├── tests/                       # Test suite
│   ├── __init__.py
│   ├── test_extractors.py     # Test data extraction
│   ├── test_transformers.py   # Test transformations
│   ├── test_loaders.py        # Test data loading
│   └── test_pipeline.py       # Integration tests
│
├── dags/                        # Airflow DAGs (optional)
│   ├── __init__.py
│   └── daily_pipeline.py      # Daily ETL DAG
│
├── sql/                         # SQL queries
│   ├── schema.sql             # Database schema
│   ├── migrations/            # Migration scripts
│   └── queries/
│       ├── daily_summary.sql  # Daily traffic summary
│       ├── top_pages.sql      # Top performing pages
│       ├── traffic_trends.sql # Traffic analysis
│       └── user_metrics.sql   # User metrics
│
├── notebooks/                   # Jupyter notebooks
│   ├── exploratory_analysis.ipynb
│   └── traffic_insights.ipynb
│
├── dashboards/                  # Dashboard configs
│   ├── metabase_config.json   # Metabase setup
│   ├── looker_specs.json      # Looker specs
│   └── tableau_workbook.twbx  # Tableau workbook
│
├── .github/
│   └── workflows/
│       ├── ci.yml             # CI/CD pipeline
│       └── etl-schedule.yml   # Scheduled ETL runs
│
├── docs/
│   ├── SETUP.md               # Detailed setup
│   ├── PIPELINE.md            # Pipeline architecture
│   ├── DATA_DICTIONARY.md     # Data definitions
│   ├── QUERIES.md             # SQL queries guide
│   └── TROUBLESHOOTING.md     # Common issues
│
└── logs/                        # Pipeline logs
    └── pipeline.log           # Execution logs
```

## 🔐 Environment Variables

Copy `.env.example` to `.env` and configure:

```env
# Data Source: Google Analytics 4
GA4_PROPERTY_ID=123456789
GA4_CREDENTIALS_PATH=./credentials/ga4-key.json

# Data Source: Google Analytics (Universal)
GA_PROPERTY_ID=UA-XXXXXXXXX-X
GA_CREDENTIALS_PATH=./credentials/ga-key.json

# Data Source: Cloudflare
CLOUDFLARE_API_TOKEN=your-api-token-here
CLOUDFLARE_ZONE_ID=your-zone-id
CLOUDFLARE_EMAIL=your-email@example.com

# Database: PostgreSQL (Local)
DATABASE_URL=postgresql://user:password@localhost:5432/analytics_db
DB_HOST=localhost
DB_PORT=5432
DB_USER=postgres
DB_PASSWORD=password
DB_NAME=analytics_db

# Data Warehouse: BigQuery (Cloud)
BIGQUERY_PROJECT_ID=your-gcp-project
BIGQUERY_DATASET=analytics
BIGQUERY_CREDENTIALS_PATH=./credentials/bigquery-key.json

# Data Warehouse: Snowflake
SNOWFLAKE_ACCOUNT=xy12345.us-east-1
SNOWFLAKE_USER=analytics_user
SNOWFLAKE_PASSWORD=password
SNOWFLAKE_WAREHOUSE=COMPUTE_WH
SNOWFLAKE_DATABASE=ANALYTICS
SNOWFLAKE_SCHEMA=PUBLIC

# Pipeline Configuration
PIPELINE_ENV=development
LOG_LEVEL=INFO
BATCH_SIZE=10000
MAX_RETRIES=3

# Notifications
SLACK_WEBHOOK_URL=https://hooks.slack.com/services/YOUR/WEBHOOK/URL
EMAIL_NOTIFICATIONS=true
EMAIL_TO=analytics-team@cleartax.in
```

## 🔄 Running the Pipeline

### Manual Execution
```bash
# Run for today
python src/pipeline.py

# Run for specific date
python src/pipeline.py --date 2026-11-01

# Run with full refresh
python src/pipeline.py --full-refresh

# Dry run (test without loading)
python src/pipeline.py --dry-run

# Run with verbose logging
python src/pipeline.py --verbose
```

### Scheduled Execution (GitHub Actions)

Pipeline runs automatically on schedule defined in `.github/workflows/etl-schedule.yml`:
- Daily at 2:00 AM UTC
- Hourly (optional)

Trigger manual run:
1. Go to GitHub repository
2. Click "Actions" tab
3. Select "ETL Pipeline Schedule"
4. Click "Run workflow"

### Scheduled Execution (Cron)

Add to crontab:
```bash
# Daily at 2 AM
0 2 * * * cd /path/to/pipeline && python src/pipeline.py

# Every 6 hours
0 */6 * * * cd /path/to/pipeline && python src/pipeline.py

# Every hour (frequent)
0 * * * * cd /path/to/pipeline && python src/pipeline.py
```

### Scheduled Execution (Apache Airflow)

```bash
# Initialize Airflow database
airflow db init

# Create admin user
airflow users create --username admin --password admin \
  --firstname Admin --lastname User \
  --role Admin --email admin@example.com

# Start scheduler
airflow scheduler

# Start webserver
airflow webserver --port 8080

# Visit http://localhost:8080
# Username: admin
# Password: admin
```

## 📊 Common Queries

### Daily Traffic Summary
```sql
SELECT 
  date,
  COUNT(*) as total_sessions,
  SUM(pageviews) as total_pageviews,
  ROUND(AVG(session_duration), 2) as avg_duration,
  ROUND(SUM(bounced)::NUMERIC / COUNT(*) * 100, 2) as bounce_rate
FROM processed.daily_traffic_summary
WHERE date >= CURRENT_DATE - 30
GROUP BY date
ORDER BY date DESC;
```

### Top 10 Pages
```sql
SELECT 
  page_path,
  COUNT(*) as pageviews,
  COUNT(DISTINCT session_id) as sessions,
  ROUND(SUM(time_on_page)::NUMERIC / COUNT(*), 2) as avg_time
FROM raw.google_analytics_pageviews
WHERE date >= CURRENT_DATE - 7
GROUP BY page_path
ORDER BY pageviews DESC
LIMIT 10;
```

### Traffic Trends (Last 30 Days)
```sql
SELECT 
  date,
  source,
  medium,
  COUNT(*) as sessions
FROM processed.traffic_by_source
WHERE date >= CURRENT_DATE - 30
GROUP BY date, source, medium
ORDER BY date DESC, sessions DESC;
```

See [sql/queries/](sql/queries/) for more examples.

## 🧪 Testing

```bash
# Run all tests
pytest

# Run specific test file
pytest tests/test_extractors.py -v

# Run with coverage report
pytest --cov=src

# Run integration tests only
pytest tests/ -k integration
```

## 📊 Data Quality

Pipeline includes validation checks:

```bash
# Run validation only
python src/pipeline.py --run-validation-only

# Check data freshness
python src/validation/completeness.py

# Detect anomalies
python src/validation/anomaly_detector.py
```

## 🔍 Monitoring

### View Logs
```bash
# Recent logs
tail -f logs/pipeline.log

# Logs from specific date
grep "2026-11-01" logs/pipeline.log

# Error logs only
grep "ERROR" logs/pipeline.log
```

### Health Check
```bash
# Check pipeline status
python src/pipeline.py --health-check

# Output example:
# ✓ Database: Connected
# ✓ GA4 API: Accessible
# ✓ Cloudflare API: Accessible
# ✓ Last run: 2 hours ago
# ✓ Data freshness: OK
```

### Metrics
```bash
# View pipeline metrics
python -c "from src.utils.metrics import show_metrics; show_metrics()"

# Outputs:
# Total records: 5,234,891
# Processing time: 12m 34s
# Success rate: 99.8%
# Data quality score: 98.2%
```

## 🚀 Deployment

### Local Deployment
```bash
# Start pipeline as background service
nohup python src/pipeline.py > pipeline.log 2>&1 &

# Or with systemd
sudo systemctl enable data-pipeline
sudo systemctl start data-pipeline
```

### Docker Deployment
```bash
# Build image
docker build -t data-pipeline .

# Run container
docker run -d --env-file .env \
  -v $(pwd)/logs:/app/logs \
  --name data-pipeline \
  data-pipeline

# View logs
docker logs -f data-pipeline
```

### Cloud Deployment (Google Cloud)

```bash
# Deploy to Cloud Run
gcloud run deploy data-pipeline \
  --source . \
  --platform managed \
  --region us-central1 \
  --set-env-vars DATABASE_URL=$DATABASE_URL

# Setup Cloud Scheduler
gcloud scheduler jobs create app-engine daily-pipeline \
  --schedule="0 2 * * *" \
  --http-method=POST \
  --uri="https://[region]-[project].cloudfunctions.net/pipeline"
```

## 🐛 Troubleshooting

### "Connection refused" (Database)
```bash
# Check PostgreSQL is running
psql -U postgres

# Or check Docker
docker-compose ps
```

### "API authentication failed"
```bash
# Verify credentials
python -c "from src.extractors.ga4_extractor import GA4; GA4().test_connection()"

# Re-authenticate GA4
python src/extractors/ga4_extractor.py --authenticate
```

### "No data extracted"
```bash
# Check date range
python src/pipeline.py --date 2026-11-01 --verbose

# Verify API has data for that date
# Usually 24-48 hours behind
```

## 📚 Documentation

- [Setup Guide](docs/SETUP.md) - Detailed installation
- [Pipeline Architecture](docs/PIPELINE.md) - How it works
- [Data Dictionary](docs/DATA_DICTIONARY.md) - All tables & fields
- [SQL Queries](docs/QUERIES.md) - Common queries
- [Troubleshooting](docs/TROUBLESHOOTING.md) - Common issues

## 🤝 Contributing

1. **Fork** the repository
2. **Create** feature branch: `git checkout -b feature/new-extractor`
3. **Add tests** for new functionality
4. **Update docs** (README, PIPELINE.md, DATA_DICTIONARY.md)
5. **Commit** changes: `git commit -m 'Add new data source'`
6. **Push** branch: `git push origin feature/new-extractor`
7. **Open** Pull Request

### Code Style
- PEP 8 for Python
- Docstrings for all functions
- Type hints for parameters
- Unit tests for all new extractors/transformers

```bash
# Run linter
flake8 src/

# Format code
black src/

# Check types
mypy src/
```

## 📝 License

MIT License - see [LICENSE](LICENSE) file for details.

## 👥 Team

- **Maintained by**: ClearTax Analytics Team
- **Last Updated**: November 2, 2026
- **Status**: Production Ready

## 📊 Performance

- **Daily run time**: 5-15 minutes
- **Historical backfill**: 1-2 hours
- **Data freshness**: 2-4 hours behind real-time
- **Throughput**: 100K+ records/minute
- **Uptime**: 99.8% (SLA)

## 📞 Support

- **Issues**: [Create issue](https://github.com/ClearTax/data-analytics-pipeline/issues)
- **Discussions**: [Discussions board](https://github.com/ClearTax/data-analytics-pipeline/discussions)
- **Email**: analytics-team@cleartax.in

## 🗓️ Roadmap

- [ ] Real-time streaming (Kafka)
- [ ] Advanced ML anomaly detection
- [ ] Predictive analytics
- [ ] Custom event tracking SDK
- [ ] Mobile app analytics
- [ ] Attribution modeling
- [ ] Customer journey analysis
- [ ] Salesforce CRM integration

---

**Made with ❤️ by ClearTax**

For questions, see [docs/](docs/) folder or [TROUBLESHOOTING.md](docs/TROUBLESHOOTING.md)
