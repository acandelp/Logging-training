## Session 1. 


### 1)Architecture
- [New Commers Check List](https://docs.google.com/spreadsheets/d/1M5DT-GiFVJt9PYAjPnMQBRCvm6tKu4KurtkRzEg0LLA/edit?gid=1234451132#gid=1234451132) OpenShift 4.x Troubleshooting ClusterLogging

- [Limitation of the OpenShift Logging Architecture](https://access.redhat.com/solutions/3329761)

- [Support](https://docs.openshift.com/container-platform/4.14/observability/logging/cluster-logging-support.html)

### 2)[Installation](https://docs.openshift.com/container-platform/4.14/observability/logging/cluster-logging-deploying.html)

- [Configuring the logging Collector](https://docs.openshift.com/container-platform/4.14/observability/logging/log_collection_forwarding/cluster-logging-collector.html)
  
- [Configuring the Elasticsearch log store](https://docs.openshift.com/container-platform/4.14/observability/logging/log_storage/logging-config-es-store.html)

- [Configuring log forwarding](https://docs.openshift.com/container-platform/4.14/observability/logging/log_collection_forwarding/configuring-log-forwarding.html)

- [Log visualization with Kibana](https://docs.openshift.com/container-platform/4.14/observability/logging/log_visualization/logging-kibana.html)
  

### 3)Common customer issues
- The customer cannot see the logs in Kibana.
- Logs are delayed in Kibana.
- [Logging alerts](https://docs.openshift.com/container-platform/4.14/observability/logging/logging_alerts/default-logging-alerts.html).
- Cronjobs are not working properly.
- Elasticsearch is in a Yellow/Red status.
- Elasticsearch PVC is full.
- The logs are not in a JSON format.
- The multiline feature is not working.
- Elasticsearch index is not defined properly.
- Logs are not sent to the third-party system.
- Some logs are not sent to the third-party system.
- Cluster Log Forwarder configuration.

### 4)Troubleshooting


#### 4.1) How to narrow down the problem?

- Is the issue only happening for a concrete user?
- Is the issue only happening for a concrete application?
- Is the issue only happening for the pods/applications allocated in a concrete node?
- Logging must-gather
- Kibana screenshot

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
$ for pod in `oc get po -l component=elasticsearch -o jsonpath='{.items[*].metadata.name}'`; do echo $pod; oc exec -c elasticsearch $pod -- df -h /elasticsearch/persistent; done
$ for i in $(oc get pods -l component=collector | awk '/collector/ { print $1 }') ; do oc exec $i -- du -sh /var/lib/fluentd  ; done

$ oc rsh -c elasticsearch <elasticsearchpod>
# es_util --query=_cat/health?v
# es_util --query=_cat/nodes?v
# es_util --query="_cat/indices?h=health,status,index,id,pri,rep,docs.count,docs.deleted,store.size,creation.date.string&v="
```

#### 4.4)Common checks

- Logging Operator Version and Elasticsearch Operator version
- ClusterLogging Managed status
- ClusterLogging instance
- ClusterLogForwarder instance
- Elasticsearch status
- Collector Logs
- Elasticsearch Logs
- Buffer status
- Kibana Logs

#### 4.5) [Elasticsearch alerts troubleshooting](https://docs.openshift.com/container-platform/4.10/logging/troubleshooting/cluster-logging-troubleshooting-for-critical-alerts.html)

#### 4.6) Forwarding troubleshooting
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





