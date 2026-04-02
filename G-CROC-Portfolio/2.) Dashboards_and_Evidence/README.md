# 📊 Dashboards & Visual Intelligence (Workbooks)

### Overview
This directory contains the visual layer of the G-CROC project. It includes the **G-CROC Azure Monitor Workbook**, which provides real-time situational awareness for SOC analysts.

### Visual Capabilities
* **Global Threat Landscape:** A Geo-IP heatmap tracking inbound RDP brute-force attempts across the globe.
* **Geopolitical Targeting:** A correlation matrix showing which countries are targeting specific high-privilege accounts (e.g., Administrator, root).
* **Network Reconnaissance:** Pie charts and bar graphs detailing the most frequent destination ports (80, 445, 1433, 3306) probed by external scanners.

### Folder Contents
* `G-CROC_Dashboard_Workbook.json`: The raw JSON ARM template to deploy the entire visual dashboard.
* `Architecture_Diagram.png`: The full-scale, 6-zone architectural blueprint for the project.
