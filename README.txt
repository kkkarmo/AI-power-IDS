text
# AI-Powered Intrusion Detection System (IDS)

## Introduction

This project is an AI-powered Intrusion Detection System (IDS) that leverages advanced machine learning models and external cybersecurity data sources to analyze network logs and detect potential threats. The system integrates with Suricata for log collection, utilizes a local LLaMA2 language model for analysis, and enriches the data with insights from VirusTotal and Tavily APIs. The results are stored in a PostgreSQL database for further investigation and reporting.

## Features

- **Real-time Log Processing**: Continuously monitors and processes logs from Suricata.
- **AI Analysis**: Uses a local LLaMA2 model to analyze log data and detect anomalies.
- **External Data Enrichment**: Queries VirusTotal and Tavily APIs for additional cybersecurity insights.
- **Database Storage**: Stores analysis results in a PostgreSQL database for easy access and reporting.
- **Public IP Extraction**: Monitors `eve.json` to extract public IPs and writes them to `Public_IPs.txt`.
- **Dockerized Deployment**: Easily deployable using Docker for a consistent and isolated environment.

## Installation

### Prerequisites

- Docker installed on your system.
- A `.env` file with the following environment variables:
  - `DB_NAME`
  - `DB_USER`
  - `DB_PASSWORD`
  - `DB_HOST`
  - `DB_PORT`
  - `TAVILY_API_KEY`
  - `VIRUSTOTAL_API_KEY`
  - `LOG_FILE_PATH`
  - `VT_RESULT_FILE_PATH`
  - `CHECKED_IPS_FILE`
  - `PUBLIC_IPS_FILE`

### Build and Run the Docker Container

1. **Clone the Repository**:
   ```bash
   git clone https://github.com/yourusername/ai-ids.git
   cd ai-ids

Build the Docker Image:
bash
docker build -t ai-ids .

Run the Docker Container:
bash
docker run -d --name ai-ids-container --env-file .env -v $(pwd)/data:/usr/src/app/data ai-ids

Replace $(pwd)/data with the path to your data directory containing eve.json, Public_IPs.txt, and other necessary files.
Usage
The Docker container will automatically start processing logs and querying APIs based on the schedule defined in the script. It will:
Extract public IPs from eve.json continuously.
Check IPs through VirusTotal API daily at midnight.
Perform Tavily analysis every Monday at midnight.
Access Logs and Results
Logs: Check debug.log for detailed logs of the system's operations.
Results: Analysis results are stored in the PostgreSQL database configured in your .env file.
Use Case Scenario
Sample Input
Log File: A Suricata log file (eve.json) with network events.
Public IPs: Automatically extracted from eve.json and saved to Public_IPs.txt.
Sample Output
VirusTotal Results: vt_results.txt containing analysis results for each IP, such as:
text
IP: 192.168.1.1, Malicious: 2, Suspicious: 1
IP: 10.0.0.5, Error: Unable to fetch results, status code: 404

Tavily Analysis: Additional cybersecurity insights for flagged IPs, such as:
text
IP: 192.168.1.1
Query: Why is IP 192.168.1.1 flagged as malicious or suspicious?
Answer: This IP is flagged due to multiple reports of malicious activity...
Top Results:
- Title: Known Malware Source
  Content: This IP has been associated with malware distribution...

Database Entries: The PostgreSQL database will have tables log_analysis and results populated with analysis data.
Contributing
Contributions are welcome! Please open an issue or submit a pull request with your improvements.
License
This project is licensed under the MIT License. See the LICENSE file for details.
Contact
For questions or support, please contact your-email@example.com.
text

### Updated Dockerfile

```dockerfile
# Use an official Python runtime as a parent image
FROM python:3.9-slim

# Set the working directory in the container
WORKDIR /usr/src/app

# Copy the current directory contents into the container at /usr/src/app
COPY . .

# Install any needed packages specified in requirements.txt
RUN pip install --no-cache-dir -r requirements.txt

# Make port 80 available to the world outside this container
EXPOSE 80

# Define environment variable
ENV NAME World

# Run the script when the container launches
CMD ["python", "./your_script_name.py"]