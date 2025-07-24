#  Brute-Force Detection and Slack Alert System

A Python automation project that parses Linux `auth.log` files to detect brute-force SSH login attempts and sends real-time alerts to Slack using securely stored webhook credentials.

## Project Overview

This project simulates and detects brute-force SSH login attempts through log file analysis. Upon detection, it sends formatted alerts to a designated Slack channel to support security operations and incident response.

## Features

- Parses SSH login attempts from `auth.log` files
- Detects brute-force behavior based on a configurable threshold
- Sends alerts to Slack using Incoming Webhooks
- Loads sensitive credentials securely from environment variables
- Includes troubleshooting steps for PowerShell and Python environments

## Requirements

- Python 3.12 or later
- Dependencies (install via `requirements.txt`):

requests
python-dotenv


Install with:
pip install -r requirements.txt

File Structure

brute-force-detector/
├── auth.log              # Sample input log file
├── detector.py           # Main Python script
├── .env                  # Environment file storing the Slack webhook URL
├── requirements.txt      # Python dependencies
└── README.md             # Project documentation

Slack Integration

    Create a Slack App with Incoming Webhooks enabled.

    Generate a webhook URL for a target channel.

    Store the URL in a .env file as follows:

SLACK_URL=https://hooks.slack.com/services/x/x/x

How to Use

    Ensure auth.log is placed in the project directory.

    Run the detection script:

python detector.py

    If brute-force attempts are detected, a summary will be printed to the console and posted to Slack.

Detection Logic

The script looks for repeated failed SSH login attempts in the following format:

Failed password for root from 203.0.113.10 port 56123 ssh2

Any IP address with failed attempts equal to or above the defined threshold (default is 3) is flagged and reported.
Sample Output

Console:

Brute-force attempts detected from these IP addresses:
203.0.113.10

Slack Message:

Brute-force login attempts detected from the following IP addresses:
203.0.113.10

Troubleshooting

    Script execution policy issues (PowerShell): Run Set-ExecutionPolicy RemoteSigned -Scope Process

    Webhook not working: Ensure the URL is valid and the .env file is properly formatted

    No output: Verify auth.log path and enable debug prints

    Duplicate Slack alerts: Tune detection logic for batch uniqueness

Lessons Learned

    Real-time detection and alerting using Python scripting

    Secure integration with Slack for SOC pipelines

    Practical experience with log parsing and pattern recognition

    Troubleshooting and environment isolation using PowerShell and virtual environments
