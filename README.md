# Curl and Dummy Pods in K3s Cluster

This repository contains Kubernetes Pod specifications for:
1. A container (`curlpod`) equipped with `curl`.
2. A container (`dummy`) that pings Google every 10 minutes.

These are tested in a K3s cluster created by `k3d` on macOS.

## Instructions
0. Create a cluster call **curlster**
    * `k3d cluster create curlster`  
    * `kubectl get nodes` (Check whether the **curlster** was created successfully or not)

1. Apply the Pod manifests to your cluster: 
    * `kubectl apply -f curlpod-serviceaccount.yaml`
    * `kubectl apply -f pod-list-role.yaml`
    * `kubectl apply -f pod-list-rolebinding.yaml`
    * `kubectl apply -f curl-pod.yaml` 
    * `kubectl apply -f dummy-pod.yaml`

2. Exec into the `curlpod` Pod: 
    * `kubectl exec -it curlpod -- /bin/sh`

3. Once inside the `curlpod`, use `curl` to retrieve all Pods in the `default` namespace:

```bash 
curl -k https://kubernetes.default.svc/api/v1/namespaces/default/pods -H "Authorization: Bearer $(cat /var/run/secrets/kubernetes.io/serviceaccount/token)"
```

4. To check the logs of the `dummy` Pod and see the ping results: