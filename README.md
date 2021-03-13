# Kubernetes Quick 101
How to install K8S, set up your cluster and do basic `kubectl` operations in it.

# Installing:
https://kubernetes.io/docs/tasks/tools/

# Configuring certificate, cleint and private key:
```
mkdir -p ~/.kube
```
```
cat << EOF > ~/.kube/k8scc.crt
-----BEGIN CERTIFICATE-----
<CERTIFICATE HERE>
-----END CERTIFICATE-----
EOF
```
```
cat << EOF > ~/.kube/k8scc-client.crt
-----BEGIN CERTIFICATE-----
<CLIENT HERE>
-----END CERTIFICATE-----
EOF
```
```
cat << EOF > ~/.kube/k8scc-client.key
-----BEGIN RSA PRIVATE KEY-----
<PRIVATE RSA KEY HERE>
-----END RSA PRIVATE KEY-----
EOF
```

# Setting up the cluster:
After these steps, we are now able to set our cluster, credentials and context.
```
cd ~/.kube
kubectl config set-cluster k8scc --server <SERVER HERE> --certificate-authority=k8scc.crt
kubectl config set-credentials k8scc --client-certificate=k8scc-client.crt --client-key=k8scc-client.key
kubectl config set-context k8scc --user=k8scc --cluster=k8scc
kubectl config use-context k8scc
```

# YAML:
Now, it's time for your first YAML file. K8S is a YAML-based container orquestrating tool so it's important to understand what the YAML is saying that it will create.
```
vim testpod.yaml
```
```
apiVersion: v1
kind: Pod
metadata:
  name: testpod
spec:
  containers:
    - name: testpod
      image: alpine:latest
      command: ["sleep", "3600"]
```

# Basic Commands
Building the YAML:
```
kubectl apply -f <YAML>
```

Shelling into a pod:
```
kubectl exec --stdin --tty testpod -- /bin/sh
```

Checking the logs of a specific pod:
```
kubectl logs -f <POD NAME>
```

Deleting a pod:
```
kubectl delete podo <POD NAME>
```

Deleting all pods:
```
kubectl delete pods --all
```

Deleting pods with label <LABEL>:
```
kubectl delete pods -l app=<LABEL>
```