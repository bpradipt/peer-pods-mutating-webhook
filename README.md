# peer-pods-mutating-webhook
This mutating webhook modifies a POD using specific runtimeclass to remove
all resources entries and replace it with peer-pod extended resource


## Getting Started
You’ll need a Kubernetes cluster to run against. You can use [KIND](https://sigs.k8s.io/kind) to get a local cluster for testing, or run against a remote cluster.
**Note:** Your controller will automatically use the current context in your kubeconfig file (i.e. whatever cluster `kubectl cluster-info` shows).

### Using kind cluster
For `kind` clusters, you can use the following Makefile targets

Create kind cluster
```
make kind-cluster
```
Deploy the webhook in the kind cluster
```
make kind-deploy IMG=<some-registry>/peer-pods-webhook:<tag>
```

If not using `kind`, the follow these steps to deploy the webhook

### Deploy cert-manager
```
kubectl apply -f https://github.com/jetstack/cert-manager/releases/download/v1.6.1/cert-manager.yaml
```

### Running on the cluster
1. Build and push your image to the location specified by `IMG`:
	
```sh
make docker-build && make docker-push IMG=<some-registry>/peer-pods-webhook:<tag>
```
	
2. Deploy the controller to the cluster with the image specified by `IMG`:

```sh
make deploy IMG=<some-registry>/peer-pods-webhook:<tag>
```

3. To delete the webhook from the cluster:

```sh
make undeploy
```

### Testing
1. Create the runtimeclass
```sh
kubectl apply -f hack/rc.yaml
```
2. Create the pod
```sh
kubectl apply -f hack/pod.yaml
```
3. View the mutated pod
```sh
kubectl get -f hack/pod.yaml -o yaml | grep kata.peerpods
```
