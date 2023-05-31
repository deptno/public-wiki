# dockerfile

## multistage build
## kubernetes 에 의해서 재정의 될 수 있는 구문

아래 구문을 통해서 쿠버네티스에서 재정의가 가능하므로 해당 구문때문에 다커를 다시 빌드할 필요는 없다.
[[monorepo|모노레포]] 로 된 구성에 특히 유용하다

|   | dockerfile      | kubernetes                                        |
|---|-----------------|---------------------------------------------------|
|   | WORKDIR         | spec.containers[].workingDir                      |
|   | CMD, ENTRYPOINT | spec.containers[].command, spec.containers[].args |
|   | ENV             | spec.containers[].env                             |
|   | EXPOSE          | spec.containers[].ports                           |
|   | VOLUME          | spec.volumes, spec.containers[].volumeMounts      |
|   | USER            | spec.containers[].securityContext.runAsUser       |

## related
- [[docker]]
- [[kubernetes]]
- [[monorepo]]
