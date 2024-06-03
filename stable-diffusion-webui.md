# stable-diffusion-webui

## 실행
1. 클론
2. 실행 `./webui.sh`

### 옵션
- 커맨드라인 옵션
  + https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/Command-Line-Arguments-and-Settings
  - 해당 설정 적용은 `webui-user.sh` 에서 덮어쓰게되어있으니 해당 파일을 참조해서 처리해야한다

###  모델 위치 변경등
- `webui-user.sh` 참고
  - `COMMANDLINE_ARGS=--data-dir [절대경로]` 내용 포함, 상대경로 처리안됨
  - `webui.sh` 실행시 내부적으로 `webui-user.sh`도 함께 실행된다
  - `webui-user.sh` 자체도 `git` 으로 관리되는 파일이기 때문에 수정하기 시작하면 업데이트(`pull`) 시에 귀찮아진다.
- 혹은 `models/` 내에 `ln -s` 를 통해서 심볼릭 링크를 사용할 수 있다.

## [[error]]
### Cannot locate TCMalloc
- [[ubuntu]] `./webui.sh` 실행시 에러
```sh 
Cannot locate TCMalloc. Do you have tcmalloc or google-perftool installed on your system? (improves CPU memory usage)
```
  - 패키지 설치
  ```sh 
  sudo apt install google-perftools
  ```
  + https://github.com/AUTOMATIC1111/stable-diffusion-webui/issues/10117#issuecomment-1536437860

### RuntimeError: Torch is not able to use GPU; add --skip-torch-cuda-test to COMMANDLINE_ARGS variable to disable this check
- [[cuda]] 참조

### remote 서버로 사용시 ip 로 접속이 안됨
- [[ubuntu]] 인 경우
```sh
sudo service ufw stop # 방화벽 제거
./webui.sh --listen
```
  - `--listen` 옵션을 줘야지 로컬외 접속이 허용된다
  + https://www.reddit.com/r/StableDiffusion/comments/15ge386/comment/jui85sv/?utm_source=share&utm_medium=web3x&utm_name=web3xcss&utm_term=1&utm_content=share_button
  
### `Failed to build clip`
```sh 
Building wheels for collected packages: clip
  Building wheel for clip (setup.py): started
  Building wheel for clip (setup.py): finished with status 'error'
  Running setup.py clean for clip
Failed to build clip

stderr:   error: subprocess-exited-with-error

  × python setup.py bdist_wheel did not run successfully.
  │ exit code: 1
  ╰─> [89 lines of output]
      /tmp/pip-req-build-62akizb5/setup.py:3: DeprecationWarning: pkg_resources is deprecated as an API. See https://setuptools.pypa.io/en/latest/pkg_resources.html
        import pkg_resources
      running bdist_wheel
      running build
      running build_py
      file clip.py (for module clip) not found
      creating build
      creating build/lib
      creating build/lib/clip
      copying clip/simple_tokenizer.py -> build/lib/clip
      copying clip/model.py -> build/lib/clip
      copying clip/__init__.py -> build/lib/clip
      copying clip/clip.py -> build/lib/clip
      running egg_info
      creating clip.egg-info
      writing clip.egg-info/PKG-INFO
      writing dependency_links to clip.egg-info/dependency_links.txt
      writing requirements to clip.egg-info/requires.txt
      writing top-level names to clip.egg-info/top_level.txt
      writing manifest file 'clip.egg-info/SOURCES.txt'
      file clip.py (for module clip) not found
      reading manifest file 'clip.egg-info/SOURCES.txt'
      reading manifest template 'MANIFEST.in'
      adding license file 'LICENSE'
      writing manifest file 'clip.egg-info/SOURCES.txt'
      copying clip/bpe_simple_vocab_16e6.txt.gz -> build/lib/clip
      file clip.py (for module clip) not found
      warning: build_py: byte-compiling is disabled, skipping.

      /home/deptno/.local/data/virtualenvs/stable-diffusion-webui-Q-TMEvtD/lib/python3.10/site-packages/setuptools/_distutils/cmd.py:66: SetuptoolsDeprecationWarning: setup.py install is deprecated.
      !!

              ********************************************************************************
              Please avoid running ``setup.py`` directly.
              Instead, use pypa/build, pypa/installer or other
              standards-based tools.

              See https://blog.ganssle.io/articles/2021/10/setup-py-deprecated.html for details.
              ********************************************************************************

      !!
        self.initialize_options()
      installing to build/bdist.linux-x86_64/wheel
      running install
      running install_lib
      Traceback (most recent call last):
        File "<string>", line 2, in <module>
        File "<pip-setuptools-caller>", line 34, in <module>
        File "/tmp/pip-req-build-62akizb5/setup.py", line 6, in <module>
#...
      AttributeError: install_layout. Did you mean: 'install_platlib'?
      [end of output]

  note: This error originates from a subprocess, and is likely not a problem with pip.
  ERROR: Failed building wheel for clip
ERROR: Could not build wheels for clip, which is required to install pyproject.toml-based projects
```
- 아래 패키지를 업데이트한 후 재 실행한다
```sh 
pip install --upgrade setuptools wheel
```

## link
- [[ai]]
- [[midjourney]]
- [[comfyui]]
- [[python]]
- [[book/making-game-graphic-with-stable-diffusion]]
- [[huggingface-ml-for-games]]
- [[animatediff]]
