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
kubectl delete -f operator.yaml
kubectl apply -f operator.yaml
## verify the rook-ceph-operator is in the `Running` state before proceeding
kubectl apply -f cluster.yaml
```

## verification

To get a list of all the `rook-ceph` pods:
```
$ kubectl -n rook-ceph get pod
NAME                                             READY   STATUS      RESTARTS   AGE
csi-cephfsplugin-cktzz                           3/3     Running     0          81m
csi-cephfsplugin-provisioner-7469b99d4b-6ztmx    5/5     Running     0          81m
csi-cephfsplugin-provisioner-7469b99d4b-vsh2f    5/5     Running     0          81m
csi-cephfsplugin-zzgpk                           3/3     Running     0          81m
csi-rbdplugin-98mhx                              3/3     Running     0          81m
csi-rbdplugin-provisioner-865f4d8d-mbsn7         6/6     Running     0          81m
csi-rbdplugin-provisioner-865f4d8d-x9wpf         6/6     Running     0          81m
csi-rbdplugin-vwkr6                              3/3     Running     0          81m
rook-ceph-crashcollector-nuc2-56c879b75-n75mb    1/1     Running     0          81m
rook-ceph-crashcollector-nuc3-544c96f59b-2wfmk   1/1     Running     0          80m
rook-ceph-mgr-a-7cdf8dbd64-tq4sg                 1/1     Running     0          25m
rook-ceph-mon-a-6955c688b4-b62vg                 1/1     Running     0          81m
rook-ceph-mon-b-5895995559-sqs55                 1/1     Running     0          81m
rook-ceph-operator-5b6674cb6-xbdx4               1/1     Running     0          82m
rook-ceph-osd-prepare-nuc2-b48rk                 0/1     Completed   0          25m
rook-ceph-osd-prepare-nuc3-gtrxx                 0/1     Completed   0          25m
rook-ceph-tools-67788f4dd7-6rdn8                 1/1     Running     0          79m
rook-discover-dpzqr                              1/1     Running     0          82m
rook-discover-m4fn5                              1/1     Running     0          82m
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

## troubleshooting

If it appears everything is running or completed but you're still left with 0 OSDs, then look at the logs of the `*osd-prepare*` pods.

Get a list of the osd-prepare pods:
```
kubectl -n rook-ceph get pod | grep prepare
```
will return something like:
```
rook-ceph-osd-prepare-nuc2-b48rk                 0/1     Completed   0          36m
rook-ceph-osd-prepare-nuc3-gtrxx                 0/1     Completed   0          35m
```

To view the logs of one of the pods:
```
kubectl -n rook-ceph logs rook-ceph-osd-prepare-nuc2-b48rk
```



