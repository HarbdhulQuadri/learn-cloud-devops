# Week 1: Linux, Git, Shell, and AWS Fundamentals

## Learning Objectives
- Master Linux tooling, core networking, Git workflows, and AWS IAM/Billing/CLI
- Build reproducible dev environments and automations
- Understand container fundamentals (namespaces, cgroups)
- Implement secure AWS account setup with proper IAM and billing controls

## Daily Schedule

### Day 1: Linux Mastery and Container Fundamentals
**Duration**: 4 hours

#### Lesson 1.1: Linux Namespaces and cgroups
**Objective**: Understand the building blocks of containers

**Theory**:
- Linux namespaces provide process isolation
- cgroups control resource usage
- These are the foundation of Docker and Kubernetes

**Hands-on Lab**:
```bash
# Explore current namespaces
ls -la /proc/self/ns/

# Create a new network namespace
sudo ip netns add test-ns
sudo ip netns list

# Create a new PID namespace (requires unshare)
sudo unshare --pid --fork --mount-proc /bin/bash
ps aux  # Notice the different process tree

# Explore cgroups
cat /proc/self/cgroup
ls /sys/fs/cgroup/
```

**Exercise**: Create a simple container-like environment using namespaces
```bash
#!/bin/bash
# simple-container.sh
set -e

# Create new namespaces
unshare --pid --fork --mount-proc --net --uts --ipc /bin/bash
```

#### Lesson 1.2: systemd Services and Logs
**Objective**: Master service management and troubleshooting

**Theory**:
- systemd is the init system and service manager
- Services are defined in unit files
- Logs are managed by journald

**Hands-on Lab**:
```bash
# Explore systemd services
systemctl list-units --type=service
systemctl status ssh
systemctl cat ssh

# Create a custom service
sudo tee /etc/systemd/system/my-app.service > /dev/null <<EOF
[Unit]
Description=My Custom Application
After=network.target

[Service]
Type=simple
User=ubuntu
WorkingDirectory=/home/ubuntu/my-app
ExecStart=/home/ubuntu/my-app/start.sh
Restart=always
RestartSec=5

[Install]
WantedBy=multi-user.target
EOF

# Reload and enable
sudo systemctl daemon-reload
sudo systemctl enable my-app
sudo systemctl start my-app
sudo systemctl status my-app
```

**Exercise**: Create a service that monitors disk usage and logs warnings

#### Lesson 1.3: Building Dotfiles
**Objective**: Create reproducible development environment

**Project**: Create a comprehensive dotfiles repository

```bash
# Initialize dotfiles repo
mkdir ~/dotfiles && cd ~/dotfiles
git init

# Create basic structure
mkdir -p {bash,zsh,vim,git,tmux,scripts}

# Create install script
cat > install.sh << 'EOF'
#!/bin/bash
set -e

DOTFILES_DIR="$(cd "$(dirname "${BASH_SOURCE[0]}")" && pwd)"

# Create symlinks
ln -sf "$DOTFILES_DIR/bash/.bashrc" ~/.bashrc
ln -sf "$DOTFILES_DIR/git/.gitconfig" ~/.gitconfig
ln -sf "$DOTFILES_DIR/vim/.vimrc" ~/.vimrc
ln -sf "$DOTFILES_DIR/tmux/.tmux.conf" ~/.tmux.conf

echo "Dotfiles installed successfully!"
EOF

chmod +x install.sh
```

**Deliverable**: Functional dotfiles repo with install script

### Day 2: Networking Fundamentals
**Duration**: 4 hours

#### Lesson 2.1: TCP/IP Basics
**Objective**: Understand network protocols and troubleshooting

**Theory**:
- OSI model and TCP/IP stack
- TCP vs UDP
- Ports and sockets
- DNS resolution

**Hands-on Lab**:
```bash
# Network interface information
ip addr show
ip route show

# Test connectivity
ping -c 4 8.8.8.8
ping -c 4 google.com

# Port scanning
nmap -p 22,80,443 localhost
ss -tulpen  # Modern replacement for netstat

# DNS resolution
dig google.com
dig @8.8.8.8 google.com
nslookup google.com
```

#### Lesson 2.2: TLS and Certificate Management
**Objective**: Understand SSL/TLS and certificate handling

