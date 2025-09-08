# Loopback Configuration & Backup Guide

## Overview

Loopback by Rogue Amoeba is a virtual audio routing application for macOS that creates virtual audio devices. While Loopback doesn't have a native command-line interface, there are several approaches to backup, restore, and manage configurations programmatically.

**Consider this an experimental guide only,
hopefully Rogue Amoeba will allow scripting of Loopback configuration
and operation in a later release.**

## Configuration Storage Location

### Primary Configuration Files
Loopback stores its configuration in the following locations:

```bash
# Main application support directory
~/Library/Application Support/Loopback/

# Preferences file
~/Library/Preferences/com.rogueamoeba.Loopback.plist

# Device configurations
~/Library/Application Support/Loopback/Devices/

# Audio settings and presets
~/Library/Application Support/Loopback/Audio/
```

### Key Configuration Files
- **Device Definitions**: Stored as property list files
- **Audio Settings**: Sample rates, buffer sizes, routing configurations
- **Application Preferences**: UI settings, default behaviors
- **Virtual Device Configurations**: Your "RecordingRouter" setup

## Backup Strategies

### 1. Manual Configuration Export

#### Export Current Configuration
```bash
# Create backup directory
mkdir -p ~/Desktop/loopback-backup/$(date +%Y%m%d)

# Copy all Loopback configuration files
cp -R ~/Library/Application\ Support/Loopback/ ~/Desktop/loopback-backup/$(date +%Y%m%d)/

# Copy preferences
cp ~/Library/Preferences/com.rogueamoeba.Loopback.plist ~/Desktop/loopback-backup/$(date +%Y%m%d)/
```

#### Automated Backup Script
```bash
#!/bin/bash
# loopback-backup.sh

BACKUP_DIR="$HOME/Desktop/loopback-backups"
DATE=$(date +%Y%m%d_%H%M%S)
BACKUP_PATH="$BACKUP_DIR/loopback_$DATE"

# Create backup directory
mkdir -p "$BACKUP_PATH"

# Backup Loopback configuration
cp -R "$HOME/Library/Application Support/Loopback/" "$BACKUP_PATH/"

# Backup preferences
cp "$HOME/Library/Preferences/com.rogueamoeba.Loopback.plist" "$BACKUP_PATH/"

# Create metadata file
cat > "$BACKUP_PATH/backup-info.txt" << EOF
Loopback Configuration Backup
Date: $(date)
macOS Version: $(sw_vers -productVersion)
Loopback Version: $(defaults read /Applications/Loopback.app/Contents/Info.plist CFBundleShortVersionString)
EOF

echo "Backup completed: $BACKUP_PATH"
```

### 2. Git-Based Configuration Management

#### Initialize Git Repository
```bash
# Create configuration repository
mkdir ~/loopback-config
cd ~/loopback-config

# Initialize git repository
git init

# Create .gitignore
cat > .gitignore << EOF
# Exclude temporary files
*.tmp
*.temp
*.log

# Exclude system-specific paths
SystemAudio/
EOF

# Add configuration files
cp -R ~/Library/Application\ Support/Loopback/ .
cp ~/Library/Preferences/com.rogueamoeba.Loopback.plist .

# Initial commit
git add .
git commit -m "Initial Loopback configuration backup"
```

#### Automated Git Backup Script
```bash
#!/bin/bash
# loopback-git-backup.sh

CONFIG_DIR="$HOME/loopback-config"
LOOPBACK_SUPPORT="$HOME/Library/Application Support/Loopback"
LOOPBACK_PREFS="$HOME/Library/Preferences/com.rogueamoeba.Loopback.plist"

cd "$CONFIG_DIR"

# Update configuration files
cp -R "$LOOPBACK_SUPPORT" .
cp "$LOOPBACK_PREFS" .

# Commit changes
git add .
git commit -m "Update Loopback configuration $(date)"

# Push to remote repository (if configured)
# git push origin main
```

## Restore Strategies

### 1. Manual Restore
```bash
# Stop Loopback
killall Loopback

# Restore from backup
cp -R ~/Desktop/loopback-backup/YYYYMMDD/Loopback/ ~/Library/Application\ Support/
cp ~/Desktop/loopback-backup/YYYYMMDD/com.rogueamoeba.Loopback.plist ~/Library/Preferences/

# Restart Loopback
open -a Loopback
```

### 2. Automated Restore Script
```bash
#!/bin/bash
# loopback-restore.sh

if [ $# -eq 0 ]; then
    echo "Usage: $0 <backup-date>"
    echo "Example: $0 20241201"
    exit 1
fi

BACKUP_DATE=$1
BACKUP_DIR="$HOME/Desktop/loopback-backup/$BACKUP_DATE"

if [ ! -d "$BACKUP_DIR" ]; then
    echo "Backup directory not found: $BACKUP_DIR"
    exit 1
fi

echo "Restoring Loopback configuration from $BACKUP_DATE..."

# Stop Loopback
killall Loopback 2>/dev/null
sleep 2

# Restore configuration
cp -R "$BACKUP_DIR/Loopback/" ~/Library/Application\ Support/
cp "$BACKUP_DIR/com.rogueamoeba.Loopback.plist" ~/Library/Preferences/

# Restart Loopback
open -a Loopback

echo "Restore completed. Loopback has been restarted."
```

