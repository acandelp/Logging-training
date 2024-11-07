## Session 1. 
**Architecture** also cronjobs

https://access.redhat.com/solutions/3329761

**Installation**
**Common issues**
- The customer cannot see the logs in Kibana.
- Logs are delayed in Kibana.
- Collector alerts.
- Cronjobs are not working properly.
- Elasticsearch is in a Yellow/Red status.
- Elasticsearch PVC is full.

**How to narrow down the problem?**

- Is the issue only happening for a concrete user?
- Is the issue only happening for a concrete application?
- Is the issue only happening for the pods/applications allocated in a concrete node?
- Logging must-gather
- Kibana screenshot

Must gather paths and what I see
ClusterLogging instance (from 5.7): /03943294/must-gather.local.4358234533440916194/registry-redhat-io-openshift-logging-cluster-logging-rhel9-operator-sha256-463bdc2944690d56eb597314a7045fdb751c56e28bc6d45b279f194d4695be0f/namespaces/openshift-logging/logging.openshift.io/clusterloggings

ClusterLogForwarder instance (from 5.7): /03943294/must-gather.local.4358234533440916194/registry-redhat-io-openshift-logging-cluster-logging-rhel9-operator-sha256-463bdc2944690d56eb597314a7045fdb751c56e28bc6d45b279f194d4695be0f/namespaces/openshift-logging/logging.openshift.io/clusterlogforwarders

Elasticsearch: /03943294/must-gather.local.4358234533440916194/registry-redhat-io-openshift-logging-cluster-logging-rhel9-operator-sha256-463bdc2944690d56eb597314a7045fdb751c56e28bc6d45b279f194d4695be0f/cluster-logging/es/cluster-elasticsearch
Important files: indices_size.cat, health.cat, nodes.cat

If the customer cannot collect a must-gather-------
```
$ oc adm inspect ns/openshift-logging (also add the generated file)
$ oc -n openshift-logging get clusterlogging instance -o yaml > clo.txt
$ oc -n openshift-logging get clusterLogForwarder instance -o yaml > clf.txt
$ oc -n openshift-logging get csv > csv.txt
$ $ for pod in `oc get po -l component=elasticsearch -o jsonpath='{.items[*].metadata.name}'`; do echo $pod; oc exec -c elasticsearch $pod -- df -h /elasticsearch/persistent; done
$ oc rsh -c elasticsearch <elasticsearchpod>
# es_util --query=_cat/health?v
# es_util --query=_cat/nodes?v
# es_util --query="_cat/indices?h=health,status,index,id,pri,rep,docs.count,docs.deleted,store.size,creation.date.string&v="
```

##### - Common checks

Logging Operator Version and Elasticsearch Operator version.
ClusterLogging Managed status.
ClusterLogging instance.
ClusterLogForwarder instance.
Collector Logs.
Elasticsearch Logs.
Elasticsearch status.
Kibana Logs.



**Common causes**

##### - Configuration issues
- The logs are not in a JSON format.
- Multiline feature is not working.
- Elasticsearch index is not defined properly.

  

03692480
03608590
03444961
03582425
03267654
03978669


Loki 03866052
Loki 03775124