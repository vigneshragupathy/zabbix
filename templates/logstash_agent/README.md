# Logstash by Zabbix agent

## Overview

This template is to monitor Logstash by Zabbix that works without any external scripts.

Most of the metrics are collected in one go, thanks to Zabbix bulk data collection.

This Template collects metrics by polling Logstash Node Stats Page with Zabbix agent.

```
curl -X GET http://localhost:9600/_node/stats?pretty
```

## Zabbix configuration

No specific Zabbix configuration is required.

## Macros used

| Name | Description | Default |
| --- | --- | --- |
| {$LOGSTASH.HOST} | Hostname or IP Address of Logstash | localhost |
| {$LOGSTASH.STATS.PATH} | The path to Logstash stats page | _node/stats |
| {$LOGSTASH.STATS.PORT} | The port of Logstash. | 9600 |
| {$LOGSTASH.STATS.SCHEME} | The scheme of logstash node stats page(http/https) | http |

## Discovery rules

| Name | Description | Type | Key and additional info |
| --- | --- | --- | --- |
| Pipeline discovery | Discovery pipelines | DEPENDENT | logstash.pipeline.discovery<br>**Preprocessing steps**<br>1\. Json path<br>2\. Javascript function<br>**LLD Macros**<br>1\. {#PIPELINE} |

## Items collected

| Group | Name | Description | Type | Key and additional info |
| --- | --- | --- | --- | --- |
| Logstash: Node stats | Logstash: Version | -   | DEPENDENT | logstash.version<br>**Preprocessing**:<br>\- JSONPath: $.version<br>\- DISCARD\_UNCHANGED\_HEARTBEAT: 1d |
| Logstash: Node stats | Logstash: Batch delay | -   | DEPENDENT | logstash.batch\_delay<br>**Preprocessing**:<br>\- JSONPath: $.pipeline.batch_delay |
| Logstash: Node stats | Logstash: Batch size | -   | DEPENDENT | logstash.batch\_size<br>**Preprocessing**:<br>\- JSONPath: $.pipeline.batch_size |
| Logstash: Node stats | Logstash: JVM threads count | -   | DEPENDENT | logstash.jvm.threads.count<br>**Preprocessing**:<br>\- JSONPath: $.jvm.threads.count |
| Logstash: Node stats | Logstash: JVM threads peak count | -   | DEPENDENT | logstash.jvm.threads.peak\_count<br>**Preprocessing**:<br>\- JSONPath: $.jvm.threads.peak_count |
| Logstash: Node stats | Logstash: JVM uptime | -   | DEPENDENT | logstash.jvm.uptime\_in_millis<br>**Preprocessing**:<br>\- JSONPath: $.jvm.uptime\_in\_millis<br>\- Custom multiplier: 0.001 |
| Logstash: Node stats | Logstash: Node stats | -   | ZABBIX_PASSIVE | web.page.get\["{KaTeX parse error: Expected 'EOF', got '}' at position 25: …SH.STATS.SCHEME}̲://{IA\_LOGSTASH.HOST}:{KaTeX parse error: Expected 'EOF', got '}' at position 23: …TASH.STATS.PORT}̲/{IA\_LOGSTASH.STATS.PATH}?pretty"\]<br>**Preprocessing**:<br>\- Regular expression: \\n\\s?\\n(\[\\s\\S\]*) |
| Logstash: Node stats | Logstash: process total virutal memory | -   | DEPENDENT | logstash.get.process.mem<br>**Preprocessing**:<br>\- JSONPath : $.process.mem.total\_virtual\_in_bytes |
| Logstash: Service status | Logstash: Service status | -   | ZABBIX_PASSIVE | net.tcp.service\["{KaTeX parse error: Expected 'EOF', got '}' at position 26: …SH.STATS.SCHEME}̲","{IA\_LOGSTASH.HOST}","{$IA\_LOGSTASH.STATS.PORT}"\]<br>**Preprocessing**:<br>\- Discard unchanged with heartbeat : 600 |
| Logstash: Node stats | Logstash: Status | -   | DEPENDENT | logstash.status<br>**Preprocessing**:<br>\- JSONPath : $.status |
| Logstash: Node stats | Logstash: workers | -   | DEPENDENT | logstash.workers<br>**Preprocessing**:<br>\- JSONPath : $.pipeline.workers |
| Logstash: Node stats | Logstash {#PIPELINE} pipeline - DLQ queue\_size\_in_bytes | -   | DEPENDENT | logstash.pipeline.dlq.queue\_size\_in\_bytes\[{#PIPELINE}\]<br>**Preprocessing**:<br>\- JSONPath : $.pipelines.\['{#PIPELINE}'\].dead\_letter\_queue.queue\_size\_in_bytes |
| Logstash: Node stats | Logstash {#PIPELINE} pipeline - events filtered | -   | DEPENDENT | logstash.pipeline.events.filtered\[{#PIPELINE}\]<br>**Preprocessing**:<br>\- JSONPath : $.pipelines.\['{#PIPELINE}'\].events.filtered |
| Logstash: Node stats | Logstash {#PIPELINE} pipeline - events in | -   | DEPENDENT | logstash.pipeline.events.in\[{#PIPELINE}\]<br>**Preprocessing**:<br>\- JSONPath : $.pipelines.\['{#PIPELINE}'\].events.in |
| Logstash: Node stats | Logstash {#PIPELINE} pipeline - events out | -   | DEPENDENT | logstash.pipeline.events.out\[{#PIPELINE}\]<br>**Preprocessing**:<br>\- JSONPath : $.pipelines.\['{#PIPELINE}'\].events.out |
| Logstash: Node stats | Logstash {#PIPELINE} pipeline - events push_duration | -   | DEPENDENT | logstash.pipeline.events.queue\_push\_duration\_in_millis\[{#PIPELINE}\]<br>**Preprocessing**:<br>\- JSONPath : $.pipelines.\['{#PIPELINE}'\].events.queue\_push\_duration\_in\_millis \- Custom Multiplier: 0.001 |
| Logstash: Node stats | Logstash {#PIPELINE} pipeline - queue max size | -   | DEPENDENT | logstash.pipeline.queue.max\_queue\_size\_in_bytes\[{#PIPELINE}\]<br>**Preprocessing**:<br>\- JSONPath : $.pipelines.\['{#PIPELINE}'\].queue.max\_queue\_size\_in\_bytes |
| Logstash: Node stats | Logstash {#PIPELINE} pipeline - queue size | -   | DEPENDENT | logstash.pipeline.queue.queue\_size\_in\_bytes\[{#PIPELINE}\]<br>**Preprocessing**:<br>\- JSONPath : $.pipelines.\['{#PIPELINE}'\].queue.queue\_size\_in_bytes |
| Logstash: Node stats | Logstash {#PIPELINE} pipeline - events processing success rate | -   | CALCULATED | logstash.pipeline.events_success_rate[{#PIPELINE}]<br>**Formula**:<br>\- Expression : 100*((last(//logstash.pipeline.events.out[{#PIPELINE}])+ 1)/(last(//logstash.pipeline.events.in[{#PIPELINE}]) + 1)) |

## Triggers
```
in progress
```
