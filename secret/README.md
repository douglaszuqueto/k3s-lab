## Secret

### Operações

```bash

# 1º Criar manualmente
kubectl create secret generic my-secret \
    --from-literal=DB_USER=dbuser \
    --from-literal=DB_PASSWORD=dbpassword

# 2º Criar a partir de um arquivo .env
kubectl create secret generic db-secret --from-env-file=.env.example

# 3º Resgatar determinada key e fazer o decode
kubectl get secret my-secret -o jsonpath='{.data.DB_USER}' | base64 --decode

# 4º Criar secret a partir de um arquivo manifesto
kubectl apply -f env.yml

# 5º Descrever o arquivo criado
kubectl describe secret env-secret

# 6º Remover a partir do arquivo de manifesto
kubectl delete -f env.yml

# 7º Remover a partir da key
kubectl delete secret env-secret
kubectl delete secret db-secret
```

### Testar configurações a partir de um Pod

Arquivo de manifesto

```yml
apiVersion: v1
kind: Pod
metadata:
  name: secret-test-pod
spec:
  containers:
    - name: secret-test-container
      image: k8s.gcr.io/busybox
      command: [ "/bin/sh", "-c", "env" ]
      envFrom:
      - secretRef:
          name: my-secret
  restartPolicy: Never
```

```bash

# 1º Criar o pod
kubectl apply -f secret-pod-test.yml

# 2º Visualizar logs
kubectl logs secret-test-pod

# 3º Remover pod
kubectl delete -f secret-pod-test.yml

# 4º Remover my-secret
kubectl delete secret my-secret

```