# tekton

[[kubernetes]] ci/cd

---
Event > PipelineBinding > PipelineTemplate > PipelineRun > Pipeline > Task

- Event
- PipelineBinding 
- PipelineTemplate 
- PipelineRun 
- Pipeline 
- Task


|   |                  |                          |   |   |
|---|------------------|--------------------------|---|---|
|   | Event            |                          |   |   |
|   | PipelineBinding  | 파이프라인에 변수 바인딩 |   |   |
|   | PipelineTemplate |                          |   |   |
|   | PipelineRun      | 파이프라인의 실행        |   |   |
|   | Pipeline         | Task 콜렉션              |   |   |
|   | Task             |                          |   |   |


## install
- [[diary/2023-01-19]] 
- tutorial
  + https://tekton.dev/docs/how-to-guides/clone-repository/
    - `tkn hub install task git-clone` 에러가 나므로 `kubectl apply -f` 를 통한 직접 설치가 필요 2023-01-20 


### pipeline
+ https://tekton.dev/docs/pipelines/install/#installing-tekton-pipelines-on-kubernetes
```sh
kubectl apply --filename https://storage.googleapis.com/tekton-releases/pipeline/latest/release.yaml
```
`tecton-pipelines` namepsace 로 설치됨

- pvc 설정
```sh
apiVersion: v1
kind: ConfigMap
metadata:
  labels:
    app.kubernetes.io/instance: default
    app.kubernetes.io/part-of: tekton-pipelines
  name: config-artifact-pvc
  namespace: tekton-pipelines
data:
  storageClassName: "openebs-hostpath"
  size: "10GiB"
```

- private repo 설정
+ https://tekton.dev/docs/installation/pipelines/#configuring-cloudevents-notifications

- pipeline 설정 `cm/feature-flags` 에 있다
  - [ ] require-git-ssh-secret-known-hosts
  - disable-creds-init credential initialization 을 스킵하고 secret 로드로 대체

### dashboard
```sh 
kubectl apply --filename \
https://storage.googleapis.com/tekton-releases/dashboard/latest/tekton-dashboard-release.yaml
```

### cli
+ https://github.com/tektoncd/cli
```sh
brew install tektoncd-cli
```

## error
pipelinerun 을 통해서 pod 생성 후 계속 pending 상태라 보니 pvc 가 바운드되지 않는 문제
```sh
Events:
  Type     Reason            Age   From               Message
  ----     ------            ----  ----               -------
  Warning  FailedScheduling  2m8s  default-scheduler  0/1 nodes are available: pod has unbound immediate PersistentVolumeClaims. preemption: 0/1
 nodes are available: 1 No preemption victims found for incoming pod..
```
-> pvc 를 가보니 storageClassName 이 비어있다, 현 세팅은 `openebs-hostpath` 를 지정해야 사용이 가능한 상태
-> manifest 에서 pvc 설정에 storageClassName 을 설정했음에도 동작하지 않았다.
-> pvc 에서 `spec.storageClassName: openebs-hostpath` 를 주입하니 정상 실행된다.
-> `PipelineRun` 에서 `storageClassName` 을 주입하면 동작한다
```yaml
  workspaces:
  - name: shared-data
    volumeClaimTemplate:
      spec:
        accessModes:
        - ReadWriteOnce
        resources:
          requests:
            storage: 1Gi
        storageClassName: openebs-hostpath
```
+ https://tekton.dev/docs/pipelines/workspaces/#using-persistent-volumes-within-a-pipelinerun

## releated
- [[kubernetes]]
- [[metrics-server]]
