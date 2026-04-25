# Splunk 

## What is Splunk?
- Splunk is a SIEM tool used to collect, store, search, and analyze machine data (logs)  
- it helps in monitoring, detection, and investigation of security events  

---

## Splunk Architecture 

### Core Components
- forwarder  
- indexer  
- search head  

---

### Forwarder 
- forwarder is a lightweight agent installed on endpoints to collect data and send it to Splunk  
- it collects logs from systems like Windows, Linux, applications, etc  
- it sends data to the indexer  
- it uses very few system resources and does not impact performance  


### Indexer
- indexer is the core component that processes incoming data  
- it receives data from forwarders  
- it parses and normalizes the data into field value pairs  
- it stores the data as events in indexes  
- indexed data becomes searchable and ready for analysis  



### Search Head
- search head is where users interact with Splunk  
- it is used to search, analyze, and visualize data  
- it sends search queries to the indexer and receives results  

- searches are written using SPL (Search Processing Language)  

- it allows users to:
  - create dashboards  
  - generate reports  
  - visualize data (bar chart, pie chart, etc)  

---

## Data Flow in Splunk


Forwarder → Indexer → Search Head


- forwarder collects logs  
- indexer processes and stores logs  
- search head is used to query and analyze data  

---

## Basic SPL (Search Processing Language)

- SPL is used to search and analyze data in Splunk  

example:

index=main


- search logs from a specific index  


index=main error


- search logs containing the word "error"  


index=main | stats count by source


- count events grouped by source  

---

## Why Splunk is used
- centralized log analysis  
- real time monitoring  
- detection of suspicious activity  
- investigation and troubleshooting  

---

## Key Points
- Splunk collects, processes, and analyzes logs  
- forwarder collects data  
- indexer processes and stores data  
- search head is used for searching and visualization  
- SPL is used to query data  
- Splunk helps in detection and analysis of security events  