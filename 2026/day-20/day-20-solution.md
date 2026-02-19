## Expected Output
- A Bash script: `log_analyzer.sh`
- A generated summary report: `log_report_<date>.txt`
- A markdown file: `day-20-solution.md` documenting your approach

### Task 1: Input and Validation
Your script should:
1. Accept the path to a log file as a command-line argument
2. Exit with a clear error message if no argument is provided
3. Exit with a clear error message if the file doesn't exist

'''
            #!/bin/bash
            
            set -euo pipefail
            
            # -----------------------------
            # Input Validation
            # -----------------------------
            
            # Check if argument is provided
            if [ $# -ne 1 ]; then
                echo "Usage: $0 <log_file_path>"
                exit 1
            fi
            
            LOG_FILE="$1"
            
            # Check if file exists
            if [ ! -f "$LOG_FILE" ]; then
                echo "Error: Log file '$LOG_FILE' does not exist."
                exit 1
            fi
            
            # -----------------------------
            # Report Setup
            # -----------------------------
            
            DATE=$(date +%Y-%m-%d)
            REPORT_FILE="log_report_${DATE}.txt"
            
            echo "Analyzing log file: $LOG_FILE"
            echo "Generating report: $REPORT_FILE"
            
            # Create report header
            {
                echo "===================================="
                echo "         LOG ANALYSIS REPORT        "
                echo "===================================="
                echo "Log File : $LOG_FILE"
                echo "Date     : $(date)"
                echo
            } > "$REPORT_FILE"
            
            echo "Validation successful. Report initialized."

            OUTPUT:

            ❌ No Argument
              ./log_analyzer.sh


Output:

      Usage: ./log_analyzer.sh <log_file_path>
      
      ❌ File Does Not Exist
      ./log_analyzer.sh missing.log
      
      Output:
      Error: Log file 'missing.log' does not exist.
      
      🎯 What This Demonstrates
      Strict mode (set -euo pipefail)
      Argument count validation ($#)
      File existence check (-f)
      Clean report generation
      Safe variable handling
'''


### Task 2: Error Count
1. Count the total number of lines containing the keyword `ERROR` or `Failed`
2. Print the total error count to the console

---

### Task 3: Critical Events
1. Search for lines containing the keyword `CRITICAL`
2. Print those lines along with their line number


'''
          #!/bin/bash
          
          set -euo pipefail
          
          # -----------------------------
          # Input Validation
          # -----------------------------
          if [ $# -ne 1 ]; then
              echo "Usage: $0 <log_file_path>"
              exit 1
          fi
          
          LOG_FILE="$1"
          
          if [ ! -f "$LOG_FILE" ]; then
              echo "Error: Log file '$LOG_FILE' does not exist."
              exit 1
          fi
          
          # -----------------------------
          # Report Setup
          # -----------------------------
          DATE=$(date +%Y-%m-%d)
          REPORT_FILE="log_report_${DATE}.txt"
          
          {
              echo "===================================="
              echo "         LOG ANALYSIS REPORT        "
              echo "===================================="
              echo "Log File : $LOG_FILE"
              echo "Date     : $(date)"
              echo
          } > "$REPORT_FILE"
          
          echo "Analyzing log file: $LOG_FILE"
          echo
          
          # -----------------------------
          # Task 2: Error Count
          # -----------------------------
          
          ERROR_COUNT=$(grep -Ei "ERROR|Failed" "$LOG_FILE" | wc -l)
          
          echo "Total ERROR/Failed entries: $ERROR_COUNT"
          
          {
              echo "----- Error Summary -----"
              echo "Total ERROR/Failed entries: $ERROR_COUNT"
              echo
          } >> "$REPORT_FILE"
          
          # -----------------------------
          # Task 3: Critical Events
          # -----------------------------
          
          echo "CRITICAL events found:"
          grep -n "CRITICAL" "$LOG_FILE" || echo "No CRITICAL events found."
          
          {
              echo "----- CRITICAL Events -----"
              grep -n "CRITICAL" "$LOG_FILE" || echo "No CRITICAL events found."
              echo
          } >> "$REPORT_FILE"
          
          echo
          echo "Report saved to: $REPORT_FILE"

          **OUTPUT:**
          Analyzing log file: app.log

          Total ERROR/Failed entries: 12
          
          CRITICAL events found:
          45:CRITICAL: Database connection lost
          89:CRITICAL: Disk full
          
          Report saved to: log_report_2026-02-18.txt

          **What This Does**
              🔹 Task 2
                    grep -Ei "ERROR|Failed"
                    -E → extended regex
                    -i → case-insensitive
                    Counts matching lines using wc -l
              🔹 Task 3
                    grep -n "CRITICAL"
                    -n → shows line number
                    Prints directly to console and report

          Example output:
              ```
              --- Critical Events ---
              Line 84: 2025-07-29 10:15:23 CRITICAL Disk space below threshold
              Line 217: 2025-07-29 14:32:01 CRITICAL Database connection lost
              ```
          
'''