**Hands-on Lab**:
```bash
# Check SSL certificate
openssl s_client -connect google.com:443 -servername google.com

# Generate self-signed certificate
openssl req -x509 -newkey rsa:4096 -keyout key.pem -out cert.pem -days 365 -nodes

# Verify certificate
openssl x509 -in cert.pem -text -noout
```

**Exercise**: Create a simple HTTPS server with self-signed certificate

### Day 3: Git Deep Dive
**Duration**: 4 hours

#### Lesson 3.1: Advanced Git Workflows
**Objective**: Master Git for collaborative development

**Theory**:
- Git internals (objects, refs, index)
- Branching strategies (GitFlow, GitHub Flow, GitLab Flow)
- Merge vs Rebase strategies

**Hands-on Lab**:
```bash
# Explore Git internals
git cat-file -t HEAD
git cat-file -p HEAD
git ls-tree HEAD
git show-branch --all

# Interactive rebase
git rebase -i HEAD~3

# Cherry-picking
git cherry-pick <commit-hash>

# Bisect for finding bugs
git bisect start
git bisect bad HEAD
git bisect good <known-good-commit>
# Test the current commit, then:
git bisect good  # or git bisect bad
```

#### Lesson 3.2: Git Hooks and Automation
**Objective**: Automate Git workflows

**Project**: Set up comprehensive Git hooks

```bash
# Pre-commit hook
cat > .git/hooks/pre-commit << 'EOF'
#!/bin/bash
set -e

# Run linters
if command -v flake8 &> /dev/null; then
    flake8 --max-line-length=88 --extend-ignore=E203,W503 .
fi

if command -v black &> /dev/null; then
    black --check .
fi

# Check for large files
if git rev-parse --verify HEAD >/dev/null 2>&1; then
    against=HEAD
else
    against=$(git hash-object -t tree /dev/null)
fi

git diff --cached --name-only --diff-filter=A | while read -r file; do
    if [ -f "$file" ] && [ "$(stat -c%s "$file")" -gt 10485760 ]; then
        echo "Error: $file is larger than 10MB"
        exit 1
    fi
done
EOF

chmod +x .git/hooks/pre-commit
```

**Exercise**: Create a commit-msg hook that enforces conventional commits

### Day 4: AWS Setup and IAM
**Duration**: 4 hours

#### Lesson 4.1: AWS Account Structure
**Objective**: Set up secure, cost-controlled AWS environment

**Theory**:
- AWS Organizations and account structure
- IAM users, groups, roles, and policies
- Multi-factor authentication
- Cost management and billing

**Hands-on Lab**:
```bash
# Install AWS CLI v2
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install

# Configure AWS CLI
aws configure
# Enter: Access Key ID, Secret Access Key, Region, Output format

# Test configuration
aws sts get-caller-identity

# Create IAM user for programmatic access
aws iam create-user --user-name devops-engineer
aws iam attach-user-policy --user-name devops-engineer --policy-arn arn:aws:iam::aws:policy/PowerUserAccess
aws iam create-access-key --user-name devops-engineer
```

#### Lesson 4.2: IAM Policies and Roles
**Objective**: Implement least-privilege access

**Project**: Create custom IAM policies

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "ec2:Describe*",
                "ec2:RunInstances",
                "ec2:TerminateInstances"
            ],
            "Resource": "*",
            "Condition": {
                "StringEquals": {
                    "ec2:Region": "us-west-2"
                }
            }
        }
    ]
}
```

**Exercise**: Create a policy that allows only specific EC2 instance types

### Day 5: Shell Automation
**Duration**: 4 hours

#### Lesson 5.1: Advanced Bash Scripting
**Objective**: Write production-ready shell scripts

**Theory**:
- Error handling and exit codes
- Input validation and sanitization
- Logging and debugging
- Idempotent operations

**Hands-on Lab**:
```bash
#!/bin/bash
# advanced-script.sh
set -euo pipefail  # Exit on error, undefined vars, pipe failures

# Configuration
SCRIPT_DIR="$(cd "$(dirname "${BASH_SOURCE[0]}")" && pwd)"
LOG_FILE="${SCRIPT_DIR}/script.log"
LOCK_FILE="/tmp/script.lock"

# Logging function
log() {
    echo "[$(date '+%Y-%m-%d %H:%M:%S')] $*" | tee -a "$LOG_FILE"
}

# Error handling
error_exit() {
    log "ERROR: $1"
    rm -f "$LOCK_FILE"
    exit 1
}

