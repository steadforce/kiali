# Kiali-Operator and Kiali

Repository containing manifests for
[kiali-operator plus kiali](https://kiali.io/docs/installation/installation-guide/install-with-helm/)
installation. Never install the content of this repo on our clusters manually. This is all done by argo-cd.

## Dependencies

This chart pulls in `kiali-operator` as a dependency. The version
used is specified in `Chart.yaml` in the `dependencies` section.
If you change the version in there, you need to then run

    $ helm dependency update

in order to have the chart downloaded to the `charts` directory
and then also commit that new version alongside with the altered
`Chart.yaml` file.

See the [Helm docs](https://helm.sh/docs/topics/charts/#chart-dependencies)
for details.


## Render helm charts locally

The following command renders the charts like argo-cd does to validate the content.

### local

```
 helm template --release-name kiali -n kiali-operator --include-crds --skip-tests \
  -a autoscaling.k8s.io/v1 \
  -a cert-manager.io/v1 \
  -a forecastle.stakater.com/v1alpha1 \
  -a keycloak.org/v1alpha1 \
  -a kiali.io/v1alpha1 \
  -a monitoring.coreos.com/v1 \
  -a networking.istio.io/v1beta1 \
  -a security.istio.io/v1beta1 \
  -f values-local.yaml \
  --output-dir _local . 
```

### dev

```
 helm template --release-name kiali -n kiali-operator --include-crds --skip-tests \
  -a autoscaling.k8s.io/v1 \
  -a cert-manager.io/v1 \
  -a forecastle.stakater.com/v1alpha1 \
  -a keycloak.org/v1alpha1 \
  -a kiali.io/v1alpha1 \
  -a monitoring.coreos.com/v1 \
  -a networking.istio.io/v1beta1 \
  -a security.istio.io/v1beta1 \
  -f values-development.yaml \
  --output-dir _dev . 
```

### prod

```
 helm template --release-name kiali -n kiali-operator --include-crds --skip-tests \
  -a autoscaling.k8s.io/v1 \
  -a cert-manager.io/v1 \
  -a forecastle.stakater.com/v1alpha1 \
  -a keycloak.org/v1alpha1 \
  -a kiali.io/v1alpha1 \
  -a monitoring.coreos.com/v1 \
  -a networking.istio.io/v1beta1 \
  -a security.istio.io/v1beta1 \
  -f values-production.yaml \
  --output-dir _prod . 
```

You can use this command to check if the output is as you expect. The `-a` parameters are needed since we use the
helm feature `.Capabilities.APIVersions.Has` to determine if a `CR` is installable in the cluster or not. Since
helm templating is designed to work offline we have to list the supported `CR`. Using `.Capabilities.APIVersions.Has`
feature in templating prevents sync errors in argo-cd if a `CR` can't be applied since its `CRD` isn't ready.
