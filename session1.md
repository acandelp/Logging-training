## Session 1. 
**Architecture** also cronjobs

**Installation**
##### - Common issues
- Logs are not in Kibana
- Logs are delayed in Kibana
- Collector alerts
- Cronjobs are not working properly
- Elasticsearch is in a Yellow/Red status

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


Loki 03866052
Loki 03775124