# Cleanup on exit
cleanup() {
    rm -f "$LOCK_FILE"
}
trap cleanup EXIT

# Check for lock file
if [ -f "$LOCK_FILE" ]; then
    error_exit "Script is already running"
fi

# Create lock file
echo $$ > "$LOCK_FILE"

# Main script logic
log "Starting script execution"
# ... your script logic here ...
log "Script completed successfully"
```

#### Lesson 5.2: Makefile for Automation
**Objective**: Create maintainable build and deployment scripts

**Project**: Create a comprehensive Makefile

```makefile
# Makefile
.PHONY: help install test lint format clean deploy

# Default target
help: ## Show this help message
	@echo 'Usage: make [target]'
	@echo ''
	@echo 'Targets:'
	@awk 'BEGIN {FS = ":.*?## "} /^[a-zA-Z_-]+:.*?## / {printf "  %-15s %s\n", $$1, $$2}' $(MAKEFILE_LIST)

# Variables
PYTHON := python3
PIP := pip3
VENV := venv
REQUIREMENTS := requirements.txt

# Installation
install: ## Install dependencies
	$(PYTHON) -m venv $(VENV)
	$(VENV)/bin/$(PIP) install -r $(REQUIREMENTS)

# Testing
test: ## Run tests
	$(VENV)/bin/pytest tests/ -v --cov=src/

# Linting
lint: ## Run linters
	$(VENV)/bin/flake8 src/ tests/
	$(VENV)/bin/black --check src/ tests/
	$(VENV)/bin/mypy src/

# Formatting
format: ## Format code
	$(VENV)/bin/black src/ tests/
	$(VENV)/bin/isort src/ tests/

# Cleanup
clean: ## Clean up generated files
	rm -rf $(VENV)/
	rm -rf __pycache__/
	rm -rf .pytest_cache/
	rm -rf .coverage
	find . -name "*.pyc" -delete

# Deployment
deploy: test lint ## Deploy application
	@echo "Deploying to production..."
	# Add deployment commands here
```

**Exercise**: Add Docker build and push targets to the Makefile

### Day 6: Project Day - AWS Onboarding Kit
**Duration**: 4 hours

#### Project: Secure AWS Onboarding Kit
**Objective**: Create a production-ready AWS setup automation

**Deliverable**: Complete AWS onboarding with budget alarms and CLI profiles

**Project Structure**:
```
cloud-foundations-aws/
├── README.md
├── Makefile
├── scripts/
│   ├── bootstrap_aws_user.sh
│   ├── create_budget_alarm.sh
│   ├── setup_mfa.sh
│   └── audit_iam.sh
├── iam/
│   ├── policies/
│   │   ├── devops-policy.json
│   │   └── readonly-policy.json
│   └── roles/
│       └── cross-account-role.json
├── terraform/
│   ├── budget-alarms.tf
│   └── iam-users.tf
└── .github/workflows/
    └── security-audit.yml
```

**Key Components**:

1. **Bootstrap Script** (`scripts/bootstrap_aws_user.sh`):
```bash
#!/bin/bash
set -euo pipefail

# Bootstrap AWS user with proper permissions
USER_NAME="${1:-devops-engineer}"
POLICY_ARN="${2:-arn:aws:iam::aws:policy/PowerUserAccess}"

# Create user
aws iam create-user --user-name "$USER_NAME" || true

# Attach policy
aws iam attach-user-policy --user-name "$USER_NAME" --policy-arn "$POLICY_ARN"

# Create access key
ACCESS_KEY=$(aws iam create-access-key --user-name "$USER_NAME" --output json)
echo "$ACCESS_KEY" | jq -r '.AccessKey | "AWS_ACCESS_KEY_ID=\(.AccessKeyId)\nAWS_SECRET_ACCESS_KEY=\(.SecretAccessKey)"'

# Enable MFA
aws iam create-virtual-mfa-device --virtual-mfa-device-name "${USER_NAME}-mfa" --outfile QRCode.png --bootstrap-method QRCodePNG
```

2. **Budget Alarm Script** (`scripts/create_budget_alarm.sh`):
```bash
#!/bin/bash
set -euo pipefail

BUDGET_AMOUNT="${1:-100}"
EMAIL="${2:-admin@company.com}"

