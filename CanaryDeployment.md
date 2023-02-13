Document - Canary Deployments
1. Create Deployment Manifests:

```
kubectl create deployment v1-app --image=nginx --replicas 3 --dry-run=client -o yaml
kubectl create deployment v2-app --image=httpd --replicas 1 --dry-run=client -o yaml
```

2. Store these manifests in v1-canary.yaml and v2-canary.yaml

3. Add a common label of deptype: canary to both of these manifest files for the pod template section

4. Create deployment in K8s
```
kubectl apply -f v1-canary.yaml
kubectl apply -f v1-canary.yaml
```

5. Verify if PODS are created with appropriate labels:

```kubectl get pods --show-labels
```
6. Create a Canary Service

canary-svc.yaml
```
apiVersion: v1
kind: Service
metadata:
 name: canary-deployment
spec:
 type: NodePort
 ports:
 - name: http
   port: 80
   targetPort: 80
 selector:
   deptype: canary
```
```
kubectl apply -f canary-svc.yaml
```

7. Verify if Service have 4 Endpoint IPs:
```
kubectl describe svc canary-deployment
```

8. Make CURL request to see if requests are distributed among v1 and v2 apps

```
curl PUBLIC-IP:NODEPORT
```
