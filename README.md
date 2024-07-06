## Introduction
SplunkLogGuardian is a robust and automated script designed to monitor and manage log entries in Splunk. It ensures the timely and efficient detection of critical log events, enhances security posture, and maintains the integrity of log data. By leveraging the powerful search capabilities of Splunk, this script periodically checks for specific log entries within a defined timeframe and takes appropriate actions if logs are missing or certain conditions are met.
## Features
**Automated Log Monitoring:** Periodically searches Splunk for log entries within the past 5 minutes to ensure continuous log availability and integrity.

**Retry Mechanism:** Implements a retry mechanism with configurable intervals and maximum retries to handle transient issues.

**Remote Splunk Management:** Automatically restarts the Splunk service on a remote host if log entries are not found after maximum retries.

**Remote Splunk Management:** Performs an additional restart of the local Splunk service if the issue persists post remote restart.

**Detailed Logging:** Maintains a comprehensive log file documenting all actions, retries, and status updates for audit and troubleshooting purposes.

## Installation
**Clone the repository to your local machine:**

-- git clone https://github.com/AmirhosseinArjomandi/SplunkLogGuardian.git --

-- cd SplunkLogGuardian --

Ensure that your Splunk credentials are securely stored in environment variables:

-- **export SPLUNK_USER="your_username"** --

-- **export SPLUNK_PASS="your_password"** --

## Configuration

Edit the script to configure the following variables as needed:

**SPLUNK_HOME:** Define the Splunk installation directory.

**SEARCH_QUERY:** Specify the Splunk search query to monitor specific logs.

**RETRY_INTERVAL:** Set the interval (in seconds) between each retry attempt.

**MAX_RETRIES:** Define the maximum number of retry attempts before taking corrective action.

**LOG_FILE:** Specify the path to the log file for recording script activities.

**REMOTE_HOST:** Provide the IP address of the remote host where the Splunk service needs to be managed.

**REMOTE_SPLUNK_PATH:** Set the path to the Splunk executable on the remote host.


## Important Note:

To enable remote operations, you must generate an SSH key and add it to the target machine. This allows the script to restart the Splunk service remotely.


# License

**This project is licensed under the MIT License.**

