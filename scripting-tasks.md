# DevOps Internship - Practical Tasks

## Task 1: Automated System Health Monitor

### Objective
Create a monitoring script that tracks system health and manages logs with Git version control.

### Requirements
1. **Script Features** (bash or Python):
   - Monitor CPU usage, memory usage, and disk space
   - Check if specific services are running (choose 3-5 services like nginx, docker, sshd)
   - Log results with timestamps to `/var/log/health-monitor/` or `~/logs/health-monitor/`
   - Alert (print to console or send email) when:
     - CPU usage > 80%
     - Memory usage > 85%
     - Disk usage > 90% (Name of partition and mount point)
     - Any monitored service is down

2. **Automation**:
   - Configure cron job to run every 5 minutes
   - Create a separate script that commits logs to Git daily at midnight

3. **Git Integration**:
   - Initialize Git repository for the project
   - Create `.gitignore` (exclude large log files, only commit summaries)
   - Daily commit with message format: "Health logs - YYYY-MM-DD"

### Deliverables
- `monitor.sh` or `monitor.py` - main monitoring script
- `git-commit-logs.sh` - script for daily Git commits
- `README.md` - installation and usage instructions
- `crontab.txt` - cron configuration to be added
- Git repository with at least 3 meaningful commits

### Evaluation Criteria
- Script runs without errors
- Proper error handling
- Correct cron syntax
- Meaningful Git commit messages
- Documentation quality

---

## Task 2: Multi-Protocol Web Server Setup

### Objective
Set up a web server with multiple protocols, proper security, and firewall configuration. Use Nginx for internet facing

### Requirements
1. **Web Application**:
   - Create a simple Python web server (Flask or built-in http.server)
   - Support both HTTP and WebSocket connections
   - HTTP endpoint: `/status` returns JSON with server status
   - WebSocket endpoint: `/ws` echoes received messages

2. **Security Configuration**:
   - Generate self-signed SSL certificate
   - Configure HTTPS on port 443 using nginx and proxy-pass
   - HTTP on port 80 redirects to HTTPS using 301(302) or 308(307) response codes

3. **Firewall Setup**:
   - Configure iptables/firewalld rules:
     - Allow ports: 22 (SSH), 80 (HTTP), 443 (HTTPS)
     - Block all other incoming traffic
     - Allow all outgoing traffic
   - Document all firewall rules

4. **WAF Basic Rules**: (Read how can be done)
   - Implement simple request validation
   - Block requests containing SQL injection patterns (`' OR '1'='1`, `UNION SELECT`, etc.)
   - Log blocked requests

5. **Git Version Control**:
   - All code and configuration in Git
   - Include setup scripts
   - Proper `.gitignore` (exclude certificates, virtual environment)

### Deliverables
- `app.py` - Python web application
- `setup-ssl.sh` - script to generate SSL certificates
- `setup-firewall.sh` - script to configure firewall
- `firewall-rules.txt` - documentation of firewall rules
- `README.md` - complete setup and testing instructions
- Git repository with proper structure

### Testing
Provide commands to test:
```bash
# Test HTTP redirect
curl -I http://localhost

# Test HTTPS endpoint
curl -k https://localhost/status

# Test WebSocket
# (provide example using wscat or Python)

# Test WAF blocking
curl -k "https://localhost/status?id=1' OR '1'='1"
```

### Evaluation Criteria
- All protocols working correctly
- Firewall rules properly configured
- WAF blocks malicious patterns
- Clean code structure
- Comprehensive documentation

---

## Task 3: Automated Backup and Deployment Script

### Objective
Build a robust backup solution with remote transfer capabilities and version control.

### Requirements
1. **Backup Script** (bash):
   - Accept parameters: source directory, destination (local or remote)
   - Create compressed tar.gz archive with timestamp: `backup-YYYY-MM-DD-HHMMSS.tar.gz`
   - Support both local and remote (SSH) destinations
   - Test network connectivity before remote backup
   - Verify archive integrity after creation
   - Keep only last 7 backups, delete older ones
   - Understand what is a .tar file and what is a .gz file
   - compare initial size and archive size

2. **Features**:
   - Pre-backup validation (check disk space, verify source exists)
   - Progress indicator for large backups
   - Logging to file and console
   - Email/console notification on success/failure
   - Rollback capability (restore last backup)

