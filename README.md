# Deploy Argocd In Cluster OKD

1. Install Argo CD
```
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```
2. Sample Route To Apply

```
kind: Route
apiVersion: route.openshift.io/v1
metadata:
  name: argocd
  namespace: argocd
  uid: 7ce4e47e-b87b-4dc9-87e6-c62a47e3b8f8
  resourceVersion: '7029588'
  creationTimestamp: '2024-03-12T03:45:05Z'
  labels:
    app.kubernetes.io/component: server
    app.kubernetes.io/name: argocd-server
    app.kubernetes.io/part-of: argocd
  managedFields:
    - manager: openshift-router
      operation: Update
      apiVersion: route.openshift.io/v1
      time: '2024-03-12T03:45:05Z'
      fieldsType: FieldsV1
      fieldsV1:
        'f:status':
          'f:ingress': {}
      subresource: status
    - manager: Mozilla
      operation: Update
      apiVersion: route.openshift.io/v1
      time: '2024-03-12T03:46:58Z'
      fieldsType: FieldsV1
      fieldsV1:
        'f:metadata':
          'f:labels':
            .: {}
            'f:app.kubernetes.io/component': {}
            'f:app.kubernetes.io/name': {}
            'f:app.kubernetes.io/part-of': {}
        'f:spec':
          'f:host': {}
          'f:port':
            .: {}
            'f:targetPort': {}
          'f:tls':
            .: {}
            'f:insecureEdgeTerminationPolicy': {}
            'f:termination': {}
          'f:to':
            'f:kind': {}
            'f:name': {}
            'f:weight': {}
          'f:wildcardPolicy': {}
spec:
  host: argocd.apps.prod.okd
  to:
    kind: Service
    name: argocd-server
    weight: 100
  port:
    targetPort: https
  tls:
    termination: passthrough
    insecureEdgeTerminationPolicy: Redirect
  wildcardPolicy: None
status:
  ingress:
    - host: argocd.apps.prod.okd
      routerName: default
      conditions:
        - type: Admitted
          status: 'True'
          lastTransitionTime: '2024-03-12T03:45:05Z'
      wildcardPolicy: None
      routerCanonicalHostname: router-default.apps.prod.okd

```
3. Show The Password Admin on Argo Dashboard
```
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
```