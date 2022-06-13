# Grafana Agent

This sets up the grafana agent to send metrics to a remote prometheus `remote_write` compatible endpoint.
So far, only the integrated node_exporter is configured.

## Config options

* `grafana_agent_prometheus_scrapes`: Allows you to pass through Prometheus scraping blocks