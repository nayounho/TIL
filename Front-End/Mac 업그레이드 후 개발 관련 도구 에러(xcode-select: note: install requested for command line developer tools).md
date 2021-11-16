# xcrun: error: invalid active developer path (/Library/Developer/CommandLineTools), missing xcrun at: /Library/Developer/CommandLineTools/usr/bin/xcrun

- 최근 Mac 업그레이드를 진행하였는데 git를 사용하려고 보니 이런 에러가 발생
- 구글링 해보니 Mac 업그레이드 마다 이러한 에러가 발생하는 것 같아 아예 메모 해둬야 겠다는 생각에...

## 해결 방법
- xcode-select 명령으로 xcode cli만 따로 설치해서 이 문제를 해결해보자

```
xcode-select --install
```

- 대략 3~5분 정도면 설치 완료

```
$ git
usage: git [--version] [--help] [-C <path>] [-c <name>=<value>]
           [--exec-path[=<path>]] [--html-path] [--man-path] [--info-path]
           [-p | --paginate | -P | --no-pager] [--no-replace-objects] [--bare]
           [--git-dir=<path>] [--work-tree=<path>] [--namespace=<name>]
           <command> [<args>]

These are common Git commands used in various situations:

start a working area (see also: git help tutorial)
   clone             Clone a repository into a new directory
   init              Create an empty Git repository or reinitialize an existing one

   ...skip
```