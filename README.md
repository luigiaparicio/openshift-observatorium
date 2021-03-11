# openshift-observatorium

Lab settings:

- OCP 4.5
- OCS 4.5

## Repos

Additional reference repos:
    https://github.com/observatorium/deployments
    https://github.com/observatorium/operator

## Deploy Observatorium (with operator)

### Deploy dex

    oc new-project dex
    oc apply -f manifests/observatorium-xyz-tls-dex.yaml
    oc apply -f manifests/dex-secret.yaml
    oc apply -f manifests/dex-pvc.yaml
    oc apply -f manifests/dex-deployment.yaml
    oc apply -f manifests/dex-service.yaml
    
    # service CA for the first tenant, "test"
    oc apply -f jsonnet/vendor/github.com/observatorium/deployments/tests/manifests/test-ca-tls.yaml

    # Observatorium needs the Dex API to be ready for authentication to work and thus for the tests to pass.
    oc wait --for=condition=available --timeout=10m -n dex deploy/dex || (must_gather "$ARTIFACT_DIR" && exit 1)
