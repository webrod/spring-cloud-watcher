Those are a set of Kubernetes yaml files to deploy the [spring-cloud-kubernetes-configuration-watcher](https://docs.spring.io/spring-cloud-kubernetes/docs/current/reference/html/#spring-cloud-kubernetes-configuration-watcher).

The goal is to illustrate the deployment of the watcher that should monitor 2 config maps from 2 different namespaces.
We deploy:
* the watcher in 'cw' namespace
* one configmap (cm1) in namespace1 ns
* another configmap (cm2) in namespace2 ns

For now, we do not deploy the backend applications that will consume the configmaps and will be refresed by the watcher, that will be done later.
The immediate goal is to support this github issue where we explain the support for multiple namespaces do not work:
https://github.com/spring-cloud/spring-cloud-kubernetes/issues/1199

Run those commands to deploy everything:
```
kubectl apply -f ns.yaml
kubectl apply -f role.yaml
kubectl apply -f deployment.yaml
kubectl apply -f cm.yaml
```

Run this command to see how many configmaps are watched (for now it shows 1 instead of 2 because of the 'bug' (or misconfiguration?)):
```
kubectl logs -l app=configuration-watcher --tail=-1 -n cw | grep "was added"
```

Run those commands to remove everything:
```
kubectl delete -f role.yaml
kubectl delete -f deployment.yaml
kubectl delete -f cm.yaml
kubectl delete -f ns.yaml
```
