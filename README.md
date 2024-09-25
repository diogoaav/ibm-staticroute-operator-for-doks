# IBM static-route-operator for DOKS
Modified IBM static IP route operator for Kubernetes clusters on DigitalOcean

This project is under development, use it on your own risk please.

# Usage

This is a modified version of the IBM static route operator found here: https://github.com/IBM/staticroute-operator

This version has a few tweaks to make it work with DOKS. The pod image is already pre built.

## Create CRD / Namespace / ServiceAccount / ClusterRole / ClusterRoleBinding / DaemonSet

1- Deploy the latest release of static routes operator to your DOKS cluster using kubectl:

```
kubectl apply -f https://raw.githubusercontent.com/diogoaav/ibm-staticroute-operator-for-doks/main/releases/v1/k8s-staticroute-operator-v1.0.0.yaml
``` 

## Sample custom resources

Route a subnet across the default gateway.
```
apiVersion: static-route.ibm.com/v1
kind: StaticRoute
metadata:
  name: example-static-route
spec:
  subnet: "192.168.0.0/24"
```

Route a subnet to the custom gateway.
```
apiVersion: static-route.ibm.com/v1
kind: StaticRoute
metadata:
  name: example-static-route
spec:
  subnet: "192.168.0.0/24"
  gateway: "10.0.0.1"
```

Selecting target node(s) of the static route by label(s):
```
apiVersion: static-route.ibm.com/v1
kind: StaticRoute
metadata:
  name: example-static-route-with-selector
spec:
  subnet: "192.168.1.0/24"
  selectors:
    -
      key: "kubernetes.io/arch"
      operator: In
      values:
        - "amd64"
```