# Grafana Agent

This sets up the grafana agent to send metrics to a remote prometheus `remote_write` compatible endpoint.
So far, only the integrated node_exporter is configured.

## Config options

* `grafana_agent_prometheus_scrapes`: Allows you to pass through Prometheus scraping blocks
* `grafana_agent_logs_scrape`: Allows you to pass through loki scraping blocks
* `grafana_agent_blackbox_targets`: Allows you to pass through blackbox scraping blocks

Note: Both options also exist with `_host`, `_group_a`, `_group_b`, `_group_c` and `_special` suffixes.
All values are merged together in the role - this allows you to sidestep group var merging issues.

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

## Blackbox (Website probes)

Set `grafana_agent_enable_blackbox` to `true` and add the targets you want to
be probed in `grafana_agent_blackbox_targets`, e.g:

    grafana_agent_blackbox_targets:
      - name: example
        address: example.com
        module: http_2xx

The module must be a [blackbox module](https://github.com/prometheus/blackbox_exporter/blob/master/CONFIGURATION.md) defined in `grafana_agent_blackbox_module_config`.


## Postgres exporter

Follow the [official documentation](https://github.com/prometheus-community/postgres_exporter#running-as-non-superuser)
on how to configure a separate user for the agent. This will configure the DB
for user `postgres_exporter`.
Specify database `postgres` and your user (e.g. `postgres_exporter`) in
`grafana_agent_prometheus_postgresexporter_datasourcenames`:

    "psql://postgres_exporter:password@example.com/postgres"
