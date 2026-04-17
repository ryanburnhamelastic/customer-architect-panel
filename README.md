# Customer Architect Panel Interview

This repository contains everything a candidate needs to prepare for the Elastic Customer Architect panel interview.

## Contents

| File | Description |
|------|-------------|
| [elastic-ca-panel-interview-briefing.md](elastic-ca-panel-interview-briefing.md) | Full interview briefing — read this first |
| [sample-data/apache_access.log](sample-data/apache_access.log) | Apache access log file used in Exercise 1 (Logstash ingestion) |
| [logstash/pipeline.conf](logstash/pipeline.conf) | Logstash pipeline configuration template for Exercise 1 |

## Quick Start

1. Read the [briefing](elastic-ca-panel-interview-briefing.md) in full
2. [Create an Elastic Cloud trial](https://cloud.elastic.co/) and save your credentials + Cloud ID
3. Choose Exercise 1 or Exercise 2 for Part 2 of the panel
4. Prepare your Part 1 architecture diagrams separately (any diagramming tool is fine)

## Exercise 1 Setup Summary

```
Elastic Cloud (trial)
  └── Elasticsearch + Kibana
        ├── Elastic Agent   →  System metrics (CPU, memory)
        ├── Logstash        →  Apache access logs  ← sample-data/apache_access.log
        └── APM             →  Spring Pet Clinic traces
```

See the [pipeline config](logstash/pipeline.conf) for a starting point on the Logstash ingestion step.