### Task 4: Top Error Messages
1. Extract all lines containing `ERROR`
2. Identify the **top 5 most common** error messages
3. Display them with their occurrence count, sorted in descending order

   '''
   # -----------------------------
# Task 4: Top 5 Error Messages
# -----------------------------

echo
echo "Top 5 Most Common ERROR Messages:"

TOP_ERRORS=$(grep "ERROR" "$LOG_FILE" \
    | sed 's/^.*ERROR[: ]*//' \
    | sort \
    | uniq -c \
    | sort -nr \
    | head -5)

if [ -z "$TOP_ERRORS" ]; then
    echo "No ERROR entries found."
else
    echo "$TOP_ERRORS"
fi

{
    echo "----- Top 5 ERROR Messages -----"
    if [ -z "$TOP_ERRORS" ]; then
        echo "No ERROR entries found."
    else
        echo "$TOP_ERRORS"
    fi
    echo
} >> "$REPORT_FILE"


    🔎 How This Works
    Step-by-step:
    grep "ERROR"
    → Extract lines containing ERROR
    sed 's/^.*ERROR[: ]*//' 
    → Removes everything before the word ERROR
    → Keeps only actual error message text
    sort | uniq -c
    → Counts duplicates
    sort -nr
    → Sort numerically (descending)
    head -5
    → Show top 5
    🔎 Example Output
    Top 5 Most Common ERROR Messages:
        15 Database connection failed
        10 Timeout while connecting to API
        7 Invalid user credentials
        4 Disk space low
        2 Permission denied
    
    🎯 Why This Is Powerful
        This mimics real SRE log analysis:
        Detect recurring issues
        Identify top failure causes
        Prioritize fixes
        Spot abnormal spikes

    Example output:
    ```
    --- Top 5 Error Messages ---
    45 Connection timed out
    32 File not found
    28 Permission denied
    15 Disk I/O error
    9  Out of memory
    ```
    '''
### Task 5: Summary Report
Generate a summary report to a text file named `log_report_<date>.txt` (e.g., `log_report_2026-02-11.txt`). The report should include:
1. Date of analysis
2. Log file name
3. Total lines processed
4. Total error count
5. Top 5 error messages with their occurrence count
6. List of critical events with line numbers

---

### Task 6 (Optional): Archive Processed Logs
Add a feature to:
1. Create an `archive/` directory if it doesn't exist
2. Move the processed log file into `archive/` after analysis
3. Print a confirmation message

'''
        #!/bin/bash
        
        set -euo pipefail
        
        # -----------------------------
        # Input Validation
        # -----------------------------
        if [ $# -ne 1 ]; then
            echo "Usage: $0 <log_file_path>"
            exit 1
        fi
        
        LOG_FILE="$1"
        
        if [ ! -f "$LOG_FILE" ]; then
            echo "Error: Log file '$LOG_FILE' does not exist."
            exit 1
        fi
        
        # -----------------------------
        # Variables
        # -----------------------------
        DATE=$(date +%Y-%m-%d)
        REPORT_FILE="log_report_${DATE}.txt"
        
        # -----------------------------
        # Collect Metrics
        # -----------------------------
        
        TOTAL_LINES=$(wc -l < "$LOG_FILE")
        
        ERROR_COUNT=$(grep -Ei "ERROR|Failed" "$LOG_FILE" | wc -l)
        
        TOP_ERRORS=$(grep -i "ERROR" "$LOG_FILE" \
            | sed 's/^.*ERROR[: ]*//' \
            | sort \
            | uniq -c \
            | sort -nr \
            | head -5)
        
        CRITICAL_EVENTS=$(grep -n "CRITICAL" "$LOG_FILE" || true)
        
        # -----------------------------
        # Generate Summary Report
        # -----------------------------
        
        {
            echo "========================================="
            echo "            LOG SUMMARY REPORT           "
            echo "========================================="
            echo "Date of Analysis : $(date)"
            echo "Log File         : $LOG_FILE"
            echo "Total Lines      : $TOTAL_LINES"
            echo "Total Errors     : $ERROR_COUNT"
            echo
        
            echo "----- Top 5 ERROR Messages -----"
            if [ -z "$TOP_ERRORS" ]; then
                echo "No ERROR entries found."
            else
                echo "$TOP_ERRORS"
            fi
            echo
        
            echo "----- CRITICAL Events (with line numbers) -----"
            if [ -z "$CRITICAL_EVENTS" ]; then
                echo "No CRITICAL events found."
            else
                echo "$CRITICAL_EVENTS"
            fi
            echo
        
        } > "$REPORT_FILE"
        
        echo "✅ Report generated: $REPORT_FILE"

        # -----------------------------
        # Task 6: Archive Processed Log
        # -----------------------------
        
        ARCHIVE_DIR="archive"
        
        if [ ! -d "$ARCHIVE_DIR" ]; then
            mkdir "$ARCHIVE_DIR"
        fi
        
        mv "$LOG_FILE" "$ARCHIVE_DIR/"
        
        echo "📦 Log file moved to $ARCHIVE_DIR/"
        echo "Analysis complete."
        
        Example file:
        log_report_2026-02-18.txt

'''

✅ What This Script Now Does
        Task 5 – Summary Report Includes:
        
        ✔ Date of analysis
        ✔ Log file name
        ✔ Total lines processed
        ✔ Total error count
        ✔ Top 5 error messages
        ✔ CRITICAL events with line numbers
        
        
        Task 6 – Archive Feature
        
        ✔ Creates archive/ directory if missing
        ✔ Moves processed log into archive
        ✔ Prints confirmation message
        
        After execution:
        
        archive/app.log
        log_report_2026-02-18.txt

---

## Sample Log File

A sample log file is available in this directory: `sample_log.log`

You can also pick real-world log datasets from the [LogHub repository](https://github.com/logpai/loghub) to test your script against production-like logs (e.g., ZooKeeper, HDFS, Apache, Linux syslogs).

---




