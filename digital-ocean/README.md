droplet.yaml config

```
apiVersion: compute.do.crossplane.io/v1alpha1
kind: Droplet
metadata:
  annotations:
    crossplane.io/external-name: crossplane-droplet
  name: talks-to-code-demo
spec:
  forProvider:
    region: nyc1
    size: s-1vcpu-512mb-10gb
    image: ubuntu-22-04-x64
  providerConfigRef:
    name: default
```


resized droplet

```
apiVersion: compute.do.crossplane.io/v1alpha1
kind: Droplet
metadata:
  annotations:
    crossplane.io/external-name: crossplane-droplet
  name: talks-to-code-demo
spec:
  forProvider:
    region: nyc1
    size: s-1vcpu-1gb
    image: ubuntu-22-04-x64
  providerConfigRef:
    name: default
```
