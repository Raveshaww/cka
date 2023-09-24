# Exam Tips
These tips are meant for my own personal advice for CKA
### Tips
- Take some time to get used to json paths:
    - `kubectl -n admin2406 get deployment -o custom-columns=DEPLOYMENT:.metadata.name,CONTAINER_IMAGE:.spec.template.spec.containers[].image,READY_REPLICAS:.status.readyReplicas,NAMESPACE:.metadata.namespace --sort-by=.metadata.name > /opt/admin2406_data`
    - `kubectl get nodes -o jsonpath='{.items[*].status.nodeInfo.osImage}' > /opt/outputs/nodes_os_x43kj56.txt`
    - `kubectl get nodes -o jsonpath='{.items[*].status.addresses[?(@.type=="InternalIP")].address}'`
    - `kubectl --context cluster1 get pod -n kube-system kube-apiserver-cluster1-controlplane  -o jsonpath='{.metadata.labels.component}'`
- review user creation
    - remember `RBAC`
    - imperative may be the easiest way to go:
        - `kubectl --context cluster1 create serviceaccount deploy-cka20-arch`
        - `kubectl --context cluster1 create clusterrole deploy-role-cka20-arch --resource=deployments --verb=get`
        - `kubectl --context cluster1 create clusterrolebinding deploy-role-binding-cka20-arch --clusterrole=deploy-role-cka20-arch --serviceaccount=default:deploy-cka20-arch`
            - Note that in the role binding, a `user` is different than a `serviceaccount`
- don't forget about `k top`
- review networking
    - You can expose an existing pod like this:
        - `kubectl expose pod nginx-resolver --name=nginx-resolver-service --port=80 --target-port=80 --type=ClusterIP`
    - when running nslookup from a pod, use the name of the `service`
        - i.e.:
            - `kubectl run test-nslookup --image=busybox:1.28 --rm -it --restart=Never -- nslookup nginx-resolver-service`
- pod logs can be found at `/var/log/pods`
- container logs at `/var/log/containers`
- You can sort out pods like this:
    - `kubectl get pod -A --sort-by=.metadata.uid`
- to show all namespaced resources:
    - `k api-resources --namespaced -o name > /opt/course/16/resources.txt`