# Grafana Agent

This sets up the grafana agent to send metrics to a remote prometheus `remote_write` compatible endpoint.
So far, only the integrated node_exporter is configured.

## Config options

* `grafana_agent_prometheus_scrapes`: Allows you to pass through Prometheus scraping blocks

## Postgres exporter

Follow the [official documentation](https://github.com/prometheus-community/postgres_exporter#running-as-non-superuser)
on how to configure a separate user for the agent. This will configure the DB
for user `postgres_exporter`.
Specify database `postgres` and your user (e.g. `postgres_exporter`) in
`grafana_agent_prometheus_postgresexporter_datasourcenames`:

    "psql://postgres_exporter:password@example.com/postgres"
