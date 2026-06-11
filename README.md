# SIEM-Log-Analysis-ELK
# Investigating with ELK: SOC Analyst Case Study

## Scenario
Security Operations Center (SOC) teams frequently utilize the Elastic Stack (ELK) as a SIEM solution to store, search, and visualize log data. This case study documents the setup, architecture comprehension, and subsequent log analysis using ELK to detect anomalies.

## Objective
To navigate the Elastic Stack environment, understand its data ingestion pipeline, and utilize Kibana for log filtering, threat hunting, and visual dashboard creation.

## Tools Used
* Elastic Stack (Elasticsearch, Logstash, Kibana)
* Data Shippers (Beats)

## Skills Demonstrated
* SIEM Architecture Comprehension
* Log Pipeline Analysis

---

## Investigation Process

### Task 1: Introduction to Elastic Stack
**Objective:** Establish the baseline purpose of ELK within a security context.
* **Overview:** While initially designed for application performance monitoring, ELK is deployed by SOC teams for security operations. Its ability to collect data from any source, search it, and visualize it in real-time makes it highly effective for log investigations.

### Task 2: Elastic Stack Component Overview
**Objective:** Map the log data pipeline from endpoint collection to analyst visibility.
* **Component Breakdown:**
  * **Beats:** Host-based agents deployed on endpoints (e.g., Winlogbeat for Windows logs, Packetbeat for network traffic). They act as data shippers, pushing local logs into the pipeline.
  * **Logstash:** The data processing engine. It utilizes a configuration file split into three sections: *Input* (defining the source), *Filter* (parsing and normalizing the raw logs), and *Output* (routing the data to the database or interface).
  * **Elasticsearch:** The core full-text search and analytics engine. It stores the parsed data as JSON-formatted documents, allowing analysts to perform rapid queries and correlations via a RESTful API.
  * **Kibana:** The web-based visualization tool. This is the primary interface for an analyst to investigate data streams, build dashboards, and interact with the data stored in Elasticsearch.
* **Data Flow Architecture:** Endpoints (Beats) -> Data Processing (Logstash) -> Database Indexing (Elasticsearch) -> Analyst Interface (Kibana).

---
### Task 3: Lab Connection & Environment Setup
**Objective:** Establish secure access to the target Elastic Stack instance.
* **Action:** Deployed the simulated ELK instance and connected to the Kibana interface via the provided lab environment.
* **Notes:** Successfully authenticated into the Kibana dashboard using the provided analyst credentials. Verified that the web interface was responsive and ready for log querying and threat hunting in the subsequent phases of the investigation.

---
### Task 4: Log Triage & Baseline Analysis (Discover Tab)
**Objective:** Utilize Kibana's Discover interface to triage VPN logs, establish normal traffic baselines, and identify initial visual anomalies.
* **Action:** Interacted with raw log data to filter out noise, build custom data tables, and extract initial metrics regarding user access patterns.
* **Process:**
  * **Index Configuration:** Selected the `vpn_connections` index pattern to specifically target the logs relevant to the investigation.
  * **Time Filtering:** Adjusted the time filter from December 31, 2021, to February 2, 2022, capturing a total baseline of 2,861 connection events.
  * **Visual Triage (Timeline):** Analyzed the event count timeline and identified a distinct log spike occurring on January 11th, marking it for further investigation.
  * **Data Structuring:** Converted the raw JSON log outputs into a clean, readable table by isolating key fields (`IP`, `UserName`, and `Source_Country`). This significantly reduced noise and focused the view on actionable data.
  * **Advanced Filtering:** Applied combined boolean logic to isolate specific behaviors, such as tracking connections from a specific high-volume IP (`238.163.231.224`) while actively excluding expected traffic from the state of New York.
* **Initial Investigation Findings:**
  * **High-Volume Indicators:** Identified user `James` as responsible for the overall maximum traffic, and IP `238.163.231.224` as generating the maximum number of connections.
  * **Spike Attribution:** Traced the anomalous log spike on January 11th directly to source IP `172.201.60.191`.
  * **Targeted Correlation:** Correlated user `Emanda`'s activity to find maximum hits originating from source IP `107.14.1.247`.

*(Note: Add a screenshot of the clean table you created or the timeline spike here: `![Kibana Timeline Spike](images/timeline-spike.png)`)*
