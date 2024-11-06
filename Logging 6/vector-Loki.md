## Vector+Loki 
##### - Install [Cluster Observability Operator](https://docs.openshift.com/container-platform/4.15/observability/cluster_observability_operator/installing-the-cluster-observability-operator.html)
##### - Install [Loki Operator Version 6](https://docs.openshift.com/container-platform/4.15/observability/logging/log_storage/installing-log-storage.html#logging-loki-gui-install_installing-log-storage)
##### - Install [Logging Operator Version 6](https://docs.openshift.com/container-platform/4.15/observability/logging/log_storage/installing-log-storage.html#logging-loki-gui-install_installing-log-storage)
##### - Available Object Storage
##### - Define a secret for Loki Object Storage
```
apiVersion: v1
kind: Secret
metadata:
  name: logging-loki-s3
  namespace: openshift-logging
stringData:
  access_key_id: <>
  access_key_secret: <>
  bucketnames: lokistoragebucket
  endpoint: https://s3.eu-west-3.amazonaws.com
  region: eu-west-3
```
##### - Deploy the LokiStack instance
```
apiVersion: loki.grafana.com/v1
kind: LokiStack
metadata:
  name: logging-loki
  namespace: openshift-logging
spec:
  size: 1x.demo
  storage:
    schemas:
    - version: v13
      effectiveDate: '2022-06-01'
    secret:
      name: logging-loki-s3
      type: s3
  storageClassName: gp3-csi
  tenants:
    mode: openshift-logging
```
##### - Create a service account for the collector:
```
$ oc create sa collector -n openshift-logging
```
##### - Create the Cluster Role Binding for the Service Account:
```
$ oc create clusterrolebinding collect-application-logs --clusterrole=collect-application-logs --serviceaccount openshift-logging:collector
$ oc create clusterrolebinding collect-infrastructure-logs --clusterrole=collect-infrastructure-logs --serviceaccount openshift-logging:collector
$ oc create clusterrolebinding collect-audit-logs --clusterrole=collect-audit-logs --serviceaccount openshift-logging:collector
```
##### - Bind the writer Cluster Role to the Service Account:
```
$ oc adm policy add-cluster-role-to-user logging-collector-logs-writer -z collector
```
##### - Deploy the UIPlugin to enable the Log section in the Observe tab:
```
apiVersion: observability.openshift.io/v1alpha1
kind: UIPlugin
metadata:
  name: logging
spec:
  type: Logging
  logging:
    lokiStack:
      name: logging-loki
```
##### - Deploy the ClusterLogForwarder observability Custom Resource:
```
apiVersion: observability.openshift.io/v1
kind: ClusterLogForwarder
metadata:
  name: instance
  namespace: openshift-logging
spec:
  serviceAccount:
    name: collector
  outputs:
  - name: default-lokistack
    type: lokiStack
    lokiStack:
      target:
        name: logging-loki
        namespace: openshift-logging
      authentication:
        token:
          from: serviceAccount
    tls:
      ca:
        key: service-ca.crt
        configMapName: openshift-service-ca.crt
  pipelines:
  - name: default-logstore
    inputRefs:
    - application
    - infrastructure
    outputRefs:
    - default-lokistack
```
