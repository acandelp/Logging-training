## Vector+Elasticsearch (deprecated)

##### - Install [Elasticsearch Operator](https://docs.openshift.com/container-platform/4.15/observability/logging/cluster-logging-deploying.html)
##### - Install [Logging Operator](https://docs.openshift.com/container-platform/4.15/observability/logging/cluster-logging-deploying.html)
##### - Deploy the ClusterLogging instance 
```
apiVersion: "logging.openshift.io/v1"
kind: "ClusterLogging"
metadata:
  name: "instance"
  namespace: "openshift-logging"
spec:
  managementState: "Managed"
  logStore:
    type: "elasticsearch"
    elasticsearch:
      nodeCount: 1
      resources:
        limits:
          cpu: 200m
          memory: 1Gi
        requests:
          cpu: 200m
          memory: 1Gi
      storage: {}
      redundancyPolicy: "ZeroRedundancy"
  visualization:
    type: "kibana"
    kibana:
      resources:
        limits:
          cpu: 500m
          memory: 512Mi
        requests:
          cpu: 500m
          memory: 512Mi
      replicas: 1
  collection:
    type: "vector"
```
##### - Deploy the ClusterLogForwarder instance (optional)
```
kind: ClusterLogForwarder
metadata:
  name: instance 
  namespace: openshift-logging 
spec:
  pipelines:
   - name: elasticsearch-application 
     inputRefs:
      - application
     outputRefs:
      - default
     parse: json
```
