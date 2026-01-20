# Library Management System - Frappe Application

A comprehensive library management system built on Frappe Framework for managing library members, memberships, articles, and transactions.

## 📋 Table of Contents

- [Features](#features)
- [Prerequisites](#prerequisites)
- [Installation & Setup](#installation--setup)
- [Running the Application](#running-the-application)
- [Application Structure](#application-structure)
- [Usage Guide](#usage-guide)
- [Development](#development)
- [Troubleshooting](#troubleshooting)

## ✨ Features

- **Library Member Management**: Create and manage library member profiles
- **Membership System**: Track active memberships with validity periods
- **Article Management**: Manage library articles with web portal support
- **Transaction Tracking**: Issue and return articles with validation
- **Maximum Limit Enforcement**: Configure and enforce maximum issued articles per member
- **Custom Buttons**: Quick actions for creating memberships and transactions
- **Validation Logic**: 
  - Check membership validity before issuing articles
  - Prevent duplicate article issues
  - Validate article availability
  - Enforce maximum article limits

## 🔧 Prerequisites

Before you begin, ensure you have the following installed:

- **Docker Desktop** (Windows/Mac) or **Docker Engine** (Linux)
- **Docker Compose** v2.0 or higher
- **Git**
- At least **4GB RAM** available for Docker
- **10GB free disk space**

## 🚀 Installation & Setup

### Step 1: Clone the Repository

```bash
git clone https://github.com/Brainmetrix/frappe-library-management.git
cd frappe-library-management
```

### Step 2: Navigate to the DevContainer Directory

```bash
cd .devcontainer-example
```

### Step 3: Start Docker Services

```bash
docker-compose up -d
```

This will start the following services:
- **MariaDB** (Database) on port 3306
- **Redis Cache** for caching
- **Redis Queue** for background jobs
- **Frappe Bench** container with the application

### Step 4: Access the Frappe Container

```bash
docker-compose exec frappe bash
```

You should now be inside the Frappe container at `/home/frappe/library-bench`.

### Step 5: Install the Library Management App

If not already installed, run:

```bash
bench get-app library_management
bench --site library.test install-app library_management
```

### Step 6: Start the Development Server

```bash
bench start
```

## 🌐 Running the Application

### Access the Application

Once the bench is running, open your browser and navigate to:

- **Frontend**: http://localhost:8000
- **Default Site**: http://library.test:8000 (add to hosts file if needed)

### Default Login Credentials

- **Username**: Administrator
- **Password**: admin (or the password set during site creation)

### Configure Library Settings

1. Navigate to **Library Settings** from the desk
2. Set the **Maximum Number of Issued Articles** (e.g., 5)
3. Save the settings

## 📁 Application Structure

```
library-bench/
├── apps/
│   ├── frappe/                          # Frappe framework
│   └── library_management/              # Library Management app
│       └── library_management/
│           └── library_management/
│               ├── doctype/
│               │   ├── article/         # Article DocType
│               │   ├── library_member/  # Member DocType
│               │   ├── library_membership/  # Membership DocType
│               │   ├── library_settings/    # Settings DocType
│               │   └── library_transaction/ # Transaction DocType
│               └── workspace/           # Workspace configuration
├── sites/
│   └── library.test/                    # Site directory
└── config/                              # Bench configuration
```

### Key DocTypes

1. **Article** (`article/`)
   - Web-enabled article management
   - Status tracking (Available/Issued)
   - Templates for web display

2. **Library Member** (`library_member/`)
   - Member profile management
   - Custom buttons for quick actions

3. **Library Membership** (`library_membership/`)
   - Membership validity tracking
   - Date-based validation

4. **Library Transaction** (`library_transaction/`)
   - Issue/Return tracking
   - Validation methods:
     - `validate_issue()` - Check membership & availability
     - `validate_return()` - Validate return conditions
     - `validate_membership()` - Check active membership
     - `validate_maximum_limit()` - Enforce article limits

5. **Library Settings** (`library_settings/`)
   - Global configuration
   - Maximum articles limit setting

## 📖 Usage Guide

### Creating a Library Member

1. Go to **Library Member** list
2. Click **New**
3. Fill in member details (First Name, Last Name, Email, Phone)
4. Save

### Creating a Membership

**Method 1: From Library Member**
1. Open a Library Member record
2. Click **Create Membership** button
3. Set From Date and To Date
4. Submit the membership

**Method 2: Direct Creation**
1. Go to **Library Membership** list
2. Click **New**
3. Select Library Member
4. Set dates and submit

### Issuing an Article

**Method 1: From Library Member**
1. Open a Library Member record
2. Click **Create Transaction** button
3. Select Article
4. Set Type as "Issue"
5. Submit

**Method 2: Direct Creation**
1. Go to **Library Transaction** list
2. Click **New**
3. Select Library Member and Article
4. Set Type as "Issue"
5. Submit

The system will validate:
- Active membership exists
- Article is available
- Maximum limit not exceeded

### Returning an Article

1. Create a new **Library Transaction**
2. Select the same Library Member and Article
3. Set Type as "Return"
4. Submit

## 🛠️ Development

### Running in Development Mode

```bash
# Inside the Frappe container
bench start
```

This starts:
- Web server on port 8000
- SocketIO on port 9000
- Background workers
- Scheduler

### Accessing Logs

```bash
# View all logs
tail -f logs/*.log

# View specific log
tail -f logs/frappe.log
```

### Database Access

```bash
# Access MariaDB console
bench --site library.test mariadb
```

### Clearing Cache

```bash
bench --site library.test clear-cache
```

### Migrating Changes

```bash
bench --site library.test migrate
```

### Running Tests

```bash
# Run all tests
bench --site library.test run-tests --app library_management

# Run specific doctype tests
bench --site library.test run-tests --doctype "Library Transaction"
```

## 🐛 Troubleshooting

### Port Already in Use

If port 8000 is already in use:

```bash
# Stop all bench processes
bench stop

# Or change ports in Procfile
```

### Database Connection Issues

```bash
# Restart MariaDB container
docker-compose restart mariadb

# Check database status
docker-compose ps
```

### Permission Issues

```bash
# Fix permissions inside container
bench setup socketio
bench build
```

### Clear Everything and Restart

```bash
# Exit container
exit

# Stop and remove all containers
docker-compose down

# Remove volumes (WARNING: This deletes all data)
docker-compose down -v

# Start fresh
docker-compose up -d
```

### Indentation Errors in Python

If you encounter "Inconsistent use of tabs and spaces" errors:

```bash
# Use autopep8 to fix indentation
pip install autopep8
autopep8 --in-place --aggressive --aggressive <file_path>
```

### Rebuild Assets

```bash
bench build --app library_management
bench clear-cache
```

## 📝 Additional Commands

### Create a New Site

```bash
bench new-site mysite.local --mariadb-root-password 123 --admin-password admin
```

### Backup Site

```bash
bench --site library.test backup
```

### Restore Site

```bash
bench --site library.test restore /path/to/backup
```

### Update Frappe/Apps

```bash
bench update --reset
```

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## 📄 License

This project is licensed under the MIT License.

## 👤 Author

**Vivek Singh**
- Email: vivek.kumar@dhwaniris.com

## 🙏 Acknowledgments

- Built with [Frappe Framework](https://frappeframework.com/)
- Powered by [Docker](https://www.docker.com/)

---

**Note**: This is a development setup. For production deployment, refer to the [Frappe Production Setup Guide](https://frappeframework.com/docs/user/en/production-setup).
