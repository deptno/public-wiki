# terraform

## terraform@0.13
사용버전: terraform@1.1.7
요약:
- terraform 0.13에서 upgrade를 요함
- 바이너리 제공안하니 소스 빌드할 것
- 소스빌드하려면 go@1.14 필요
- go@1.14 설치하려면 x86_64 아키텍쳐로 시도할것

```sh
$ terraform init

Initializing the backend...
╷
│ Error: Invalid legacy provider address
│
│ This configuration or its associated state refers to the unqualified provider "aws".
│
│ You must complete the Terraform 0.13 upgrade process before upgrading to later versions.
╵

$ brew install terraform@0.13
Running `brew update --preinstall`...
==> Auto-updated Homebrew!
Updated 2 taps (homebrew/core and homebrew/cask).
==> Updated Formulae
Updated 12 formulae.
==> Updated Casks
Updated 7 casks.

Warning: terraform@0.13 has been deprecated because it is not supported upstream!
Error: terraform@0.13: no bottle available!
You can try to install from source with:
  brew install --build-from-source terraform@0.13
Please note building from source is unsupported. You will encounter build
failures with some formulae. If you experience any issues please create pull
requests instead of asking for help on Homebrew's GitHub, Twitter or any other
official channels.
$ brew install --build-from-source terraform@0.13
Warning: terraform@0.13 has been deprecated because it is not supported upstream!
Warning: go@1.14 has been deprecated because it is not supported upstream!
Error: go@1.14: no bottle available!
You can try to install from source with:
  brew install --build-from-source go@1.14
Please note building from source is unsupported. You will encounter build
failures with some formulae. If you experience any issues please create pull
requests instead of asking for help on Homebrew's GitHub, Twitter or any other
official channels.
$ brew install --build-from-source go@1.14

Warning: go@1.14 has been deprecated because it is not supported upstream!
go@1.14: The x86_64 architecture is required for this software.
Error: go@1.14: An unsatisfied requirement failed this build.
```


## related
- [[isc]]