# Create budget
aws budgets create-budget \
    --account-id "$(aws sts get-caller-identity --query Account --output text)" \
    --budget '{
        "BudgetName": "Monthly-Budget",
        "BudgetLimit": {
            "Amount": "'$BUDGET_AMOUNT'",
            "Unit": "USD"
        },
        "TimeUnit": "MONTHLY",
        "BudgetType": "COST",
        "CostFilters": {},
        "TimePeriod": {
            "Start": "'$(date -u +%Y-%m-01T00:00:00Z)'",
            "End": "2027-12-31T23:59:59Z"
        }
    }'

# Create alarm
aws budgets create-notification \
    --account-id "$(aws sts get-caller-identity --query Account --output text)" \
    --budget-name "Monthly-Budget" \
    --notification '{
        "NotificationType": "ACTUAL",
        "ComparisonOperator": "GREATER_THAN",
        "Threshold": 80,
        "ThresholdType": "PERCENTAGE"
    }' \
    --subscribers '[{
        "SubscriptionType": "EMAIL",
        "Address": "'$EMAIL'"
    }]'
```

3. **Terraform Configuration** (`terraform/budget-alarms.tf`):
```hcl
resource "aws_budgets_budget" "monthly" {
  name         = "monthly-budget"
  budget_type  = "COST"
  limit_amount = "100"
  limit_unit   = "USD"
  time_unit    = "MONTHLY"

  cost_filters = {
    Tag = [
      "Environment:Production",
    ]
  }

  notification {
    comparison_operator        = "GREATER_THAN"
    threshold                 = 80
    threshold_type            = "PERCENTAGE"
    notification_type         = "ACTUAL"
    subscriber_email_addresses = [var.admin_email]
  }
}
```

**Validation Tests**:
```bash
# Test IAM setup
aws iam get-user --user-name devops-engineer
aws iam list-attached-user-policies --user-name devops-engineer

# Test budget alarms
aws budgets describe-budgets --account-id $(aws sts get-caller-identity --query Account --output text)

# Test MFA
aws iam list-mfa-devices --user-name devops-engineer
```

## Self-Assessment Checkpoints

### Technical Skills
- [ ] Can you explain the difference between namespaces and cgroups?
- [ ] Can you create and manage systemd services?
- [ ] Can you troubleshoot network connectivity issues?
- [ ] Can you perform advanced Git operations (rebase, bisect, cherry-pick)?
- [ ] Can you write idempotent shell scripts with proper error handling?

### AWS Skills
- [ ] Can you set up IAM users with least-privilege access?
- [ ] Can you create and manage budget alarms?
- [ ] Can you configure MFA for AWS accounts?
- [ ] Can you audit IAM permissions and identify security issues?

### Automation Skills
- [ ] Can you create maintainable Makefiles?
- [ ] Can you set up Git hooks for code quality?
- [ ] Can you write scripts that handle errors gracefully?

## Interview Prep Questions

### Linux and Containers
1. **Q**: What's the difference between a namespace and a cgroup?
   **A**: Namespaces provide process isolation (PID, network, mount, etc.), while cgroups control resource usage (CPU, memory, I/O). Together they form the foundation of containers.

2. **Q**: How would you troubleshoot a service that's failing to start?
   **A**: Check `systemctl status`, `journalctl -u service-name`, examine the service file with `systemctl cat`, check file permissions and dependencies.

### Git
1. **Q**: When would you use rebase vs merge?
   **A**: Rebase for clean, linear history (feature branches), merge for preserving branch context (main branches, collaborative features).

2. **Q**: How do you find which commit introduced a bug?
   **A**: Use `git bisect` to binary search through commits, or `git log --oneline --grep="keyword"` to search commit messages.

### AWS IAM
1. **Q**: What's the difference between a user and a role?
   **A**: Users have permanent credentials, roles are assumed temporarily. Roles are preferred for applications and cross-account access.

2. **Q**: How do you implement least-privilege access?
   **A**: Start with minimal permissions, use resource-based policies, implement conditions, regularly audit with tools like AWS Access Analyzer.

## Weekly Deliverables

1. **Dotfiles Repository**: Complete development environment setup
2. **AWS Onboarding Kit**: Automated AWS account setup with security controls
3. **Git Hooks**: Automated code quality checks
4. **Shell Scripts**: Production-ready automation scripts
5. **Documentation**: Comprehensive README files with usage examples

## Next Week Preview

Week 2 will focus on Docker and container security, building on the Linux fundamentals from this week. You'll learn to create secure, minimal container images and implement supply chain security practices.