3. **Error Handling**:
   - Network failures
   - Insufficient disk space
   - Permission issues
   - Invalid parameters

4. **Git Integration**:
   - Version control for scripts
   - Daily commit of backup logs
   - Exclude actual backup files from Git

### Deliverables
- `backup.sh` - main backup script
- `restore.sh` - restore from backup
- `backup.conf` - configuration file (paths, retention, remote server)
- `README.md` - usage guide with examples
- Sample cron configuration
- Git repository

### Usage Examples
```bash
# Local backup
./backup.sh /home/user/documents /backup/local

# Remote backup
./backup.sh /var/www/html user@backup-server:/backups/web

# Restore
./restore.sh /backup/local/backup-2025-12-10-143022.tar.gz /restore/location
```

### Evaluation Criteria
- Handles all error scenarios gracefully
- Proper SSH key-based authentication
- Backup integrity verification
- Clean logs and notifications
- Code modularity and documentation

---

## Task 4: Network Service Tester

### Objective
Develop a comprehensive network testing tool in Python.

### Requirements
1. **Script Features**:
   - Read endpoints from JSON config file:
     ```json
     {
       "endpoints": [
         {"url": "https://example.com", "type": "https", "expected_status": 200},
         {"url": "http://api.example.com/health", "type": "http"},
         {"url": "grpc://grpc.example.com:50051", "type": "grpc"},
         {"host": "example.com", "type": "dns"}
       ]
     }
     ```
   - Test each endpoint and collect:
     - Response time
     - Status code (HTTP/HTTPS)
     - SSL certificate validity and expiration date
     - DNS resolution time
     - Connection success/failure
     - GRCP is optional

2. **Output**:
   - Generate JSON report with timestamp
   - Generate HTML dashboard (optional but recommended)
   - Print summary to console
   - Color-coded output (green=success, red=failure)

3. **Features**:
   - Concurrent testing (use threading/asyncio)
   - Timeout configuration
   - Retry logic for failed tests
   - Alert on SSL certificates expiring within 30 days

4. **Git Best Practices**:
   - `.gitignore` excludes reports and `__pycache__`
   - Sample config file included
   - Requirements.txt for dependencies

### Deliverables
- `network_tester.py` - main script
- `config.json` - endpoint configuration
- `requirements.txt` - Python dependencies
- `generate_report.py` - HTML report generator (optional)
- `README.md` - installation and usage
- Git repository

### Example Output
```
Network Service Test Report - 2025-12-10 14:30:22
================================================
✓ https://example.com - 200 OK (245ms) - SSL valid until 2026-03-15
✗ http://api.example.com/health - Connection timeout
✓ example.com - DNS resolved (12ms)

Summary: 2 passed, 1 failed
```

### Evaluation Criteria
- Handles various protocols correctly
- Proper error handling
- Clean, readable code
- Useful reports
- Good documentation

---

## Task 5: Firewall Configuration Manager

### Objective
Create a declarative firewall management system.

### Requirements
1. **Configuration Format** (YAML or JSON):
   ```yaml
   rules:
     - name: "Allow SSH"
       port: 22
       protocol: tcp
       action: accept
       source: any
     
     - name: "Allow HTTP"
       port: 80
       protocol: tcp
       action: accept
     
     - name: "Block MySQL from external"
       port: 3306
       protocol: tcp
       action: drop
       source: "!192.168.1.0/24"
   
   default_policy:
     input: drop
     forward: drop
     output: accept
   ```

2. **Script Features** (Python or bash):
   - Parse configuration file
   - Backup existing firewall rules before applying changes
   - Apply rules using iptables or firewalld
   - Validate rules are active
   - Rollback capability if validation fails
   - Log all changes with timestamps

3. **Safety Features**:
   - Dry-run mode (show what would be applied)
   - Confirmation prompt before applying
   - Always preserve SSH access (prevent lockout)
   - Automatic rollback after 60 seconds unless confirmed

4. **Git Integration**:
   - Version control for configurations
   - Track rule changes with meaningful commits
   - Maintain change log

### Deliverables
- `firewall-manager.sh` or `firewall-manager.py`
- `rules.yaml` or `rules.json` - configuration file
- `backup-restore.sh` - manual backup/restore script
- `README.md` - usage guide
- Example rule configurations
- Git repository

