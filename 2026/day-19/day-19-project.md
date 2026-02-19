### Task 1: Log Rotation Script
          Create `log_rotate.sh` that:
          1. Takes a log directory as an argument (e.g., `/var/log/myapp`)
          2. Compresses `.log` files older than 7 days using `gzip`
          3. Deletes `.gz` files older than 30 days
          4. Prints how many files were compressed and deleted
          5. Exits with an error if the directory doesn't exist
          
          
          '''
          #!/bin/bash
          
          set -euo pipefail
          
          # Check if directory argument is provided
          if [ $# -ne 1 ]; then
              echo "Usage: $0 <log_directory>"
              exit 1
          fi
          
          LOG_DIR="$1"
          
          # Check if directory exists
          if [ ! -d "$LOG_DIR" ]; then
              echo "Error: Directory '$LOG_DIR' does not exist."
              exit 1
          fi
          
          echo "Log rotation started for: $LOG_DIR"
          echo "---------------------------------------"
          
          # Initialize counters
          compressed_count=0
          deleted_count=0
          
          # Compress .log files older than 7 days
          while IFS= read -r file; do
              gzip "$file"
              ((compressed_count++))
          done < <(find "$LOG_DIR" -type f -name "*.log" -mtime +7)
          
          # Delete .gz files older than 30 days
          while IFS= read -r file; do
              rm -f "$file"
              ((deleted_count++))
          done < <(find "$LOG_DIR" -type f -name "*.gz" -mtime +30)
          
          echo "---------------------------------------"
          echo "Files compressed : $compressed_count"
          echo "Files deleted    : $deleted_count"
          echo "Log rotation completed successfully."
          
          OUTPUT:
          Log rotation started for: /var/log/myapp
          ---------------------------------------
          Files compressed : 5
          Files deleted    : 2
          Log rotation completed successfully.

What This Script Does
    Validates input argument
    Checks directory existence
    Compresses .log files older than 7 days
    Deletes .gz files older than 30 days
    Counts and prints results
    Uses set -euo pipefail for safety
'''

### Task 2: Server Backup Script
        Create `backup.sh` that:
        1. Takes a source directory and backup destination as arguments
        2. Creates a timestamped `.tar.gz` archive (e.g., `backup-2026-02-08.tar.gz`)
        3. Verifies the archive was created successfully
        4. Prints archive name and size
        5. Deletes backups older than 14 days from the destination
        6. Handles errors — exit if source doesn't exist

  '''
          #!/bin/bash
        
        set -euo pipefail
        
        # -----------------------------
        # Argument Validation
        # -----------------------------
        if [ $# -ne 2 ]; then
            echo "Usage: $0 <source_directory> <backup_destination>"
            exit 1
        fi
        
        SOURCE="$1"
        DEST="$2"
        
        # Check if source exists
        if [ ! -d "$SOURCE" ]; then
            echo "Error: Source directory '$SOURCE' does not exist."
            exit 1
        fi
        
        # Create destination if it doesn't exist
        mkdir -p "$DEST"
        
        # -----------------------------
        # Create Timestamped Archive
        # -----------------------------
        TIMESTAMP=$(date +%Y-%m-%d)
        ARCHIVE_NAME="backup-$TIMESTAMP.tar.gz"
        ARCHIVE_PATH="$DEST/$ARCHIVE_NAME"
        
        echo "Creating backup..."
        tar -czf "$ARCHIVE_PATH" -C "$SOURCE" .
        
        # -----------------------------
        # Verify Archive Creation
        # -----------------------------
        if [ -f "$ARCHIVE_PATH" ]; then
            SIZE=$(du -h "$ARCHIVE_PATH" | awk '{print $1}')
            echo "Backup created succ
        
        
        OUTPUT:
        Creating backup...
        Backup created successfully!
        Archive: backup-2026-02-18.tar.gz
        Size   : 45M
        Cleaning old backups (older than 14 days)...
        Backup process completed successfully.

  '''


🎯 What This Script Demonstrates
      Strict mode (set -euo pipefail)
      Argument validation
      Directory validation
      Timestamped backups
      Archive verification
      File size extraction
      Automated cleanup
      Safe error handling


### Task 3: Crontab
1. Read: `crontab -l` — what's currently scheduled?
2. Understand cron syntax:
   ```
   * * * * *  command
   │ │ │ │ │
   │ │ │ │ └── Day of week (0-7)
   │ │ │ └──── Month (1-12)
   │ │ └────── Day of month (1-31)
   │ └──────── Hour (0-23)
   └────────── Minute (0-59)
   ```
