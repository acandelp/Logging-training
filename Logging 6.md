## Session 2. 


### 1)Architecture

- [Introduction to LokiStack](https://videos.learning.redhat.com/playlist/dedicated/251079123/1_ojvcvz0p/1_24vvknfn)
- [Upstream documentation](https://grafana.com/docs/loki/latest/get-started/architecture/)
- [Introduction to LokiStack Operations/Dashboard](https://videos.learning.redhat.com/playlist/dedicated/251079123/1_ojvcvz0p/1_zq29kjud)


### 2)[Installation](https://docs.openshift.com/container-platform/4.16/observability/logging/log_storage/installing-log-storage.html#logging-loki-gui-install_installing-log-storage)

#### 2.1) Configuration

- [Configuring the logging Collector](https://docs.openshift.com/container-platform/4.14/observability/logging/log_collection_forwarding/cluster-logging-collector.html)
  
- [Configuring the LokiStack log store](https://docs.openshift.com/container-platform/4.14/observability/logging/log_storage/cluster-logging-loki.html)
  
- [Configuring log forwarding](https://docs.openshift.com/container-platform/4.14/observability/logging/log_collection_forwarding/configuring-log-forwarding.html)

- [Step by Step installation Video](https://drive.google.com/file/d/1G75BKcR-_35WcSt7lra_TYYBUzBfxYUm/view)

#### 2.2) Collector and logStorage migration

- [How to migrate Fluentd to Vector in Red Hat OpenShift Logging 5.5+ versions ?](https://access.redhat.com/articles/6999658)

- [Migrating the log collector from fluentd to vector reducing the number of logs duplicated in RHOCP 4](https://access.redhat.com/articles/7063405)

- [Migrating the default log store from Elasticsearch to Loki in OCP 4](https://access.redhat.com/articles/6991632)

#### 2.2) Useful configuration information

- [Tuning log payloads and delivery in RHOCP 4](https://access.redhat.com/solutions/7074148)

- [Performance and reliability tuning](https://docs.openshift.com/container-platform/4.14/observability/logging/performance_reliability/logging-flow-control-mechanisms.html)

- [JSON logs with Loki](https://access.redhat.com/solutions/7048604).
  
- [Why Loki needs block and object storage in RHOCP 4?](https://access.redhat.com/solutions/7062821).

- [LokistackSchemaUpgradesRequired warning alert firing in RHOCP 4](https://access.redhat.com/solutions/7063482).
  




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
LokiStack instance: /must-gather.local/registry-redhat-io-openshift-logging-cluster-logging-rhel9-operator-sha256-695733369faba7b24b5ebecdbb23be5629965072d970f41e7560ad7e7bd20765/cluster-logging/lokistack
Vector config file: /must-gather.local/registry-redhat-io-openshift-logging-cluster-logging-rhel9-operator-sha256-695733369faba7b24b5ebecdbb23be5629965072d970f41e7560ad7e7bd20765/cluster-logging/clo/openshift-logging/collector-config_vector.toml
```
