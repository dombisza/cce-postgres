# deployment

```
helmfile sync
kubectl apply -f psql.yaml
```

# example running pod

```
apiVersion: v1
kind: Pod
metadata:
  annotations:
    status: '{"conn_url":"postgres://172.16.2.31:5432/postgres","api_url":"http://172.16.2.31:8008/patroni","state":"running","role":"master","version":"3.2.2","xlog_location":50603048,"timeline":1}'
  creationTimestamp: "2024-05-28T08:19:02Z"
  generateName: acid-upgrade-test-
  labels:
    application: spilo
    cluster-name: acid-upgrade-test
    controller-revision-hash: acid-upgrade-test-5d6bf78878
    spilo-role: master
    statefulset.kubernetes.io/pod-name: acid-upgrade-test-0
    team: acid
  name: acid-upgrade-test-0
  namespace: default
  ownerReferences:
  - apiVersion: apps/v1
    blockOwnerDeletion: true
    controller: true
    kind: StatefulSet
    name: acid-upgrade-test
    uid: b8a54e8d-79b4-4152-98b7-eec8e246e11b
  resourceVersion: "2366617"
  uid: 4e992260-b4e4-4dce-83bd-5cd2f001dda8
spec:
  containers:
  - env:
    - name: SCOPE
      value: acid-upgrade-test
    - name: PGROOT
      value: /home/postgres/pgdata/pgroot
    - name: POD_IP
      valueFrom:
        fieldRef:
          apiVersion: v1
          fieldPath: status.podIP
    - name: POD_NAMESPACE
      valueFrom:
        fieldRef:
          apiVersion: v1
          fieldPath: metadata.namespace
    - name: PGUSER_SUPERUSER
      value: postgres
    - name: KUBERNETES_SCOPE_LABEL
      value: cluster-name
    - name: KUBERNETES_ROLE_LABEL
      value: spilo-role
    - name: PGPASSWORD_SUPERUSER
      valueFrom:
        secretKeyRef:
          key: password
          name: postgres.acid-upgrade-test.credentials.postgresql.acid.zalan.do
    - name: PGUSER_STANDBY
      value: standby
    - name: PGPASSWORD_STANDBY
      valueFrom:
        secretKeyRef:
          key: password
          name: standby.acid-upgrade-test.credentials.postgresql.acid.zalan.do
    - name: PAM_OAUTH2
      value: https://info.example.com/oauth2/tokeninfo?access_token= uid realm=/employees
    - name: HUMAN_ROLE
      value: zalandos
    - name: PGVERSION
      value: "12"
    - name: KUBERNETES_LABELS
      value: '{"application":"spilo"}'
    - name: SPILO_CONFIGURATION
      value: '{"postgresql":{},"bootstrap":{"initdb":[{"auth-host":"md5"},{"auth-local":"trust"}],"dcs":{"failsafe_mode":false}}}'
    - name: DCS_ENABLE_KUBERNETES_API
      value: "true"
    image: ghcr.io/zalando/spilo-16:3.2-p2
    imagePullPolicy: IfNotPresent
    name: postgres
    ports:
    - containerPort: 8008
      protocol: TCP
    - containerPort: 5432
      protocol: TCP
    - containerPort: 8080
      protocol: TCP
    resources:
      limits:
        cpu: "1"
        memory: 500Mi
      requests:
        cpu: 100m
        memory: 100Mi
    securityContext:
      allowPrivilegeEscalation: true
      capabilities:
        add:
        - SYS_RESOURCE
        - SYS_NICE
        - CHOWN
        - SETUID
        - SETGID
        - KILL
        - SETFCAP
        - SETPCAP
        - NET_BIND_SERVICE
        - NET_ADMIN
        - NET_RAW
        - DAC_OVERRIDE
        - FOWNER
        - IPC_LOCK
      privileged: false
      readOnlyRootFilesystem: false
    terminationMessagePath: /dev/termination-log
    terminationMessagePolicy: File
    volumeMounts:
    - mountPath: /home/postgres/pgdata
      name: pgdata
    - mountPath: /dev/shm
      name: dshm
    - mountPath: /var/run/secrets/kubernetes.io/serviceaccount
      name: kube-api-access-gvq4c
      readOnly: true
  dnsConfig:
    options:
    - name: single-request-reopen
      value: ""
    - name: timeout
      value: "2"
  dnsPolicy: ClusterFirst
  enableServiceLinks: true
  hostname: acid-upgrade-test-0
  nodeName: 10.200.10.103
  preemptionPolicy: PreemptLowerPriority
  priority: 0
  restartPolicy: Always
  schedulerName: default-scheduler
  securityContext: {}
  serviceAccount: postgres-pod
  serviceAccountName: postgres-pod
  subdomain: acid-upgrade-test
  terminationGracePeriodSeconds: 300
  tolerations:
  - effect: NoExecute
    key: node.kubernetes.io/not-ready
    operator: Exists
    tolerationSeconds: 300
  - effect: NoExecute
    key: node.kubernetes.io/unreachable
    operator: Exists
    tolerationSeconds: 300
  volumes:
  - name: pgdata
    persistentVolumeClaim:
      claimName: pgdata-acid-upgrade-test-0
  - emptyDir:
      medium: Memory
    name: dshm
  - name: kube-api-access-gvq4c
    projected:
      defaultMode: 420
      sources:
      - serviceAccountToken:
          expirationSeconds: 3607
          path: token
      - configMap:
          items:
          - key: ca.crt
            path: ca.crt
          name: kube-root-ca.crt
      - downwardAPI:
          items:
          - fieldRef:
              apiVersion: v1
              fieldPath: metadata.namespace
            path: namespace
status:
  conditions:
  - lastProbeTime: null
    lastTransitionTime: "2024-05-28T08:19:10Z"
    status: "True"
    type: Initialized
  - lastProbeTime: null
    lastTransitionTime: "2024-05-28T08:19:20Z"
    status: "True"
    type: Ready
  - lastProbeTime: null
    lastTransitionTime: "2024-05-28T08:19:20Z"
    status: "True"
    type: ContainersReady
  - lastProbeTime: null
    lastTransitionTime: "2024-05-28T08:19:10Z"
    status: "True"
    type: PodScheduled
  containerStatuses:
  - containerID: containerd://06947242a1dc09a4e25addb2c769f2017e8b12b94105e0e60c32b2d55c991484
    image: ghcr.io/zalando/spilo-16:3.2-p2
    imageID: ghcr.io/zalando/spilo-16@sha256:705a0f71acc81c5ab1b05c20b15e6b274f8a4903b94e842ef91b5990c8cd9f81
    lastState: {}
    name: postgres
    ready: true
    restartCount: 0
    started: true
    state:
      running:
        startedAt: "2024-05-28T08:19:20Z"
  hostIP: 10.200.10.103
  phase: Running
  podIP: 172.16.2.31
  podIPs:
  - ip: 172.16.2.31
  qosClass: Burstable
  startTime: "2024-05-28T08:19:10Z"
  ```
