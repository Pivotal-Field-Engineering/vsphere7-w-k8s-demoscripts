# Install fluent bit

## Set environment variables

The scripts to prepare the YAML to deploy fluent-bit depend on a few environmental variables to be set.  Set the following variables in you terminal session:

```bash
# the DNS CN to be used for dex service
export ELASTICSEARCH_CN=elasticsearch.mgmt.tkg-aws-lab.winterfell.live
```

## Prepare Manifests

Prepare the YAML manifests for the related fluent-bit K8S objects.  Manifest will be output into `clusters/wlc-1/tkg-extensions-mods/logging/fluent-bit/outputs/elasticsearch/generated` in case you want to inspect.

```bash
./scripts/generate-fluent-bit-yaml-wlc.sh
```

## Deploy fluent-bit

```bash
kubectl apply -f tkg-extensions/logging/fluent-bit/vsphere/00-fluent-bit-namespace.yaml
kubectl apply -f tkg-extensions/logging/fluent-bit/vsphere/01-fluent-bit-service-account.yaml
kubectl apply -f tkg-extensions/logging/fluent-bit/vsphere/02-fluent-bit-role.yaml
kubectl apply -f tkg-extensions/logging/fluent-bit/vsphere/03-fluent-bit-role-binding.yaml
# Change following parameters in 04-fluent-bit-configmap.yaml

<TKG_CLUSTER_NAME> to the name of the tkg cluster.

<TKG_INSTANCE_NAME> to the name of tkg instance. This name should be the same for management cluster and all the workload clusters that form a part of one TKG deployment.

<FLUENT_ELASTICSEARCH_HOST> to service name of Elasticsearch within your cluster.

<FLUENT_ELASTICSEARCH_PORT> to the appropriate port Elastic Search server is listening to.

kubectl apply -f logging/fluent-bit/vsphere/output/elasticsearch/04-fluent-bit-configmap.yaml
kubectl apply -f tkg-extensions/logging/fluent-bit/vsphere/output/elasticsearch/05-fluent-bit-ds.yaml
```

## Validation Step

Ensure that fluent bit pods are running

```bash
kubectl get pods -n tanzu-system-logging
```
