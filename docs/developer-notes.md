# Developer notes

## Kubernetes

Kubernetes is running the RPC server as a `statefulSet` which is pulled from a
container image on a registry. This means that (local) changes to the RPC
server are **not** reflected on the RPC server when running in Kubernetes,
unless you **also** push an updated image to a registry and update the
Kubernetes config files.

To help with this a helper script is provided: [build-k8s-rpc.sh](../scripts/build-k8s-rpc.sh).

This script can be run in the following way:

```bash
DOCKER_REGISTRY=bitcoindevproject/warnet-rpc TAG=0.1 ./scripts/build-k8s-rpc.sh Dockerfile_rpc
```
> [!important]
> The `TAG` used **must** match the `SERVER_VERSION` found in [server.py](src/warnet/server.py]

The versioning/tagging is so that the CLI will call the correct URL on the RPC server, and breaking changes will be obvious to the user as no RPCs will be found at the called URL.

Once a new image has been pushed, it should be referenced in [warnet-rpc-statefulset.yaml](../src/templates/warnet-rpc-statefulset.yaml) in the `image` field.
