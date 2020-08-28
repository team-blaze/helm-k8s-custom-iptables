# k8s-custom-iptables Helm chart

A Helm chart to help with adding custom IP tables rules to nodes in a Kubernetes cluster.
This collection of scripts and templates create a NAT (MASQ) rule for outbound traffic to a TARGETS
CIDR range(s) given to the script.

This is only necessary on Google Container Engine (GKE) if your cluster isn't a VPC-native cluster
using alias IP address ranges. For more information see: https://cloud.google.com/kubernetes-engine/docs/how-to/alias-ips

Helm chart based on https://v3.helm.sh/docs/topics/chart_repository/#github-pages-example
and https://github.com/technosophos/tscharts.

Kubernetes templates and scripts based on https://github.com/bowei/k8s-custom-iptables

## Configuration

The following table lists the configurable parameters of the `k8s-custom-iptables` chart and their
default values, and can be overwritten via the helm `--set` flag.

| Parameter          | Description                     | Default                          |
| ------------------ | ------------------------------- | -------------------------------- |
| `nat_ip_ranges`    | CIDR IP ranges, space separated | `10.0.0.0/24 192.168.0.0/16`     |
| `image`            | Docker image and tag to use     | `daaain/k8s-custom-iptables:1.0` |
| `imagePullSecrets` | Docker registry secret          | unset                            |

You'll definitely need to update the `nat_ip_ranges` to match the ones in your cloud VPC. In my case
that was finding out the IP of the database – say `10.146.11.3` – and then defining a reasonable
CIDR range which would include it – say `10.146.11.0/24`.

I've pushed the image built off the `Dockerfile` in this repo into a public repo under my personal
Docker Hub account which will work, but you should push into and use your private cloud registry. If
you do so, you'll need to set `imagePullSecrets` for Kubernetes to be authenticated to pull the
image when deploying, see: https://kubernetes.io/docs/tasks/configure-pod-container/pull-image-private-registry/

## TODO

- [ ] Add a way to clear IP tables when removing chart based on https://github.com/bowei/k8s-custom-iptables/blob/master/uninstall.sh
