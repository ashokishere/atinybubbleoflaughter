Document - Blue Green Deployments
1. Create 2 Pods

```
kubectl run blue-pod --image=nginx
kubectl run green-pod --image=httpd
```

2. Add Labels to both the Pods
```

kubectl label pod blue-pod app=demo-app-v1
kubectl label pod green-pod app=demo-app-v2
```

3. Verify POD labels
```

kubectl get pods --show-labels
```

4. Create Service

blue-green.yaml
```
apiVersion: v1
kind: Service
metadata:
 name: application-service
spec:
 type: NodePort
 ports:
 - name: http
   port: 80
   targetPort: 80
 selector:
   app: demo-app-v1
kubectl apply blue-green.yaml
```

5. Verify Service:
```

kubectl get service
kubectl describe service application-service
```

6. Fetch the Public Endpoint IP of the nodes
```

kubectl get nodes
kubectl describe node <node-name>
```

7. Change from Blue to Green Environment:

Modify the selector to demo-app-v2 in the service manifest file and re-deploy it.

```
kubectl apply blue-green.yaml
```

Second Way to Edit Service (Optional)

```
kubectl edit service application-service
```

Modify the selector to demo-app-v2