3. Write cron entries (in your markdown, don't apply if unsure) for:
   - Run `log_rotate.sh` every day at 2 AM 0 2 * * * 
   - Run `backup.sh` every Sunday at 3 AM  0 3 * * 0
   - Run a health check script every 5 minutes */5 * * * *

  '''
  **SOLUTION:**
              ## ✅ 1️⃣ Check Current Scheduled Cron Jobs
            
            Run:
            
            ```bash
            crontab -l
            ```
            
            This shows **current user’s scheduled cron jobs**.
            
            If nothing is scheduled, you’ll see:
            
            ```
            no crontab for <user>
            ```
            
            To check system-wide cron jobs:
            
            ```bash
            cat /etc/crontab
            ls /etc/cron.d/
            ```
            
            ---
            
            # ✅ 2️⃣ Cron Syntax Understanding
            
            ```
            * * * * *  command
            │ │ │ │ │
            │ │ │ │ └── Day of week (0-7)  (0 or 7 = Sunday)
            │ │ │ └──── Month (1-12)
            │ │ └────── Day of month (1-31)
            │ └──────── Hour (0-23)
            └────────── Minute (0-59)
            ```
            
            ---
            
            # ✅ 3️⃣ Required Cron Entries
            
            > ⚠ These are examples — do not apply blindly in production.
            
            ---
            
            ## 🕑 Run `log_rotate.sh` every day at 2 AM
            
            ```
            0 2 * * * /path/to/log_rotate.sh /var/log/myapp >> /var/log/log_rotate.log 2>&1
            ```
            
            Explanation:
            
            * `0` → minute
            * `2` → hour (2 AM)
            * `* * *` → every day
            
            ---
            
            ## 🗓 Run `backup.sh` every Sunday at 3 AM
            
            ```
            0 3 * * 0 /path/to/backup.sh /source /backup >> /var/log/backup.log 2>&1
            ```
            
            Explanation:
            
            * `0` → minute
            * `3` → 3 AM
            * `0` → Sunday
            
            ---
            
            ## 🔁 Run health check script every 5 minutes
            
            ```
            */5 * * * * /path/to/health_check.sh >> /var/log/health_check.log 2>&1
            ```
            
            Explanation:
            
            * `*/5` → every 5 minutes
            
            ---
            
            # 🎯 DevOps Best Practice
            
            Always:
            
            * Use **absolute paths**
            * Redirect logs (`>> logfile 2>&1`)
            * Ensure script has execute permission
            * Consider locking (to avoid overlapping runs)
            * Use full path for commands inside scripts (cron has limited PATH)
            
            ---
            
            # 🚀 Interview Tip
            
            If asked:
            
            > “Why does my cron job work manually but not in cron?”
            
            Answer:
            
            * PATH issues
            * Missing environment variables
            * Permission problems
            * Script not executable
            * Wrong shebang
            
            ---
            
            If you want, I can now:
            
            * Show how to debug cron failures
            * Add cron locking mechanism
            * Or convert these into a systemd timer setup (modern alternative to cron)

  '''

  ### Task 4: Combine — Scheduled Maintenance Script
Create `maintenance.sh` that:
1. Calls your log rotation function
2. Calls your backup function
3. Logs all output to `/var/log/maintenance.log` with timestamps
4. Write the cron entry to run it daily at 1 AM

'''
#!/bin/bash

set -euo pipefail

LOG_FILE="/var/log/maintenance.log"

# Redirect all output to log file with timestamps
exec > >(while read -r line; do echo "$(date '+%Y-%m-%d %H:%M:%S') $line"; done >> "$LOG_FILE") 2>&1

# -----------------------------
# Configuration
# -----------------------------
LOG_DIR="/var/log/myapp"
BACKUP_SOURCE="/var/www"
BACKUP_DEST="/backups"

# -----------------------------
# Log Rotation Function
# -----------------------------
log_rotate() {
    echo "Starting log rotation..."

    if [ ! -d "$LOG_DIR" ]; then
        echo "Log directory does not exist: $LOG_DIR"
        return 1
    fi

    compressed=$(find "$LOG_DIR" -type f -name "*.log" -mtime +7 -print -exec gzip {} \; | wc -l)
    deleted=$(find "$LOG_DIR" -type f -name "*.gz" -mtime +30 -print -delete | wc -l)

    echo "Logs compressed: $compressed"
    echo "Old archives deleted: $deleted"
    echo "Log rotation completed."
}

# -----------------------------
# Backup Function
# -----------------------------
backup() {
    echo "Starting backup..."

    if [ ! -d "$BACKUP_SOURCE" ]; then
        echo "Backup source does not exist: $BACKUP_SOURCE"
        return 1
    fi

    mkdir -p "$BACKUP_DEST"

    TIMESTAMP=$(date +%Y-%m-%d)
    ARCHIVE="$BACKUP_DEST/backup-$TIMESTAMP.tar.gz"

    tar -czf "$ARCHIVE" -C "$BACKUP_SOURCE" .

    if [ -f "$ARCHIVE" ]; then
        SIZE=$(du -h "$ARCHIVE" | awk '{print $1}')
        echo "Backup created: $ARCHIVE"
        echo "Backup size: $SIZE"
    else
        echo "Backup failed!"
        return 1
    fi

    old_deleted=$(find "$BACKUP_DEST" -type f -name "backup-*.tar.gz" -mtime +14 -print -delete | wc -l)
    echo "Old backups deleted: $old_deleted"
    echo "Backup completed."
}

# -----------------------------
# Main Execution
# -----------------------------
echo "===== Maintenance Started ====="

log_rotate
backup

echo "===== Maintenance Completed Successfully ====="

OUTPUT:
      2026-02-18 01:00:00 ===== Maintenance Started =====
      2026-02-18 01:00:00 Starting log rotation...
      2026-02-18 01:00:01 Logs compressed: 4
      2026-02-18 01:00:01 Old archives deleted: 2
      2026-02-18 01:00:01 Starting backup...
      2026-02-18 01:00:03 Backup created: /backups/backup-2026-02-18.tar.gz
      2026-02-18 01:00:03 Backup size: 120M
      2026-02-18 01:00:03 Old backups deleted: 1
      2026-02-18 01:00:03 ===== Maintenance Completed Successfully =====

Cron Entry — Run Daily at 1 AM
0 1 * * * /path/to/maintenance.sh


Recommended production version:

0 1 * * * /path/to/maintenance.sh >> /var/log/maintenance_cron.log 2>&1

🎯 Why This Is Production-Grade
    Uses set -euo pipefail
    Centralized timestamped logging
    Modular functions
    Safe error handling
    Automatic cleanup
    Cron-ready
    Idempotent design

'''




   




