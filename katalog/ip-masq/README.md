# IP Masq

<!-- <KFD-DOCS> -->

IP masquerading is a form of network address translation (NAT) used to perform many-to-one IP address translations, which allows multiple clients to access a destination using a single source IP address. A cluster uses IP masquerading so that destinations outside of the cluster only receive packets from node IP addresses instead of Pod IP addresses.

This is useful in environments that expect to only receive packets from node IP addresses.

You can read more about this at the following link [https://cloud.google.com/kubernetes-engine/docs][kubeengine-docs]

## Image repository and tag

- `ip-masq-agent` image:
  - `k8s.gcr.io/networking/ip-masq-agent:v2.8.0`.
- `ip-masq-agent` repository:
  - [https://github.com/kubernetes-sigs/ip-masq-agent](https://github.com/kubernetes-sigs/ip-masq-agent).

## Requirements

- Tested with Kubernetes >= `1.18.X`.
- Tested with Kustomize = `v3.5.3`.

## Configuration

The `ip-masq` package is deployed with the following [default configuration](config.yml):

- `nonMasqueradeCIDRs`: as an empty list.
- `resyncInterval`: set to 60 seconds.
- `masqLinkLocal`: set to false.
- `masqLinkLocalIPv6`: set to false.

Available configuration parameters are listed here:
[https://github.com/kubernetes-sigs/ip-masq-agent#configuring-the-agent](https://github.com/kubernetes-sigs/ip-masq-agent#configuring-the-agent)

Available flags are listed here:
[https://github.com/kubernetes-sigs/ip-masq-agent#agent-flags](https://github.com/kubernetes-sigs/ip-masq-agent#agent-flags)

The design of the package's `kustomize` project allows extending the [container arguments](daemonset-args.yml) by adding, for example, the `--nomasq-all-reserved-ranges` flag used to no masquerade reserved IP ranges by default.

### Important note

You should not attempt to run this agent in a cluster where the Kubelet is also configuring a non-masquerade CIDR.
You can pass `--non-masquerade-cidr=0.0.0.0/0` to the Kubelet to nullify its rule, which will prevent the Kubelet from interfering with this agent.

## Deployment

You can deploy `ip-masq` with the default configuration by running the following command at the root of this project:

```shell
kustomize build katalog/ip-masq | kubectl apply -f -
```

<!-- LINKS -->
[kubeengine-docs]: https://cloud.google.com/kubernetes-engine/docs/how-to/ip-masquerade-agent#create_manual

<!-- </KFD-DOCS> -->

## License

For license details please see [LICENSE](./../../LICENSE)
