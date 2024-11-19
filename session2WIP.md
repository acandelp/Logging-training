## Session 2. 


### 1)Architecture

- [New Commers Check List](https://docs.google.com/spreadsheets/d/1M5DT-GiFVJt9PYAjPnMQBRCvm6tKu4KurtkRzEg0LLA/edit?gid=1234451132#gid=1234451132) Introduction to LokiStack
- [Upstream documentation](https://grafana.com/docs/loki/latest/get-started/architecture/)


### 2)[Installation](https://docs.openshift.com/container-platform/4.16/observability/logging/log_storage/installing-log-storage.html#logging-loki-gui-install_installing-log-storage)

- [Configuring the logging Collector](https://docs.openshift.com/container-platform/4.14/observability/logging/log_collection_forwarding/cluster-logging-collector.html)

[Tuning log payloads and delivery in RHOCP 4](https://access.redhat.com/solutions/7074148)

[Performance and reliability tuning](https://docs.openshift.com/container-platform/4.14/observability/logging/performance_reliability/logging-flow-control-mechanisms.html)

[How to migrate Fluentd to Vector in Red Hat OpenShift Logging 5.5+ versions ?](https://access.redhat.com/articles/6999658)

[Migrating the log collector from fluentd to vector reducing the number of logs duplicated in RHOCP 4](https://access.redhat.com/articles/7063405)

  
- [Configuring the LokiStack log store](https://docs.openshift.com/container-platform/4.14/observability/logging/log_storage/cluster-logging-loki.html)

[Migrating the default log store from Elasticsearch to Loki in OCP 4](https://access.redhat.com/articles/6991632)


- [Configuring log forwarding](https://docs.openshift.com/container-platform/4.14/observability/logging/log_collection_forwarding/configuring-log-forwarding.html)




### 3)Common customer issues
- The customer cannot see the logs in the OCP Console.
- Logs are delayed in the OCP Console.
- [Logging alerts](https://docs.openshift.com/container-platform/4.14/observability/logging/logging_alerts/default-logging-alerts.html).
- JSON logs with Loki.
- Logs are not sent to the third-party system.
- Some logs are not sent to the third-party system.
- Cluster Log Forwarder configuration.

### 4)Troubleshooting

#### 4.1) How to narrow down the problem?

- Is the issue only happening for a concrete user?
- Is the issue only happening for a concrete application?
- Is the issue only happening for the pods/applications allocated in a concrete node?
- Logging must-gather
- Log console screenshot

#### 4.2) Must-gather paths
```
Operator version: /must-gather.local/registry-redhat-io-openshift-logging-cluster-logging-rhel8-operator-sha256-ad436419a03ffd76260906448cbcea146357dd102b89e791fc84be75ea30bb0f/cluster-logging/install
ClusterLogging instance (from 5.7): /must-gather.local/registry-redhat-io-openshift-logging-cluster-logging-rhel9-operator-sha256-463bdc2944690d56eb597314a7045fdb751c56e28bc6d45b279f194d4695be0f/namespaces/openshift-logging/logging.openshift.io/clusterloggings
ClusterLogging instance (prior 5.7): /must-gather.local/registry-redhat-io-openshift-logging-cluster-logging-rhel8-operator-sha256-ad436419a03ffd76260906448cbcea146357dd102b89e791fc84be75ea30bb0f/cluster-logging/clo
ClusterLogForwarder instance (from 5.7): /must-gather.local/registry-redhat-io-openshift-logging-cluster-logging-rhel9-operator-sha256-463bdc2944690d56eb597314a7045fdb751c56e28bc6d45b279f194d4695be0f/namespaces/openshift-logging/logging.openshift.io/clusterlogforwarders
ClusterLogForwarder instance (prior 5.7): /must-gather.local/registry-redhat-io-openshift-logging-cluster-logging-rhel8-operator-sha256-ad436419a03ffd76260906448cbcea146357dd102b89e791fc84be75ea30bb0f/cluster-logging/clo
Elasticsearch: /must-gather.local/registry-redhat-io-openshift-logging-cluster-logging-rhel9-operator-sha256-463bdc2944690d56eb597314a7045fdb751c56e28bc6d45b279f194d4695be0f/cluster-logging/es/cluster-elasticsearch
Collector Buffer: /must-gather.local/registry-redhat-io-openshift-logging-cluster-logging-rhel8-operator-sha256-ad436419a03ffd76260906448cbcea146357dd102b89e791fc84be75ea30bb0f/cluster-logging/collector
```


#### 4.3) If the customer cannot collect a must-gather
```
$ oc adm inspect ns/openshift-logging (also add the generated file)
$ oc -n openshift-logging get clusterlogging instance -o yaml > clo.txt
$ oc -n openshift-logging get clusterLogForwarder instance -o yaml > clf.txt
$ oc -n openshift-logging get csv > csv.txt
$ oc -n openshift-logging get LokiStack <lokistack_instance> -o yaml > lokistack.txt
$ oc -n openshift-logging get secret <loki storage secret> > lokisecret.yaml

===Metrics to identify log drops===
/// Vector discarded events
sum by(component_name) (irate(vector_buffer_discarded_events_total{component_kind="sink",component_type="loki"}[2m]))
/// Loki distributor lines received
sum by (tenant) (rate(loki_distributor_lines_received_total[5m]))
/// Loki gateway response rate
sum by (tenant, code) (rate(http_requests_total{namespace="openshift-logging",container="gateway",handler="push"}[5m]))
/// Loki discarded samples
sum by (tenant, reason) (irate(loki_discarded_samples_total[2m]))
sum by (tenant,reason)(sum_over_time(loki_discarded_samples_total{namespace="openshift-logging"}[1m]))

```

#### 4.4)Common checks

- Logging Operator Version and Loki Operator version
- ClusterLogging Managed status
- ClusterLogging instance
- ClusterLogForwarder instance
- LokiStack instance
- Collector Logs
- Loki components logs

#### 4.5) Common Loki issues
Ingestion burst
Loki console timeout
Vector OOM
HashRing


#### 4.6) Vector Troubleshooting
- [New Commers Check List](https://docs.google.com/spreadsheets/d/1M5DT-GiFVJt9PYAjPnMQBRCvm6tKu4KurtkRzEg0LLA/edit?gid=1234451132#gid=1234451132) Understanding and troubleshooting fluentd part 1 and 2.

- Examples:
[Metadata error sending logs to Kafka in RHOCP 4](https://access.redhat.com/solutions/6992317), [Forwarding logs using syslog fails with error EMSGSIZE in OpenShift 4](https://access.redhat.com/solutions/5873961)


### 5) Support cases

- Internal Elasticsearch
[03978669](https://gss--c.vf.force.com/apex/Case_View?srPos=0&srKp=500&id=500Hn00001kqiaw&sfdc.override=1), [03887839](https://gss--c.vf.force.com/apex/Case_View?srPos=0&srKp=500&id=5006R000020ZGdc&sfdc.override=1), [03692480](https://gss--c.vf.force.com/apex/Case_View?srPos=0&srKp=500&id=5006R00001y6x7K&sfdc.override=1), [03896587](https://gss--c.vf.force.com/apex/Case_View?srPos=0&srKp=500&id=5006R000020b0ML&sfdc.override=1)
- Forwarding logs
[03946195](https://gss--c.vf.force.com/apex/Case_View?srPos=0&srKp=500&id=5006R0000211QzP&sfdc.override=1), [03877761](https://gss--c.vf.force.com/apex/Case_View?srPos=0&srKp=500&id=5006R000020P21f&sfdc.override=1)
- JSON
[03267654](https://gss--c.vf.force.com/apex/Case_View?srPos=0&srKp=500&id=5006R00001mtphr&sfdc.override=1), [03582425](https://gss--c.vf.force.com/apex/Case_View?srPos=0&srKp=500&id=5006R00001vdVZq&sfdc.override=1), [03444961](https://gss--c.vf.force.com/apex/Case_View?srPos=0&srKp=500&id=5006R00001rZf4q&sfdc.override=1) 



- Loki
[03866052](https://gss--c.vf.force.com/apex/Case_View?srPos=0&srKp=500&id=5006R000020MyFz&sfdc.override=1)
[03775124](https://gss--c.vf.force.com/apex/Case_View?srPos=0&srKp=500&id=5006R00001zBYCs&sfdc.override=1)
[03980750](https://gss--c.vf.force.com/apex/Case_View?srPos=0&srKp=500&id=500Hn00001kqp6A&sfdc.override=1)
[03887752](https://gss--c.vf.force.com/apex/Case_View?srPos=0&srKp=500&id=5006R000020ZFxl&sfdc.override=1)
[03861975](https://gss--c.vf.force.com/apex/Case_View?srPos=0&srKp=500&id=5006R000020MEMM&sfdc.override=1)
[03873885](https://gss--c.vf.force.com/apex/Case_View?srPos=0&srKp=500&id=5006R000020OJ1U&sfdc.override=1)
[03927622](https://gss--c.vf.force.com/apex/Case_View?srPos=0&srKp=500&id=5006R000020xw9L&sfdc.override=1)
[03944455](https://gss--c.vf.force.com/apex/Case_View?sbstr=03944455)


