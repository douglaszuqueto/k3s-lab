## ConfigMap

### Operações

```bash

# 1º Criar a partir de um arquivo .env
kubectl create configmap my-config --from-env-file=.env.example

# 2º Descrever o arquivo criado
kubectl describe configmap my-config

# 3º Resgatar determinada Key
kubectl get configmap my-config -o jsonpath='{.data.DB_USER}'

# 4º Criar config a partir de um arquivo manifesto
kubectl apply -f postgres.yml

# 5º Descrever o arquivo criado
kubectl describe configmap postgres-config

# 6º Remover a partir do arquivo de manifesto
kubectl delete -f postgres.yml

# 7º Remover a partir da key
kubectl delete configmaps postgres-config
kubectl delete configmaps my-config

```

### Testar configurações a partir de um Pod

Arquivo de manifesto

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
```bash

# 1º Criar o pod
kubectl apply -f configmap-pod-test.yml

# 2º Visualizar logs
kubectl logs configmap-test-pod

# 3º Remover pod
kubectl delete -f configmap-pod-test.yml
```
