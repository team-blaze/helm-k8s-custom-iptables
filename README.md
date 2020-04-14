# k8s-custom-iptables

An example of how to add custom IP tables rules to a Kubernetes cluster.
This collection of scripts creates a NAT (MASQ) rule for outbound traffic
to a TARGETS CIDR range(s) given to the script.

## Installing rules into the cluster

Install the daemonset that configures the cluster to NAT an IP range.

```sh
TARGETS="1.2.3.4/24,4.5.6.7/16" ./install.sh
```

## Uninstall rules from the cluster

Uninstall the IP tables rules from the cluster.

```sh
./uninstall.sh
```

## Configuring

The configuration for which ranges are NAT'd are in the `k8s-custom-iptables` ConfigMap.
Values can be changed via `kubectl edit cm/k8s-custom-iptables`:

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: k8s-custom-iptables
data:
  nat.rules: "10.0.0.0/24 192.168.0.0/16"
```

## Creating and pushing the image

First you need to authenticate Docker with your image registry.

For example with Google Container Registry you could do `gcloud auth configure-docker`.

For more info see: https://cloud.google.com/container-registry/docs/advanced-authentication

Then you can use the Make scripts:

```sh
REGISTRY=gcr.io/my-registry make
```
