## ConfigMap

kubectl create configmap my-config --from-env-file=.env.example

kubectl describe configmap my-config

kubectl get configmap my-config -o jsonpath='{.data}'

kubectl get configmap my-config -o jsonpath='{.data.DB_USER}'

### Show configmap in pod

```yml
apiVersion: v1
kind: Pod
metadata:
  name: configmap-test-pod
spec:
  containers:
    - name: configmap-test-container
      image: k8s.gcr.io/busybox
      command: [ "/bin/sh", "-c", "env" ]
      envFrom:
      - configMapRef:
          name: my-config
  restartPolicy: Never
```

kubectl apply -f configmap-pod-test.yml

kubectl logs configmap-test-pod

kubectl delete -f configmap-pod-test.yml

kubectl create -f postgres.yml
kubectl delete -f postgres.yml