# Custom Logs Package

The Custom Logs package is used for ingesting arbitrary log files and manipulating their content/lines by using Ingest Pipelines configuration.

In order to use the package, please follow these steps:

1. [Setup / Install Elastic Agent](https://www.elastic.co/guide/en/fleet/current/install-fleet-managed-elastic-agent.html) at the machine where the logs should be collected from
2. Identify the log location at that machine e.g. `/tmp/custom.log`. Note that `/var/log/*.log` is fully ingested by the [System](https://docs.elastic.co/en/integrations/system), no need to add this path if the [System](https://docs.elastic.co/en/integrations/system) integration is already used
3. Enroll Custom Logs integration and add it to the installed agent. Give the dataset a name that fits to the log purpose, e.g. `python` for logs from a Python app. Make sure to configure the path from the step 2
4. Check that the raw log data is coming in via [Discover](https://www.elastic.co/guide/en/kibana/current/discover.html) by filtering the `logs-*` indices to the dataset name given in step 3, e.g. `logs-python`
5. Configure the parsing rules via [Ingest Pipelines](https://www.elastic.co/guide/en/elasticsearch/reference/current/ingest.html), e.g. JSON Parsing or [grok](https://www.elastic.co/blog/slow-and-steady-how-to-build-custom-grok-patterns-incrementally) parsing
6. Create a [custom dashboard](https://www.elastic.co/guide/en/kibana/current/create-a-dashboard-of-panels-with-web-server-data.html) that analyzes the incoming log data for your needs

## Upgrade
This integration has a `Custom Logs (Legacy)` input option that uses the filebeat log input which is deprecated and will be removed in a future version of Elastic Agent. It is recommended to use the `Custom Logs` input option instead.

When migrating from the `Custom Logs (Legacy)` input to the `Custom Logs` input, the following steps are required:
1. Create a new `Custom Logs` input and configure it with the same settings as the `Custom Logs (Legacy)` input.
2. Disable the `Custom Logs (Legacy)` input.

As `Custom Logs` is a new input, configuring it to collect from a file that was previously collected by `Custom Logs (Legacy)` will result in duplicate data. You may wish to configure `ignore_older` or set `ignore_inactive: since_first_start` to limit the amount of duplicate data collected.

## ECS Field Mapping
This integration includes the ECS Dynamic Template, all fields that follows the ECS Schema will get assigned the correct index field mapping and does not need to be added manually.