# openshift-observatorium

Lab settings:

- OCP 4.5
- OCS 4.5

## Repos

Additional reference repos:
    https://github.com/observatorium/deployments
    https://github.com/observatorium/operator

## Deploy Observatorium (with operator)

### Deploy dex (Skip this section)

    oc new-project dex
    
    oc apply -f manifests/dex/observatorium-xyz-tls-dex.yaml
    oc apply -f manifests/dex/dex-secret.yaml
    oc apply -f manifests/dex/dex-pvc.yaml
    oc apply -f manifests/dex/dex-deployment.yaml
    oc apply -f manifests/dex/dex-service.yaml

### Create Observatorium Namespace
    oc new-project observatorium


### Create Bucket

  - Create an OBC in OpenShift called observatorium-thanos
  - Create a Secret using the manifest thanos-objectstorage.yaml with your real bucket's values (bucket-name, access-key and secret-key)
       oc apply -f manifests/thanos-objectstorage.yaml
       
### Deploy Observatorium
    
    # service CA for the first tenant, "test"
    oc apply -f manifests/test-ca-tls.yaml

    # Observatorium needs the Dex API to be ready for authentication to work and thus for the tests to pass.
    oc wait --for=condition=available --timeout=10m -n dex deploy/dex || (must_gather "$ARTIFACT_DIR" && exit 1)
