## Secret

kubectl create secret generic my-secret --from-literal=key1=supersecret

kubectl create secret generic redis-secret --from-env-file=.env.example

kubectl create secret generic mariadb-user-creds --from-literal=MYSQL_USER=kubeuser --from-literal=MYSQL_PASSWORD=kube-still-rocks

kubectl get secrets my-secret -o jsonpath='{.data.key1}' | base64 --decode

kubectl get secret mariadb-user-creds -o jsonpath='{.data.MYSQL_USER}' | base64 --decode

kubectl get secret mariadb-user-creds -o jsonpath='{.data.MYSQL_PASSWORD}' | base64 --decode

kubectl get secret mariadb-user-creds -o jsonpath='{.data.MYSQL_PASSWORD}' | base64 --decode

### Test secret in pod

kubectl apply -f env.yml

kubectl apply -f secret-pod-test.yml
kubectl logs secret-test-pod

kubectl delete -f secret-pod-test.yml
kubectl delete -f env.yml