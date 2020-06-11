## Storage

### Persistent Volumes

Arquivo de manifesto

```yml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: auth-pv-volume
  labels:
    type: local
spec:
  storageClassName: manual
  capacity:
    storage: 1Gi
  accessModes:
    - ReadWriteOnce
  hostPath:
    path: "/mnt/data"

```

### Persistent Volumes Claims

Arquivo de manifesto

```yml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: auth-pv-claim
spec:
  storageClassName: manual
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 1Gi
```

Pod

```yml
apiVersion: v1
kind: Pod
metadata:
  name: volume-test-pod
spec:
    restartPolicy: Never
    containers:
      - name: volume-test-container
        image: k8s.gcr.io/busybox
        command: [ "ls", "-la", "/data" ]
        volumeMounts:
          - mountPath: /data
            name: volume
    volumes:
      - name: volume
        persistentVolumeClaim:
          claimName: auth-pv-claim
```