RBAC permissions to POD's

When we crreted with  eksctl our cluster, we created a mapping (we can have multiple service accounts here defined):

```
...
iam:
  withOIDC: true
  serviceAccounts:
    - metadata:
        name: iam-foxize
        # if no namespace is set, "default" will be used;
        # the namespace will be created if it doesn't exist already
        namespace: pro
        labels: {aws-usage: "foxizecloud"}
      attachPolicyARNs:
        - "arn:aws:iam::aws:policy/AmazonS3FullAccess"
        - "arn:aws:iam::aws:policy/AmazonSQSFullAccess" # Para acceder a SQS
        - "arn:aws:iam::aws:policy/AmazonSESFullAccess" # Para acceder a SES
        - "arn:aws:iam::aws:policy/AWSCertificateManagerFullAccess" # Para acceder a ACM
...
```
Check current serviceaccounts:
```
kubectl get serviceaccounts -n pro
```

Now we need to assign the service account "iam-foxize" to the PODs:

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-nginx
  namespace: pro
spec:
  selector:
    matchLabels:
      run: my-nginx
  replicas: 1
  template:
    metadata:
      labels:
        run: my-nginx
    spec:
      serviceAccountName: iam-foxize   <<<<<<====
      containers:
      - name: my-nginx
        image: nginx
        ports:
        - containerPort: 80
        resources:
          requests:
            memory: 512Mi
            cpu: 500m
          limits:
            memory: 1Gi
            cpu: 500m
      nodeSelector:   # Node to POD matching
        nodegroup: front
```

