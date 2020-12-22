# openshift-observatorium

Lab settings:

- OCP 4.5
- OCS 4.5

## Repos

Clone these repos first:

    git clone https://github.com/observatorium/deployments
    git clone https://github.com/observatorium/operator

## Deploy Observatorium (with operator)

### Repos

    
    
### Deploy dex

    oc new-project dex
    oc apply -f tests/manifests/observatorium-xyz-tls-dex.yaml
    oc apply -f environments/dev/manifests/dex-secret.yaml
    oc apply -f environments/dev/manifests/dex-pvc.yaml
    oc apply -f environments/dev/manifests/dex-deployment.yaml
    oc apply -f environments/dev/manifests/dex-service.yaml
    
    # Observatorium needs the Dex API to be ready for authentication to work and thus for the tests to pass.
    oc wait --for=condition=available --timeout=10m -n dex deploy/dex || (must_gather "$ARTIFACT_DIR" && exit 1)
