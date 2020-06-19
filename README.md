# rook-ceph-bare-metal



## install

Clone or fork this repository.

Make changes to `cluster.yaml` to suit your environment.

Run the following:
```
cd kubernetes
kubectl apply -f common.yaml
kubectl apply -f operator.yaml
## verify the rook-ceph-operator is in the `Running` state before proceeding
kubectl -n rook-ceph get pod
kubectl apply -f cluster.yaml
```

## making changes

If, after you've done an install and you need to make changes, then perform the following steps on each worker node:

```
cd /var/lib/rook
rm -rf mon* rook*
```

Then reinstall the rook service.

```
kubectl delete -f cluster.yaml
kubectl detele -f operator.yaml
kubectl apply -f operator.yaml
kubectl apply -f cluster.yaml
```

## toolbox

### install

```
cd kubernetes
kubectl apply -f toolbox.yaml
```

Wait for running state:
```
watch kubectl -n rook-ceph get pod -l "app=rook-ceph-tools"
```

### connect

Once it's in a running state, you can connect via:
```
kubectl -n rook-ceph exec -it $(kubectl -n rook-ceph get pod -l "app=rook-ceph-tools" -o jsonpath='{.items[0].metadata.name}') bash
```



