Rendu TP2

Thomas Dumont

M1
___
```
apiVersion: apps/v1
kind: Deployment
metadata:
  annotations:
    deployment.kubernetes.io/revision: "1"
  generation: 1
  labels:
    app: web-build
  name: web-build
spec:
  progressDeadlineSeconds: 600
  replicas: 3
  revisionHistoryLimit: 10
  selector:
    matchLabels:
      app: web-build
  strategy:
    rollingUpdate:
      maxSurge: 25%
      maxUnavailable: 25%
    type: RollingUpdate
  template:
    metadata:
      creationTimestamp: null
      labels:
        app: web-build
    spec:
      containers:
      - image: akhadimer/web-build:latest
        imagePullPolicy: Always
        name: web-build
        resources: {}
        terminationMessagePath: /dev/termination-log
        terminationMessagePolicy: File
        volumeMounts:
         - mountPath: /srv/app/pvc
           name: logs
      dnsPolicy: ClusterFirst
      restartPolicy: Always
      schedulerName: default-scheduler
      securityContext: {}
      terminationGracePeriodSeconds: 30
      volumes:
       - name: logs
         persistentVolumeClaim:
          claimName: logs

---
  
apiVersion: v1
kind: PersistentVolume
metadata:
    name: web-build
spec:
    accessModes:
        - ReadWriteOnce
    persistentVolumeReclaimPolicy: Retain
    storageClassName: standard
    capacity:
        storage: 3Gi
    hostPath:
        path: /data/web-build/
  
---

apiVersion: v1
kind: PersistentVolumeClaim
metadata:
    name: logs
    namespace: default
spec:
    accessModes:
        - ReadWriteOnce
    resources:
        requests:
            storage: 1Gi
```