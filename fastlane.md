# fastlane

## [[error]]

### Invalid username and password combination. Used 'XXXX@XXXX.com' as the username.
> 계정이슈
```sh
[10:19:46]: fastlane finished with errors

Looking for related GitHub issues on fastlane/fastlane...

Found no similar issues. To create a new issue, please visit:
https://github.com/fastlane/fastlane/issues/new
Run `fastlane env` to append the fastlane environment to your issue

[!] The request could not be completed because:
	Invalid username and password combination. Used 'XXXX@XXXX.com' as the username.
```
```sh
fastlane fastlane-credentials remove --username XXXX@XXXX.com
```


### fatal error: runtime: bsdthread_register error
```sh
▸ Running script 'Unity Process symbols'

❌  fatal error: runtime: bsdthread_register error


** ARCHIVE FAILED **


The following build commands failed:
        PhaseScriptExecution Unity\ Process\ symbols /Users/deptno/Library/Developer/Xcode/DerivedData/[APP_NAME]-afpmodexeqnqytfpispoaxqrjqft/Build/Intermediates.noindex/ArchiveIntermediates/[APP_NAME]/IntermediateBuildFilesPath/Unity-iPhone.build/Release-iphoneos/UnityFramework.build/Script-84294926AA792CC5C43FB36D.sh (in target 'UnityFramework' from project 'Unity-iPhone')
(1 failure)
[12:14:32]: Exit status: 65

+---------------+-------------------------+
|            Build environment            |
+---------------+-------------------------+
| xcode_path    | /Applications/Xcode.app |
| gym_version   | 2.199.0                 |
| export_method | app-store               |
| sdk           | iPhoneOS15.0.sdk        |
+---------------+-------------------------+

[12:14:32]: ▸ runtime.schedinit()
[12:14:32]: ▸   /usr/local/go/src/runtime/proc.go:492 +0xa1 fp=0x7ff7bfef50d0 sp=0x7ff7bfef5090 pc=0x102c651
[12:14:32]: ▸ runtime.rt0_go(0x7ff7bfef5100, 0x3, 0x7ff7bfef5100, 0x1000000, 0x3, 0x7ff7bfef6360, 0x7ff7bfef63ce, 0x7ff7bfef63da, 0x0, 0x7ff7bfef64b1, ...)
[12:14:32]: ▸   /usr/local/go/src/runtime/asm_amd64.s:175 +0x1eb fp=0x7ff7bfef50d8 sp=0x7ff7bfef50d0 pc=0x10540fb
[12:14:32]: ▸ Command PhaseScriptExecution failed with a nonzero exit code
[12:14:32]:
[12:14:32]: ⬆️  Check out the few lines of raw `xcodebuild` output above for potential hints on how to solve this error
[12:14:32]: 📋  For the complete and more detailed error log, check the full log at:
[12:14:32]: 📋  /Users/deptno/Library/Logs/gym/[APP_NAME]-[APP_NAME].log
```

`/Users/deptno/Library/Logs/gym/[APP_NAME]-[APP_NAME].log` 형태로 로그가 저장되니 여기서 확인하면 된다.
[[unity]] 항목 확인

## releated
- [[ios]]
- [[cert]]
- [[github]]
- [[unity]]
