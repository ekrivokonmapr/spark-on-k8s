# Helm Chart for MEP Spark History Server

[Spark History Server](https://spark.apache.org/docs/latest/monitoring.html#viewing-after-the-fact) provides a web UI for completed and running Spark applications. The supported storage backends are MaprFS and PersistentVolumeClaim (PVC)

## Installing the Chart

Note that this chart requires ECP tenant operator to be installed and Tenant CR applied in the tenant namespace.

### Install command
`helm install spark-hs ./spark-hs-chart `

This will create the helm chart in the `default` namespace. To create the chart in a different existing namespace use the flag
` -n sampletenant `.
Please note that if you are using PVC, the pvc should exist in the same namespace.

To set the tenant namespace use the flag `--set tenantNameSpace=sampletenant` during installation

### Viewing the UI
After the chart is successfully installed, a message would be printed out to the console with details about how to access the UI.

## Examples

Install spark history server deployment named 'spark-test-hs' in 'sampletenant' tenant, assuming that tenant type is 'internal' or 'external':
```shell script
helm install -f ./spark-hs-chart/values.yaml spark-hs-sampletenant ./spark-hs-chart/ \
--namespace sampletenant \
--set tenantNameSpace=sampletenant \
--set tenantIsUnsecure=false \
--set eventlogstorage.kind=maprfs
```

Install spark history server deployment named 'spark-test-hs' in 'sampletenant' tenant, assuming that tenant type is 'none'.
Put attention that 'maprfs' eventlog storage kind is not applicable for 'none' tenants. Instead, 'pvc' should be used.
This example assumes that 'spark-hs-pvc' PVC has been created or will be created later. Spark-HS server pod will not start
if 'spark-hs-pvc' doesn't exist. The PVC should have access mode 'ReadWriteMany'.
```shell script
helm install -f ./spark-hs-chart/values.yaml spark-hs-sampletenant ./spark-hs-chart/ \
--namespace sampletenant \
--set tenantNameSpace=sampletenant \
--set tenantIsUnsecure=true \
--set eventlogstorage.kind=pvc \
--set eventlogstorage.pvcname=spark-hs-pvc
```

Uninstall spark history server deployment named "spark-test-hs" from 'sampletenant' tenant
```shell script
helm uninstall spark-test-hs --namespace sampletenant
```
Please note that this won't delete the PVC in case you are using a PVC. PVC will have to be manually deleted.