## Configuration Management

### 1. Device Configuration Export

#### Export Specific Device Configuration
```bash
# Export your RecordingRouter device configuration
cp ~/Library/Application\ Support/Loopback/Devices/RecordingRouter.plist ~/Desktop/recording-router-config.plist
```

#### Import Device Configuration
```bash
# Import device configuration
cp ~/Desktop/recording-router-config.plist ~/Library/Application\ Support/Loopback/Devices/
```

### 2. Audio Settings Backup
```bash
# Backup audio settings
cp ~/Library/Application\ Support/Loopback/Audio/settings.plist ~/Desktop/audio-settings.plist
```

## Alternative Approaches

### 1. AppleScript Automation
```applescript
-- Loopback Configuration Backup Script
tell application "Loopback"
    activate
    delay 2

    -- Export current configuration
    tell application "System Events"
        tell process "Loopback"
            -- Navigate to export menu
            click menu item "Export Configuration" of menu "File" of menu bar 1
        end tell
    end tell
end tell
```

### 2. Configuration Validation Script
```bash
#!/bin/bash
# validate-loopback-config.sh

echo "Validating Loopback configuration..."

# Check if Loopback is installed
if [ ! -d "/Applications/Loopback.app" ]; then
    echo "ERROR: Loopback not found in /Applications"
    exit 1
fi

# Check configuration directory
if [ ! -d "$HOME/Library/Application Support/Loopback" ]; then
    echo "ERROR: Loopback configuration directory not found"
    exit 1
fi

# Check for RecordingRouter device
if [ ! -f "$HOME/Library/Application Support/Loopback/Devices/RecordingRouter.plist" ]; then
    echo "WARNING: RecordingRouter device configuration not found"
else
    echo "✓ RecordingRouter device configuration found"
fi

# Check preferences
if [ ! -f "$HOME/Library/Preferences/com.rogueamoeba.Loopback.plist" ]; then
    echo "WARNING: Loopback preferences not found"
else
    echo "✓ Loopback preferences found"
fi

echo "Configuration validation complete"
```

## GitHub Integration

### 1. Repository Structure
```
loopback-config/
├── Devices/
│   └── RecordingRouter.plist
├── Audio/
│   └── settings.plist
├── Preferences/
│   └── com.rogueamoeba.Loopback.plist
├── scripts/
│   ├── backup.sh
│   ├── restore.sh
│   └── validate.sh
└── README.md
```

### 2. Automated GitHub Backup
```bash
#!/bin/bash
# github-backup.sh

# Update local repository
cd ~/loopback-config
./scripts/backup.sh

# Push to GitHub
git push origin main

echo "Configuration backed up to GitHub"
```

## Best Practices

### 1. Regular Backups
- **Daily**: For active development
- **Weekly**: For stable configurations
- **Before Updates**: Always backup before updating Loopback

### 2. Version Control
- Use descriptive commit messages
- Tag important configurations
- Maintain a changelog

### 3. Testing
- Test restore procedures regularly
- Validate configurations on different machines
- Document any machine-specific settings

### 4. Security
- Don't commit sensitive audio settings
- Use .gitignore for temporary files
- Consider encrypting backup archives

## Troubleshooting

### Common Issues

#### Configuration Not Restoring
```bash
# Check file permissions
ls -la ~/Library/Application\ Support/Loopback/
ls -la ~/Library/Preferences/com.rogueamoeba.Loopback.plist

# Reset permissions if needed
chmod 644 ~/Library/Preferences/com.rogueamoeba.Loopback.plist
chmod -R 755 ~/Library/Application\ Support/Loopback/
```

#### Device Not Appearing
```bash
# Restart audio services
sudo killall coreaudiod
sudo launchctl start com.apple.audio.coreaudiod

# Restart Loopback
killall Loopback
open -a Loopback
```

## Limitations

### What Loopback Doesn't Support
- **Native CLI**: No built-in command-line interface
- **API Access**: No public API for programmatic control
- **Remote Configuration**: No built-in remote management
- **Configuration Templates**: No native template system

### Workarounds
- Use AppleScript for automation
- Manual configuration export/import
- File system-based backup/restore
- Third-party automation tools

## Conclusion

While Loopback doesn't provide a native command-line interface, you can effectively manage configurations through:

1. **File system backups** of configuration directories
2. **Git-based version control** for tracking changes
3. **Automated scripts** for backup/restore operations
4. **AppleScript automation** for GUI interactions

This approach allows you to maintain your "RecordingRouter" configuration in a GitHub repository and restore it quickly on replacement machines or after system updates.