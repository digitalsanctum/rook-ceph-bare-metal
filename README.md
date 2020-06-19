# rook-ceph-bare-metal



## install

Clone or fork this repository.

Make changes to `cluster.yaml` to suit your environment.

Run the following:
```
cd kubernetes
kubectl create -f common.yaml
kubectl create -f operator.yaml
kubectl create -f cluster.yaml
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
kubectl create -f operator.yaml
kubectl create -f cluster.yaml
```

