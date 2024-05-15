# unity

## 설치
- [[unity-hub]]

## [[error]]
### [[macos-monterey]]
- `process_symbols.sh`
- ``fatal error: runtime: bsdthread_register error``
- [[twid]]: d6944dbe-f457-47b0-82f5-a90cbc06470d


```sh
PhaseScriptExecution Unity\ Process\ symbols /Users/deptno/Library/Developer/Xcode/DerivedData/[APP_NAME]-gelxelmdoqwomafpddbmvjtcaozi/Build/Intermediates.noindex/ArchiveIntermediates/[APP_NAME]/IntermediateBuildFilesPath/Unity-iPhone.build/Release-iphoneos/UnityFramework.build/Script-36C3405996BD3813E4CCDCBA.sh (in target 'UnityFramework' from project 'Unity-iPhone')

    export variant\=normal /bin/sh -c /Users/deptno/Library/Developer/Xcode/DerivedData/[APP_NAME]-gelxelmdoqwomafpddbmvjtcaozi/Build/Intermediates.noindex/ArchiveIntermediates/[APP_NAME]/IntermediateBuildFilesPath/Unity-iPhone.build/Release-iphoneos/UnityFramework.build/Script-36C3405996BD3813E4CCDCBA.sh
fatal error: runtime: bsdthread_register error

runtime stack:
runtime.throw(0x1622388, 0x21)
	/usr/local/go/src/runtime/panic.go:605 +0x95 fp=0x7ff7bfef5070 sp=0x7ff7bfef5050 pc=0x1029c75
runtime.goenvs()
	/usr/local/go/src/runtime/os_darwin.go:108 +0x83 fp=0x7ff7bfef50a0 sp=0x7ff7bfef5070 pc=0x1027513
runtime.schedinit()
	/usr/local/go/src/runtime/proc.go:492 +0xa1 fp=0x7ff7bfef50e0 sp=0x7ff7bfef50a0 pc=0x102c651
runtime.rt0_go(0x7ff7bfef5118, 0x3, 0x7ff7bfef5118, 0x0, 0x1000000, 0x3, 0x7ff7bfef6370, 0x7ff7bfef63df, 0x7ff7bfef63eb, 0x0, ...)
	/usr/local/go/src/runtime/asm_amd64.s:175 +0x1eb fp=0x7ff7bfef50e8 sp=0x7ff7bfef50e0 pc=0x10540fb
Command PhaseScriptExecution failed with a nonzero exit code
n
```

- [[macos-monterey]] 에서 [[deprecated]] 된 system call 을 사용해서 발생하는 문제

> https://forum.unity.com/threads/xcode-build-issues-with-macos-monterey.1189546/

```text
Here are what can be done:
1. Remove Unity Process symbols or Process symbols step from Build Phases in both Unity-iPhone and UnityFramework targets. upload_2021-11-10_14-9-12.png

2. Or you can empty this script:
/Applications/Unity/Hub/Editor/[UnityVersion]/PlaybackEngines/iOSSupport/Trampoline/process_symbols.sh. Remove everything except #!/bin/sh.

BE AWARE: Crash stack traces in Unity Dashboard will become unreadable.
```
  
`process_symbols.sh` 파일에서 빌드 관련 내용을 주석 처리하던가
- [[usymtool]] 버전을 올려서 해결 2021.2.8

## link
- [[macos-monterey]]
- [[fastlane]]
- [[xcode]]
- [[unity-learn-create-with-code]]
- [[huggingface-ml-for-games]]
