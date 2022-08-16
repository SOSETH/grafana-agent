# Grafana Agent

This sets up the grafana agent to send metrics to a remote prometheus `remote_write` compatible endpoint.
So far, only the integrated node_exporter is configured.

## Config options

* `grafana_agent_prometheus_scrapes`: Allows you to pass through Prometheus scraping blocks

## Logs

You can enable log-collection using the integrated Promtail.
For this, the variables `grafana_agent_logs_scrape_config` and
`grafana_agent_logs_client_url` must be set. The latter is a Loki-endpoint to
which the logs will be sent. An example for `grafana_agent_logs_scrape_config`
is:

    grafana_agent_logs_scrape_config:
      - job_name: varlogs
        static_configs:
          - targets: [localhost]
            labels:
              job: varlogs
              __path__: /var/log/*log
              hostname: "{{ ansible_fqdn }}"

## Postgres exporter

Follow the [official documentation](https://github.com/prometheus-community/postgres_exporter#running-as-non-superuser)
on how to configure a separate user for the agent. This will configure the DB
for user `postgres_exporter`.
Specify database `postgres` and your user (e.g. `postgres_exporter`) in
`grafana_agent_prometheus_postgresexporter_datasourcenames`:

    "psql://postgres_exporter:password@example.com/postgres"
