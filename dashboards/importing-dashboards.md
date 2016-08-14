# Importing Dashboards into Grafana

The Dashboards in this directory are JSON files that have been exported from Grafana and which can easily be imported into 
your own installation. Note that you'll need to modify the InfluxDB data sources to point to your local instance and ensure that 
your InfluxDB measurements and fields match the queries in the dashboards.

## Sample Dashboards

- [Office Environment Dashboards](office_dashboard.json)
- [Environment Dashboard (Combined Office & Entryway)](environment_dashboard.json)
- [Hub Statistics Dashboard](hub_stats_dashboard.json)
- [Repository Statistics Dashboard](repository_stats_dashboard.json)