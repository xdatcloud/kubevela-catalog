# Vector Config

Vector is a lightweight, ultra-fast tool for building observability pipelines.

This addon provides a set of components/traits to simplify the usage of vector.

The config models from this addon aim to make users define their pipelines in a use-case driven way.

Users DO NOT have to pay attentions to the details about Vector's Pipelines, open the hood to see what happened and how it works.

## Components

- vector-log2metric - A generic config model that support collect logs from files or pods, extract metrics with specified strategy, and send metrics to prometheus.

## Demo

The demo example contains two resources:

- An Application CR that shows how to write a simple log2metric config with some fake data
- A Pod that runs a Vector instance in it to receive config above, and print result of log2metric to its stdout

It is a fast && convenient way to explore how to do log2metric. 

First, apply the demo resources:

`kubectl apply -f examples/vector-config/log2metric-demo.yaml`

Wait for everything is ready, then you can observe the result of log2metric in Vector's console:

`kubectl logs -f --tail=10 vector-playground`

Feel free to edit the yaml, put in some log data that from your real applications, and try to generate some metrics.

## Config Model: log2metric

This config model is implemented by component `vector-log2metric`

A log2metric config generally contains four stages:

- source: defines where to collect logs
  - this stage MUST BE defined
- parsing: defines how to parse logs line-by-line
  - this stage MUST BE defined
- stream processing: defines how to aggregate metrics from parsed logs
  - this stage IS OPTIONAL if your were just debugging in parsing stage 
- metrics sinks: defines how to expose metrics to external, with exporter or remote-write ways
  - this stage IS OPTIONAL if your were just debugging in above stages

And, there are some configurations to enable and control the scraping of Vector's internal metrics for self monitoring.

### Source

Example of source "file":

```yaml
source: "file"

# [files] maps to Vector's `include` configuration, you can learn more from:
# - https://vector.dev/docs/reference/configuration/sources/file/#include
# - https://vector.dev/docs/reference/configuration/sources/file/#globbing
files:
  - /path/to/your.log
  - /it/supports/glob/*.log

# [ignoreCheckpoints] maps to Vector's `ignore_checkpoints` configuration, you can learn more from:
# - https://vector.dev/docs/reference/configuration/sources/file/#ignore_checkpoints
# - https://vector.dev/docs/reference/configuration/sources/file/#checkpointing
# - https://vector.dev/docs/reference/configuration/sources/file/#read-position
#ignoreCheckpoints: true # Uncomment this line to ignore checkpoints

# [readFrom] maps to Vector's `read_from` configuration, you can learn more from:
# - https://vector.dev/docs/reference/configuration/sources/file/#read_from
# - https://vector.dev/docs/reference/configuration/sources/file/#read-position
#readFrom: beginning # Uncomment this line to collect logs from the beginning of files
```

Example of source "pod":

```yaml
# By default, Vector will collect stdout logs from ALL pods in current K8s cluster
# PLEASE SPECIFY AT LEAST ONE SELECTOR TO AVOID PERFORMANCE PROBLEM
# REMEMBER, YOU ARE JUST DOING LOG2METRIC
source: "pod"

# [podLabelSelector] maps to Vector's `extra_label_selector` configuration, your can learn more from:
# - https://vector.dev/docs/reference/configuration/sources/kubernetes_logs/#extra_label_selector
# - https://kubernetes.io/docs/concepts/overview/working-with-objects/labels/
#podLabelSelector: app=yourApplication # Uncomment this line to enable label selector

# [podFieldSelector] maps to Vector's `extra_field_selector` configuration, your can learn more from:
# - https://vector.dev/docs/reference/configuration/sources/kubernetes_logs/#extra_field_selector
# - https://kubernetes.io/docs/concepts/overview/working-with-objects/field-selectors/
#podFieldSelector: metadata.namespace==yourNamespace # Uncomment this line to enable field selector
```

Example of source "demo":

```yaml
source: "demo"
# Put some fake data here
demoLogs:
  - '{"pet": "dog"}'
  - '{"pet": "dog"}'
  - '{"pet": "cat"}'
  - '{"pet": "parrot"}'
#demoIntervalSecs: 0.1 # Uncomment this line to increase or decrease the interval time to flush demo logs
#demoInSequence: true # Uncomment this line to flush demo logs in sequence, by default it is in shuffle
#demoTotalCount:  100 # Uncomment this line to set the maximum occurrences of demo logs
```

### Parsing

TODO...

### Stream Processing

TODO...

### Metrics Sinks

TODO...

### Internal Metrics
