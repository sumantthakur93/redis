Install kubernetes and create a 1 master 2 slave redis instance

1. Install kubectl

   ```
   brew install kubectl
   ```

2. Install minikube

   ```
   curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-darwin-arm64
   ```

   ```
   sudo install minikube-darwin-arm64 /usr/local/bin/minikube
   ```

3. Start minikube

   ```
   minikube start --driver=docker --alsologtostderr
   ```

4. Create redis namespace

   ```
   kubectl create ns redis
   ```

5. Create storage class
   ```
   kubectl apply -f redis-storage.yaml
   ```
6. Create persistent volume
   ```
   kubectl apply -f redis-persistent-volume.yaml
   ```
7. Create config-map in the namespace
   ```
   kubectl apply -n redis -f redis-config-map.yaml
   ```
8. Create redis stateful set in the namespace
   ```
   kubectl apply -n redis -f redis-stateful.yaml
   ```
9. Create redis service to be exposed for connections
   ```
   kubectl apply -n redis -f redis-service.yaml
   ```

Verification of setup

1. Login to master node of redis
   ```
   kubectl -n redis exec -it redis-0 -- sh
   ```
2. Connect to redis using redis-cli
   ```
   redis-cli
   auth test-password
   ```
3. Check replication information
   ```
   info replication
   ```
4. Create some data to verify replication
   ```
   SET emp1 Sumant
   SET emp2 Thakur
   ```
5. Login to a slave node of redis cluster
   ```
   kubectl -n redis exec -it redis-0 -- sh
   redis-cli
   auth test-password
   ```
6. Check the data in the replication. If the replication is successful, it will show the same data as the master node
   ```
   KEYS *
   ```
