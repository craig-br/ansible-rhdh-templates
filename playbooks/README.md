# Ansible Playbooks Collection

This directory contains a collection of Ansible playbooks designed to be executed as Job Templates in Ansible Automation Platform (AAP). Each playbook serves different operational purposes and includes interactive prompts for customization.

## 📑 Table of Contents

### Quick Reference
- [📁 Playbook Inventory](#-playbook-inventory)
- [🚀 Quick Start](#-quick-start)

### Playbook Documentation
- [📖 Hello World Playbook](#hello-world-playbook)
  - [Description](#description)
  - [Features](#features)
  - [Usage](#usage)
  - [Expected Output](#expected-output)
  - [Use Cases](#use-cases)
- [📖 System Information Gathering Playbook](#system-information-gathering-playbook)
  - [Description](#description-1)
  - [Interactive Parameters](#interactive-parameters)
  - [Features](#features-1)
  - [Usage](#usage-1)
  - [Manual Execution](#manual-execution)
  - [Sample Output Report](#sample-output-report)
  - [Use Cases](#use-cases-1)
- [📖 File Operations Playbook](#file-operations-playbook)
  - [Description](#description-2)
  - [Interactive Parameters](#interactive-parameters-1)
  - [Available Operations](#available-operations)
    - [1. Create File/Directory](#1-create-filedirectory)
    - [2. Copy File/Directory](#2-copy-filedirectory)
    - [3. Move/Rename File/Directory](#3-moverename-filedirectory)
    - [4. Delete File/Directory](#4-delete-filedirectory)
    - [5. Change Permissions](#5-change-permissions)
    - [6. Search for Files](#6-search-for-files)
  - [Safety Features](#safety-features)
  - [Usage](#usage-2)
  - [Manual Execution Examples](#manual-execution-examples)
  - [Use Cases](#use-cases-2)

### Integration & Operations
- [🔧 AAP Integration](#-aap-integration)
  - [Job Template Setup](#job-template-setup)
  - [Execution Best Practices](#execution-best-practices)
  - [Monitoring and Logging](#monitoring-and-logging)

### Support & Maintenance
- [🛠️ Troubleshooting](#️-troubleshooting)
  - [Common Issues](#common-issues)
  - [Error Messages](#error-messages)
- [📚 Additional Resources](#-additional-resources)
- [🔄 Version History](#-version-history)

---

## 📁 Playbook Inventory

| Playbook | Purpose | Complexity | Interactive |
|----------|---------|------------|-------------|
| [`hello_world.yml`](#hello-world-playbook) | Basic connectivity test | Simple | No |
| [`system_info_template.yml`](#system-information-gathering-playbook) | System information collection | Medium | Yes |
| [`file_operations_template.yml`](#file-operations-playbook) | File system operations | Advanced | Yes |

---

## 🚀 Quick Start

### Prerequisites
- Ansible Automation Platform (AAP) 2.x
- Target hosts configured in inventory
- Appropriate SSH access and sudo privileges
- Job Templates configured in AAP

### Basic Execution
```bash
# Direct ansible-playbook execution (for testing)
ansible-playbook -i inventory playbooks/hello_world.yml

# With custom variables
ansible-playbook -i inventory playbooks/system_info_template.yml \
  -e "target_user=admin" \
  -e "log_file_path=/var/log/system_info.log"
```

---

## 📖 Playbook Documentation

### Hello World Playbook
**File:** `hello_world.yml`  
**Purpose:** Basic connectivity and execution test

#### Description
A simple playbook that verifies connectivity to target hosts and displays basic information. Perfect for testing job template functionality and network connectivity.

#### Features
- ✅ No user input required
- ✅ Fast execution (< 30 seconds)
- ✅ Minimal resource usage
- ✅ Safe for production testing

#### Usage
```yaml
# AAP Job Template Configuration
Name: "Hello World Test"
Playbook: hello_world.yml
Inventory: <your-inventory>
Survey: Not required
```

#### Expected Output
```
TASK [Display hello world message] 
ok: [host1] => {
    "msg": "Hello, World! This playbook is running from AAP job template."
}

TASK [Display current date and time] 
ok: [host1] => {
    "msg": "Current date and time: 2024-08-20T15:30:00.000Z"
}

TASK [Display target host information] 
ok: [host1] => {
    "msg": "Running on host: host1"
}
```

#### Use Cases
- 🔍 Testing new AAP installations
- 🔍 Verifying inventory connectivity
- 🔍 Validating job template configuration
- 🔍 Network connectivity troubleshooting

---

### System Information Gathering Playbook
**File:** `system_info_template.yml`  
**Purpose:** Comprehensive system information collection

#### Description
Collects detailed system information including hardware specs, user details, disk usage, and optional network/process information. Generates a structured report for system auditing and documentation.

#### Interactive Parameters
| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `target_user` | Text | "" (current user) | Username to analyze |
| `log_file_path` | Text | "/tmp/system_info.log" | Report output location |
| `include_processes` | Choice | "no" | Include top processes |
| `include_network` | Choice | "yes" | Include network info |

#### Features
- 📊 Hardware information (CPU, Memory, Architecture)
- 👤 User account details (UID, GID, Home, Shell)
- 💾 Disk usage analysis
- 🌐 Network configuration (optional)
- ⚡ Process monitoring (optional)
- 📄 Structured log file output

#### Usage
```yaml
# AAP Job Template Configuration
Name: "System Information Collection"
Playbook: system_info_template.yml
Inventory: <your-inventory>
Survey: Required (see AAP_Survey_Questions.md)
```

#### Manual Execution
```bash
# With all options enabled
ansible-playbook system_info_template.yml \
  -e "target_user=admin" \
  -e "log_file_path=/var/log/system_audit.log" \
  -e "include_processes=yes" \
  -e "include_network=yes"

# Minimal execution
ansible-playbook system_info_template.yml \
  -e "log_file_path=/tmp/quick_info.log"
```

#### Sample Output Report
```
System Information Report
Generated on: 2024-08-20T15:30:00.000Z
Hostname: web-server-01
OS: Ubuntu 20.04
Architecture: x86_64
Kernel: 5.4.0-74-generic
Memory: 1024/4096 MB (free/total)
CPU Count: 4
Uptime: 86400 seconds
IP Address: 192.168.1.100
Gateway: 192.168.1.1
User: admin
```

#### Use Cases
- 📋 System auditing and compliance
- 📋 Infrastructure documentation
- 📋 Performance baseline collection
- 📋 Security assessments
- 📋 Capacity planning

---

### File Operations Playbook
**File:** `file_operations_template.yml`  
**Purpose:** Safe file system operations with multiple operation types

#### Description
Performs various file system operations with built-in safety controls and confirmation mechanisms. Supports create, copy, move, delete, permission changes, and file search operations.

#### Interactive Parameters
| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `operation_type` | Choice | "1" | Operation to perform (1-6) |
| `source_path` | Text | "/tmp/example.txt" | Source file/directory |
| `destination_path` | Text | "/tmp/backup/" | Target location |
| `file_permissions` | Choice | "0644" | Octal permissions |
| `search_pattern` | Choice | "*.txt" | File search pattern |
| `search_directory` | Text | "/tmp" | Search location |
| `file_content` | Textarea | "This is a test..." | New file content |

#### Available Operations

##### 1. Create File/Directory
- Creates new files with custom content
- Automatically creates parent directories
- Sets specified permissions
- Adds timestamp to content

```bash
# Example: Create configuration file
Operation: 1 (Create)
Source Path: /etc/myapp/config.conf
Content: "server_port=8080\ndebug=false"
Permissions: 0644
```

##### 2. Copy File/Directory
- Copies files or entire directories
- Preserves or sets new permissions
- Validates source existence
- Supports remote-to-remote copying

```bash
# Example: Backup configuration
Operation: 2 (Copy)
Source Path: /etc/myapp/config.conf
Destination: /backup/config.conf.bak
Permissions: 0644
```

##### 3. Move/Rename File/Directory
- Moves files or directories
- Supports renaming operations
- Validates source existence
- Atomic operation where possible

```bash
# Example: Archive old logs
Operation: 3 (Move)
Source Path: /var/log/app.log
Destination: /archive/app.log.old
```

##### 4. Delete File/Directory
- Safely removes files or directories
- Includes confirmation warnings
- Validates existence before deletion
- Supports recursive directory removal

```bash
# Example: Clean temporary files
Operation: 4 (Delete)
Source Path: /tmp/old_data/
```

##### 5. Change Permissions
- Modifies file or directory permissions
- Supports octal notation
- Validates target existence
- Shows before/after permissions

```bash
# Example: Secure sensitive file
Operation: 5 (Permissions)
Source Path: /etc/ssl/private/key.pem
Permissions: 0600
```

##### 6. Search for Files
- Finds files matching patterns
- Recursive directory searching
- Shows file details (size, date)
- Supports wildcards and regex

```bash
# Example: Find log files
Operation: 6 (Search)
Search Directory: /var/log
Search Pattern: *.log
```

#### Safety Features
- 🛡️ **Confirmation Required**: Explicit user confirmation for destructive operations
- 🛡️ **Backup Options**: Optional backup creation before modifications
- 🛡️ **Path Validation**: Checks for valid paths and permissions
- 🛡️ **Error Handling**: Graceful failure handling with clear messages
- 🛡️ **Audit Trail**: Detailed logging of all operations

#### Usage
```yaml
# AAP Job Template Configuration
Name: "File Operations Manager"
Playbook: file_operations_template.yml
Inventory: <your-inventory>
Survey: Required (see AAP_Survey_Questions.md)
Limit: Specific hosts recommended
```

#### Manual Execution Examples
```bash
# Create a new configuration file
ansible-playbook file_operations_template.yml \
  -e "operation_type=1" \
  -e "source_path=/etc/myapp/settings.conf" \
  -e "file_content=port=8080\ndebug=false" \
  -e "file_permissions=0644"

# Search for log files
ansible-playbook file_operations_template.yml \
  -e "operation_type=6" \
  -e "search_directory=/var/log" \
  -e "search_pattern=*.log"

# Backup important directory
ansible-playbook file_operations_template.yml \
  -e "operation_type=2" \
  -e "source_path=/etc/myapp/" \
  -e "destination_path=/backup/myapp-$(date +%Y%m%d)/"
```

#### Use Cases
- 🔧 Configuration file management
- 🔧 Log file maintenance and archival
- 🔧 Backup and restore operations
- 🔧 Permission auditing and correction
- 🔧 File system cleanup and organization
- 🔧 Application deployment preparation

---

## 🔧 AAP Integration

### Job Template Setup

1. **Create Job Template**
   ```
   Name: <Descriptive Name>
   Job Type: Run
   Inventory: <Target Inventory>
   Project: <This Repository>
   Playbook: playbooks/<playbook-name>.yml
   Credentials: <SSH Credential>
   ```

2. **Configure Survey** (for interactive playbooks)
   - Enable "Prompt on Launch" → Survey
   - Import questions from `AAP_Survey_Questions.md`
   - Test survey functionality

3. **Set Permissions**
   - Assign appropriate user/team permissions
   - Configure execution permissions
   - Set up approval workflows if needed

### Execution Best Practices

#### Development Environment
```bash
# Test playbooks locally first
ansible-playbook --check playbooks/system_info_template.yml
ansible-playbook --diff playbooks/file_operations_template.yml
```

#### Production Environment
- ✅ Always test in development first
- ✅ Use specific host limits for file operations
- ✅ Enable job approval for destructive operations
- ✅ Monitor execution logs
- ✅ Implement backup strategies

### Monitoring and Logging

#### AAP Job Logs
- Monitor job execution status
- Review survey input validation
- Check task-level output
- Analyze failure patterns

#### System Logs
```bash
# Check generated log files
tail -f /tmp/system_info.log
ls -la /var/log/ansible/

# Monitor file operations
auditctl -w /etc -p wa -k config_changes
```

---

## 🛠️ Troubleshooting

### Common Issues

#### Connection Problems
```bash
# Test connectivity
ansible all -m ping -i inventory

# Check SSH access
ansible all -m setup -i inventory --limit host1
```

#### Permission Errors
```bash
# Verify sudo access
ansible all -m shell -a "sudo whoami" -i inventory

# Check file permissions
ansible all -m file -a "path=/tmp state=directory mode=0755"
```

#### Survey Issues
- Verify variable names match playbook vars_prompt
- Check survey question types and validation
- Test with different input combinations
- Review AAP survey logs

### Error Messages

| Error | Cause | Solution |
|-------|-------|----------|
| "vars_prompt not supported" | Running with --extra-vars | Use AAP survey instead |
| "Permission denied" | Insufficient privileges | Check sudo configuration |
| "File not found" | Invalid path | Verify path exists and is accessible |
| "Survey validation failed" | Invalid input | Check survey constraints |

---

## 📚 Additional Resources

### Documentation
- [`AAP_Survey_Questions.md`](./AAP_Survey_Questions.md) - Complete survey configuration
- [Ansible Documentation](https://docs.ansible.com/) - Official Ansible docs
- [AAP Documentation](https://access.redhat.com/documentation/en-us/red_hat_ansible_automation_platform/) - Platform-specific guides

### Related Files
```
ansible-rhdh-templates/
├── playbooks/
│   ├── README.md                    # This file
│   ├── hello_world.yml             # Basic test playbook
│   ├── system_info_template.yml    # System information gathering
│   └── file_operations_template.yml # File operations
├── AAP_Survey_Questions.md         # Survey configuration guide
└── templates/                      # Backstage templates
```

### Support
- **Repository**: ansible-rhdh-templates
- **Branch**: testahon
- **Compatibility**: AAP 2.x, Ansible Core 2.12+
- **Last Updated**: August 2024

---

## 🔄 Version History

| Version | Date | Changes |
|---------|------|---------|
| 1.0.0 | 2024-08-20 | Initial playbook collection |
| | | - Added hello_world.yml |
| | | - Added system_info_template.yml |
| | | - Added file_operations_template.yml |
| | | - Created comprehensive documentation |

---

*For questions or issues, please refer to the repository documentation or contact the development team.*
