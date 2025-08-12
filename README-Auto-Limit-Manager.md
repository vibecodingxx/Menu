# Auto Limit Manager - All-in-One Script

## Overview
This script automatically configures IP limits, quota limits, and multilogin restrictions for all users on your server. It combines the functionality of multiple separate scripts into one automated solution with **COMPLETE IP LIMITING FOR ALL SERVICES** and **AUTOMATIC 4-HOUR SCHEDULING**.

## What It Does
1. **IP Limiting**: Sets maximum 2 concurrent connections per user for **ALL SERVICES**
2. **Quota Management**: Sets 1000GB bandwidth quota for all users
3. **Auto-Kill**: Enables automatic multilogin monitoring and killing
4. **Multi-Service Support**: Works with SSH, VMess, VLess, and Trojan users
5. **🆕 Auto-Scheduler**: Runs every 4 hours to check for new users and apply limits automatically

## 🆕 **NEW: Complete IP Limiting + Auto-Scheduling**
- ✅ **SSH**: IP limits in `/etc/kyt/limit/ssh/ip/`
- ✅ **VMess**: IP limits in `/etc/kyt/limit/vmess/ip/`
- ✅ **VLess**: IP limits in `/etc/kyt/limit/vless/ip/`
- ✅ **Trojan**: IP limits in `/etc/kyt/limit/trojan/ip/`
- ✅ **Universal**: Also creates `/etc/kyt/limit/ssh/ip/` for compatibility
- ✅ **🆕 Auto-Scheduler**: Every 4 hours automatically checks for new users

## Features
- ✅ **Fully Automated**: No manual input required
- ✅ **Comprehensive Coverage**: All user types and services supported
- ✅ **Real-time Monitoring**: Automatic multilogin detection for ALL services
- ✅ **Integrated Configuration**: All settings are in the script (no separate config file)
- ✅ **Detailed Logging**: Complete audit trail of all changes
- ✅ **Service Management**: Automatic service restarts
- ✅ **Enhanced IP Limiting**: Service-specific IP limit directories
- ✅ **🆕 Auto-Scheduling**: Automatically runs every 4 hours to handle new users

## Quick Start

### 1. Run the Script
```bash
sudo ./auto-limit-manager
```

### 2. What Happens Automatically
- All users get maximum 2 IP connections **across ALL services**
- All users get 1000GB bandwidth quota
- Auto-kill is enabled (checks every 1 minute)
- All services are restarted
- Configuration is logged
- **IP limits are set for SSH, VMess, VLess, and Trojan**
- **🆕 Auto-scheduler is set up to run every 4 hours**

## Configuration

### **All Configuration is Now Integrated in the Script!**

Edit the configuration section at the top of `auto-limit-manager`:

```bash
# ==========================================
# CONFIGURATION SECTION - EDIT THESE VALUES
# ==========================================

# IP Limit Configuration
MAX_IP_LIMIT=2

# Quota Configuration  
QUOTA_GB=1000

# Auto-Kill Configuration
AUTOKILL_INTERVAL=1

# Auto-Limit Manager Schedule
MANAGER_INTERVAL=4  # Every 4 hours

# Service Configuration
ENABLE_SSH=1
ENABLE_VMESS=1
ENABLE_VLESS=1
ENABLE_TROJAN=1

# Logging Configuration
VERBOSE_LOGGING=1

# Auto-Restart Configuration
AUTO_RESTART_SERVICES=1
```

### **No More Separate Config File!**
- All settings are now in the main script
- Easy to edit and maintain
- No need to manage multiple files

## How It Works

### 1. Enhanced IP Limit Management
- Creates service-specific IP limit directories:
  - `/etc/kyt/limit/ssh/ip/` - SSH users
  - `/etc/kyt/limit/vmess/ip/` - VMess users
  - `/etc/kyt/limit/vless/ip/` - VLess users
  - `/etc/kyt/limit/trojan/ip/` - Trojan users
- Sets IP limit files for each user in each service
- Creates universal compatibility directory for existing scripts
- Monitors active connections across all services
- Automatically kills excess connections

### 2. Quota Management
- **SSH**: Creates quota files in `/etc/ssh/`
- **VMess**: Creates quota files in `/etc/vmess/`
- **VLess**: Creates quota files in `/etc/vless/`
- **Trojan**: Creates quota files in `/etc/trojan/`

### 3. Enhanced Auto-Kill System
- Sets up cron job: `*/1 * * * * root /usr/local/sbin/tendang 2`
- Monitors connections every minute
- Automatically kills users exceeding IP limits
- **Monitors ALL services** (SSH, VMess, VLess, Trojan)
- Logs all actions
- Creates enhanced monitoring script: `/usr/local/sbin/monitor-all-services`

### 4. 🆕 **Auto-Limit Manager Scheduler**
- **Runs every 4 hours automatically**
- Checks for new users across all services
- Automatically applies IP limits and quotas to new users
- No manual intervention needed
- Logs all automatic updates

## File Structure
```
/workspace/
├── auto-limit-manager          # Main script (with integrated config)
├── README-Auto-Limit-Manager.md # This file
└── /root/
    └── auto-limit-manager.log  # Log file (created after first run)

/etc/kyt/limit/
├── ssh/ip/                     # SSH user IP limits
├── vmess/ip/                   # VMess user IP limits
├── vless/ip/                   # VLess user IP limits
└── trojan/ip/                  # Trojan user IP limits

/usr/local/sbin/
├── tendang                     # Main auto-kill script
├── monitor-all-services        # Enhanced monitoring script
└── auto-limit-manager         # Copy for cron execution

/etc/cron.d/
├── limitssh-ip                 # Auto-kill cron job (every 1 minute)
└── auto-limit-manager         # Manager scheduler (every 4 hours)
```

