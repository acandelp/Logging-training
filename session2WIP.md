## Session 2. 


### 1)Architecture

- [New Commers Check List](https://docs.google.com/spreadsheets/d/1M5DT-GiFVJt9PYAjPnMQBRCvm6tKu4KurtkRzEg0LLA/edit?gid=1234451132#gid=1234451132) Introduction to LokiStack
- [Upstream documentation](https://grafana.com/docs/loki/latest/get-started/architecture/)


### 2)[Installation](https://docs.openshift.com/container-platform/4.16/observability/logging/log_storage/installing-log-storage.html#logging-loki-gui-install_installing-log-storage)

#### 2.1) Configuration

- [Configuring the logging Collector](https://docs.openshift.com/container-platform/4.14/observability/logging/log_collection_forwarding/cluster-logging-collector.html)
  
- [Configuring the LokiStack log store](https://docs.openshift.com/container-platform/4.14/observability/logging/log_storage/cluster-logging-loki.html)
  
- [Configuring log forwarding](https://docs.openshift.com/container-platform/4.14/observability/logging/log_collection_forwarding/configuring-log-forwarding.html)

- [Installation steps
- Session installation

#### 2.2) Collector and logStorage migration

- [How to migrate Fluentd to Vector in Red Hat OpenShift Logging 5.5+ versions ?](https://access.redhat.com/articles/6999658)

- [Migrating the log collector from fluentd to vector reducing the number of logs duplicated in RHOCP 4](https://access.redhat.com/articles/7063405)

- [Migrating the default log store from Elasticsearch to Loki in OCP 4](https://access.redhat.com/articles/6991632)

#### 2.2) Useful configuration information

- [Tuning log payloads and delivery in RHOCP 4](https://access.redhat.com/solutions/7074148)

- [Performance and reliability tuning](https://docs.openshift.com/container-platform/4.14/observability/logging/performance_reliability/logging-flow-control-mechanisms.html)

- [JSON logs with Loki](https://access.redhat.com/solutions/7048604).



  










### 3)Common customer issues
- The customer cannot see the logs in the OCP Console.
- Logs are delayed in the OCP Console.
- [Logging alerts](https://docs.openshift.com/container-platform/4.14/observability/logging/logging_alerts/default-logging-alerts.html).
- [JSON logs with Loki](https://access.redhat.com/solutions/7048604).
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
Configuration issue


#### 4.6) Vector Troubleshooting
- [Troubleshooting Vector Draft](https://docs.google.com/document/d/1IhQZLhQNcbA8lZ-DuO_3YWOwhOIpCXv0GA7BxF2ECeA/edit?tab=t.0#heading=h.s3ccwubfbig0) 



### 5) Support cases

- Loki
[03959863](https://gss--c.vf.force.com/apex/Case_View?id=5006R00002144DH&sfdc.override=1) Config issue first to comment
[03809176](https://gss--c.vf.force.com/apex/Case_View?srPos=0&srKp=500&id=5006R00001wgefc&sfdc.override=1)
[03737922](https://gss--c.vf.force.com/apex/Case_View?srPos=11&srKp=500&srF=1&id=5006R00001ywIZr&sfdc.override=1)
[03866052](https://gss--c.vf.force.com/apex/Case_View?srPos=0&srKp=500&id=5006R000020MyFz&sfdc.override=1)
[03775124](https://gss--c.vf.force.com/apex/Case_View?srPos=0&srKp=500&id=5006R00001zBYCs&sfdc.override=1)
[03980750](https://gss--c.vf.force.com/apex/Case_View?srPos=0&srKp=500&id=500Hn00001kqp6A&sfdc.override=1)
[03887752](https://gss--c.vf.force.com/apex/Case_View?srPos=0&srKp=500&id=5006R000020ZFxl&sfdc.override=1)
[03861975](https://gss--c.vf.force.com/apex/Case_View?srPos=0&srKp=500&id=5006R000020MEMM&sfdc.override=1)
[03873885](https://gss--c.vf.force.com/apex/Case_View?srPos=0&srKp=500&id=5006R000020OJ1U&sfdc.override=1)
[03927622](https://gss--c.vf.force.com/apex/Case_View?srPos=0&srKp=500&id=5006R000020xw9L&sfdc.override=1)
[03944455](https://gss--c.vf.force.com/apex/Case_View?sbstr=03944455)
[03871025](https://gss--c.vf.force.com/apex/Case_View?srPos=47&srKp=500&srF=1&id=5006R000020NpjY&sfdc.override=1)
[03725529](https://gss--c.vf.force.com/apex/Case_View?srPos=66&srKp=500&srF=1&id=5006R00001ylVaI&sfdc.override=1)
[03925134](https://gss--c.vf.force.com/apex/Case_View?id=5006R000020p125&sfdc.override=1)
[03953146](https://gss--c.vf.force.com/apex/Case_View?id=5006R0000212ohh&sfdc.override=1) config issue



