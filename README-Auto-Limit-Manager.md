# Auto Limit Manager - All-in-One Script

## Overview
This script automatically configures IP limits, quota limits, and multilogin restrictions for all users on your server. It combines the functionality of multiple separate scripts into one automated solution with **COMPLETE IP LIMITING FOR ALL SERVICES**.

## What It Does
1. **IP Limiting**: Sets maximum 2 concurrent connections per user for **ALL SERVICES**
2. **Quota Management**: Sets 1000GB bandwidth quota for all users
3. **Auto-Kill**: Enables automatic multilogin monitoring and killing
4. **Multi-Service Support**: Works with SSH, VMess, VLess, and Trojan users

## 🆕 **NEW: Complete IP Limiting for All Services**
- ✅ **SSH**: IP limits in `/etc/kyt/limit/ssh/ip/`
- ✅ **VMess**: IP limits in `/etc/kyt/limit/vmess/ip/`
- ✅ **VLess**: IP limits in `/etc/kyt/limit/vless/ip/`
- ✅ **Trojan**: IP limits in `/etc/kyt/limit/trojan/ip/`
- ✅ **Universal**: Also creates `/etc/kyt/limit/ssh/ip/` for compatibility

## Features
- ✅ **Fully Automated**: No manual input required
- ✅ **Comprehensive Coverage**: All user types and services supported
- ✅ **Real-time Monitoring**: Automatic multilogin detection for ALL services
- ✅ **Easy Configuration**: Simple config file for customization
- ✅ **Detailed Logging**: Complete audit trail of all changes
- ✅ **Service Management**: Automatic service restarts
- ✅ **Enhanced IP Limiting**: Service-specific IP limit directories

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

## Configuration

### Basic Configuration
Edit `auto-limit-config.conf` to customize:

```bash
# IP Limit (applies to ALL services)
MAX_IP_LIMIT=2

# Quota Limit (applies to ALL services)
QUOTA_GB=1000

# Auto-kill interval
AUTOKILL_INTERVAL=1
```

### Advanced Configuration
```bash
# Custom quotas for specific users
CUSTOM_QUOTAS="admin=2000,premium=5000"

# Custom IP limits for specific users
CUSTOM_IP_LIMITS="admin=5,premium=10"

# IP whitelist
WHITELIST_IPS="192.168.1.1,10.0.0.1"
```

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

## File Structure
```
/workspace/
├── auto-limit-manager          # Main script
├── auto-limit-config.conf      # Configuration file
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
└── monitor-all-services        # Enhanced monitoring script
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

# Check enhanced monitoring script
ls -la /usr/local/sbin/monitor-all-services
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

### Manual Verification
```bash
# Test IP limiting
sudo bash -n /usr/local/sbin/tendang

# Check all IP limit directories
sudo find /etc/kyt/limit -name "*" -type f | head -20

# Check quota files
sudo find /etc -name "*" -type f | grep -E "(ssh|vmess|vless|trojan)" | head -10

# Verify cron job
sudo crontab -l

# Test enhanced monitoring
sudo bash -n /usr/local/sbin/monitor-all-services
```

## Integration with Existing Scripts

This script is designed to work alongside your existing scripts:
- **`limitssh-ip`**: Still works for manual IP limit management
- **`tendang`**: Used by the auto-kill system
- **`auto-kill`**: Can be used for manual auto-kill configuration
- **Quota scripts**: Can still be used for individual user management
- **Enhanced monitoring**: New `/usr/local/sbin/monitor-all-services` script

## Safety Features

- ✅ **Backup Creation**: Logs all changes before applying
- ✅ **Error Handling**: Graceful failure with detailed error messages
- ✅ **Service Protection**: Checks service status before restarting
- ✅ **User Validation**: Verifies users exist before applying limits
- ✅ **Service-Specific Directories**: Creates separate IP limit directories for each service
- ✅ **Universal Compatibility**: Maintains compatibility with existing scripts

## Performance Impact

- **Minimal**: Script runs once for configuration
- **Monitoring**: Cron job runs every minute (very light)
- **Storage**: Small log files, quota files, and IP limit files
- **Network**: No external connections or downloads
- **Enhanced**: Better service separation and monitoring

## Support

For issues or questions:
1. Check the log file: `/root/auto-limit-manager.log`
2. Verify configuration in `auto-limit-config.conf`
3. Ensure all required services are running
4. Check file permissions and ownership
5. Verify IP limit directories are created for all services

## Version History

- **v1.0**: Initial release with basic functionality
- **v1.1**: Enhanced IP limiting for ALL services (SSH, VMess, VLess, Trojan)
- **Features**: Complete IP limiting, quota management, auto-kill
- **Compatibility**: SSH, VMess, VLess, Trojan
- **Automation**: Fully automated configuration
- **Enhanced**: Service-specific IP limit directories

---

**Note**: This script must be run as root and will modify system configurations. Always backup your system before running automation scripts.