#!/bin/bash

# Script created by Amirhossein Arjomandi
# Date: July 1, 2024

# Define the Splunk home directory
SPLUNK_HOME="/opt/splunk"

# Define the search query for Splunk
SEARCH_QUERY="index=\"Your Index\" earliest=-5m latest=now"

# Define the interval between retries (120 seconds)
RETRY_INTERVAL=120

# Define the maximum number of retries
MAX_RETRIES=5

# Define the log file path
LOG_FILE="Enter your path for LogFile"

# Define the remote host and Splunk path on the remote host
REMOTE_HOST="Enter IP Address of Targeted host"
REMOTE_SPLUNK_PATH="/opt/splunk/bin/splunk"

# Function to check logs using Splunk search
check_logs() {
    $SPLUNK_HOME/bin/splunk search "$SEARCH_QUERY" -auth "SPLUNK_UI_USERNAME:SPLUNK_UI_PASSWORD"
}

# Function to log messages with a timestamp
log_message() {
    echo "$(date '+%Y-%m-%d %H:%M:%S') - $1" >> $LOG_FILE
}

# Initialize retry counter
retries=0

# Retry loop
while [ $retries -lt $MAX_RETRIES ]; do
    # Get the log count from the Splunk search
    LOG_COUNT=$(check_logs | wc -l)
    echo "Debug: LOG_COUNT=$LOG_COUNT"  # Debug message

    # Check if logs are found
    if [ "$LOG_COUNT" -gt 0 ]; then
        log_message "Logs found in index. Exiting."
        exit 0
    else
        # Log message if no logs found and wait for the retry interval
        log_message "No logs found. Retrying in $RETRY_INTERVAL seconds (Retry $((retries + 1)) of $MAX_RETRIES)..."
        sleep $RETRY_INTERVAL
        retries=$((retries + 1))
    fi
done

# Log message if maximum retries reached and restart the Splunk service on the remote host
log_message "Maximum retries reached. Restarting the Splunk service on remote host ..."
ssh root@$REMOTE_HOST "$REMOTE_SPLUNK_PATH restart"

# Adding a sleep to allow Splunk to restart
sleep 60

# Check logs again after the restart
LOG_COUNT=$(check_logs | wc -l)
echo "Debug: LOG_COUNT after restart=$LOG_COUNT"  # Debug message

# If logs are still not found, perform another restart on remote host
if [ "$LOG_COUNT" -eq 0 ]; then
    log_message "Logs still not found after restart. Performing another restart on remote host..."
    ssh root@$REMOTE_HOST "$SPLUNK_HOME/bin/splunk restart"
fi