### Usage
```bash
# Dry run
./firewall-manager.sh --config rules.yaml --dry-run

# Apply rules
./firewall-manager.sh --config rules.yaml --apply

# Restore backup
./backup-restore.sh --restore /var/backups/firewall-2025-12-10.rules
```

### Evaluation Criteria
- Safe rule application (no lockouts)
- Proper validation
- Good backup/restore mechanism
- Clear error messages
- Documentation quality

---

## Task 6: API Data Operations and Monitoring

### Objective
Build a DevOps automation tool that fetches data from external APIs, processes it, and performs system operations based on the data.

### Requirements
1. **API Integration**:
   - Fetch user data from `https://jsonplaceholder.typicode.com/users`
   - Fetch posts data from `https://jsonplaceholder.typicode.com/posts`
   - Handle API errors and timeouts gracefully

2. **Data Processing Operations**:
   - Parse JSON responses
   - Generate report with:
     - Total number of users
     - Users grouped by company name
     - Top 3 users with most posts
     - List of unique domains from user emails
   - Export processed data to CSV and JSON formats

3. **System Operations** (choose 2-3):
   - Create Linux user accounts based on API data (use username from API)
   - Generate nginx virtual host configs for each user's website domain
   - Create directory structure for each user: `/home/username/{logs,data,backup}`
   - Set appropriate permissions and ownership
   - Generate SSH keys for each user

4. **Automation**:
   - Schedule script to run daily via cron
   - Send email/log summary of operations performed
   - Archive old reports (keep last 30 days)

5. **Monitoring & Logging**:
   - Log all operations to syslog or custom log file
   - Track API response times
   - Alert if API is unreachable
   - Generate daily summary report

6. **Git Version Control**:
   - All scripts in Git
   - Configuration file for API endpoints and settings
   - Proper `.gitignore` (exclude generated reports, logs)
   - README with setup instructions

### Deliverables
- `fetch-and-process.sh` or `fetch-and-process.py` - main script
- `config.yaml` - configuration (API URLs, output paths, email settings)
- `generate-report.sh` - report generation script
- `cleanup-old-reports.sh` - cleanup script
- `README.md` - installation and usage guide
- Sample output files (CSV, JSON)
- Git repository

### Example Output Structure
```
reports/
├── 2025-12-10/
│   ├── users-summary.json
│   ├── users-by-company.csv
│   ├── top-posters.txt
│   └── operations.log

operations/
├── nginx-configs/
│   ├── bret.conf
│   ├── antonette.conf
│   └── ...
└── user-creation.log
```

### Sample Operations
```bash
# Fetch and process
./fetch-and-process.py --config config.yaml

# Generate report only
./fetch-and-process.py --report-only

# Dry run (don't create users/files)
./fetch-and-process.py --dry-run

# Cleanup old reports
./cleanup-old-reports.sh --days 30
```

### Bonus Features (Optional)
- Compare data with previous day and report changes
- Integrate with Slack/Discord webhook for notifications
- Create Grafana-compatible metrics file
- Docker container for the script
- Implement rate limiting for API calls

### Evaluation Criteria
- Robust API error handling
- Clean data processing logic
- Safe system operations (validation, dry-run)
- Proper logging and monitoring
- Automation readiness (cron-friendly)
- Security considerations (permissions, input validation)
- Documentation quality

---

## General Submission Guidelines

### For All Tasks:
1. **Git Repository Structure**:
   ```
   project-name/
   ├── README.md
   ├── .gitignore
   ├── src/
   │   └── (scripts)
   ├── config/
   │   └── (configuration files)
   ├── docs/
   │   └── (additional documentation)
   └── tests/ (optional)
       └── (test scripts)
   ```

2. **README.md Must Include**:
   - Project description
   - Prerequisites
   - Installation steps
   - Usage examples
   - Configuration options
   - Troubleshooting section
   - Author information

3. **Code Quality**:
   - Proper error handling
   - Clear variable names
   - Comments for complex logic
   - Functions for reusable code
   - Exit codes (0=success, non-zero=failure)

4. **Testing**:
   - Provide test cases or testing instructions
   - Include sample data/configurations
   - Document expected outputs