## Monitoring and Logs

### View Logs
```bash
tail -f /root/auto-limit-manager.log
```

### Check Status
```bash
# View current IP limits by service
ls /etc/kyt/limit/ssh/ip/      # SSH users
ls /etc/kyt/limit/vmess/ip/    # VMess users
ls /etc/kyt/limit/vless/ip/    # VLess users
ls /etc/kyt/limit/trojan/ip/   # Trojan users

# View current quotas by service
ls /etc/ssh/ | grep -v ".ssh.db"
ls /etc/vmess/
ls /etc/vless/
ls /etc/trojan/ | grep -v ".trojan.db"

# Check auto-kill cron job
cat /etc/cron.d/limitssh-ip

# Check manager scheduler
cat /etc/cron.d/auto-limit-manager

# Check enhanced monitoring script
ls -la /usr/local/sbin/monitor-all-services
```

## 🆕 **Auto-Scheduling Features**

### **What Happens Every 4 Hours:**
1. **Automatic Detection**: Script checks for new users across all services
2. **Smart Updates**: Only updates if new users are detected
3. **Automatic Application**: Applies IP limits and quotas to new users
4. **Logging**: Records all automatic activities
5. **No Manual Intervention**: Completely hands-off operation

### **Manual Trigger:**
```bash
# Run the check manually (without full setup)
sudo /usr/local/sbin/auto-limit-manager --check-only
```

## Troubleshooting

### Common Issues

1. **Permission Denied**
   ```bash
   sudo chmod +x auto-limit-manager
   sudo ./auto-limit-manager
   ```

2. **Services Not Restarting**
   ```bash
   # Manual restart
   sudo systemctl restart ssh
   sudo systemctl restart xray
   ```

3. **Cron Job Not Working**
   ```bash
   # Check cron service
   sudo systemctl status cron
   
   # Restart cron
   sudo systemctl restart cron
   ```

4. **IP Limits Not Working for Xray Services**
   ```bash
   # Check if Xray config exists
   ls -la /etc/xray/config.json
   
   # Verify user detection
   grep -E "^### |^#& |^#! " /etc/xray/config.json
   ```

5. **🆕 Auto-Scheduler Not Working**
   ```bash
   # Check if scheduler cron job exists
   cat /etc/cron.d/auto-limit-manager
   
   # Check if script is in /usr/local/sbin
   ls -la /usr/local/sbin/auto-limit-manager
   
   # Test manual check
   sudo /usr/local/sbin/auto-limit-manager --check-only
   ```

### Manual Verification
```bash
# Test IP limiting
sudo bash -n /usr/local/sbin/tendang

# Check all IP limit directories
sudo find /etc/kyt/limit -name "*" -type f | head -20

# Check quota files
sudo find /etc -name "*" -type f | grep -E "(ssh|vmess|vless|trojan)" | head -10

# Verify cron jobs
sudo crontab -l

# Test enhanced monitoring
sudo bash -n /usr/local/sbin/monitor-all-services

# Test manager script
sudo bash -n /usr/local/sbin/auto-limit-manager
```

## Integration with Existing Scripts

This script is designed to work alongside your existing scripts:
- **`limitssh-ip`**: Still works for manual IP limit management
- **`tendang`**: Used by the auto-kill system
- **`auto-kill`**: Can be used for manual auto-kill configuration
- **Quota scripts**: Can still be used for individual user management
- **Enhanced monitoring**: New `/usr/local/sbin/monitor-all-services` script
- **🆕 Auto-scheduler**: New automatic 4-hour checking system

## Safety Features

- ✅ **Backup Creation**: Logs all changes before applying
- ✅ **Error Handling**: Graceful failure with detailed error messages
- ✅ **Service Protection**: Checks service status before restarting
- ✅ **User Validation**: Verifies users exist before applying limits
- ✅ **Service-Specific Directories**: Creates separate IP limit directories for each service
- ✅ **Universal Compatibility**: Maintains compatibility with existing scripts
- ✅ **🆕 Smart Scheduling**: Only updates when new users are detected

## Performance Impact

- **Minimal**: Script runs once for configuration
- **Monitoring**: Cron job runs every minute (very light)
- **Scheduling**: Manager runs every 4 hours (very light)
- **Storage**: Small log files, quota files, and IP limit files
- **Network**: No external connections or downloads
- **Enhanced**: Better service separation and monitoring

## Support

For issues or questions:
1. Check the log file: `/root/auto-limit-manager.log`
2. Verify configuration in the script header
3. Ensure all required services are running
4. Check file permissions and ownership
5. Verify IP limit directories are created for all services
6. **🆕 Check auto-scheduler cron job**: `cat /etc/cron.d/auto-limit-manager`

## Version History

- **v1.0**: Initial release with basic functionality
- **v1.1**: Enhanced IP limiting for ALL services (SSH, VMess, VLess, Trojan)
- **v1.2**: 🆕 **Integrated configuration + Auto-scheduling every 4 hours**
- **Features**: Complete IP limiting, quota management, auto-kill, auto-scheduling
- **Compatibility**: SSH, VMess, VLess, Trojan
- **Automation**: Fully automated configuration + automatic new user handling

---

**Note**: This script must be run as root and will modify system configurations. Always backup your system before running automation scripts.