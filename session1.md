## Session 1. 


### 1)Architecture

-[Limitation of the OpenShift Logging Architecture](https://access.redhat.com/solutions/3329761)

-[New Commers Check List](https://docs.google.com/spreadsheets/d/1M5DT-GiFVJt9PYAjPnMQBRCvm6tKu4KurtkRzEg0LLA/edit?gid=1234451132#gid=1234451132) OpenShift 4.x Troubleshooting ClusterLogging

### 2)[Installation](https://docs.openshift.com/container-platform/4.14/observability/logging/cluster-logging-deploying.html)

### 3)Common customer issues
- The customer cannot see the logs in Kibana.
- Logs are delayed in Kibana.
- Collector alerts.
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
- Kibana Logs

#### 4.5) Forwarding troubleshooting
- [New Commers Check List](https://docs.google.com/spreadsheets/d/1M5DT-GiFVJt9PYAjPnMQBRCvm6tKu4KurtkRzEg0LLA/edit?gid=1234451132#gid=1234451132) Understanding and troubleshooting fluentd part 1 and 2.
- [Metadata error sending logs to Kafka in RHOCP 4](https://access.redhat.com/solutions/6992317)
- [Forwarding logs using syslog fails with error EMSGSIZE in OpenShift 4](https://access.redhat.com/solutions/5873961)


**Common causes**



### 5) Examples
03978669 
03692480
03946195 SEV1 external third-party
03267654 JSON 
03444961 JSON
03582425 JSON


03887839
03896587 Crazy resources
External Splunk third party 03877761






Loki 03866052
Loki 03775124
Loki 03980750
Loki 03899254
Loki 03887752

Loki 03979519
03861975 Vector
Loki 03873885
