# Linux 셸: 명령, 경로와 도움말

Linux에서 Bash와 GNU 기본 명령을 사용하는 환경을 기준으로 한다.

## 터미널과 셸

터미널은 문자를 입력하고 출력을 보는 인터페이스다. 셸은 입력한 명령을 해석하고 실행하는 프로그램이며 Bash는 그중 하나다. `cd`는 Bash 내장 명령으로 현재 셸의 작업 디렉터리를 바꾼다. `ls`는 보통 별도 프로그램으로 실행된다. 실제 환경에서는 별칭이나 함수가 같은 이름을 사용할 수 있으므로 `type cd`, `type ls`로 확인한다. [Bash 매뉴얼](https://man7.org/linux/man-pages/man1/bash.1.html)

## 명령 이름과 인자

```sh
ls -a notes
ls "study notes"
```

첫 줄에서 `ls`는 명령 이름, `-a`는 옵션, `notes`는 대상 디렉터리다. 옵션도 인자에 포함된다. 둘째 줄의 따옴표는 공백이 있는 이름을 하나의 인자로 전달한다. 따옴표가 없으면 `study`와 `notes`라는 두 대상이 된다.

Bash가 `/` 없는 외부 명령 이름을 찾을 때 사용하는 `PATH`와, 명령에 전달하는 대상 파일의 경로는 별개다. `PATH`는 `ls`의 대상 파일을 찾아 주지 않는다. [Bash 매뉴얼](https://man7.org/linux/man-pages/man1/bash.1.html)

`ls`는 대상이 없으면 현재 작업 디렉터리의 항목을 표시한다. 기본적으로 점으로 시작하는 이름은 생략하며, `ls -a`는 그런 이름과 `.`·`..`도 포함한다. 목록을 보는 것은 작업 디렉터리를 바꾸거나 파일 내용을 출력하는 동작이 아니다. [ls 매뉴얼](https://man7.org/linux/man-pages/man1/ls.1.html)

## 경로의 기준 위치

현재 작업 디렉터리는 상대 경로의 출발점이다. `pwd`로 확인하고 `cd`로 변경한다.

| 표현 | 의미 |
| --- | --- |
| `/home/mina/notes/today.txt` | 루트 `/`에서 출발하는 절대 경로 |
| `notes/today.txt` | 현재 작업 디렉터리에서 출발하는 상대 경로 |
| `./today.txt` | 현재 디렉터리의 파일 |
| `../today.txt` | 부모 디렉터리의 파일 |

심볼릭 링크가 없는 구조에서 현재 위치가 `/home/mina`라면 `notes/today.txt`가, `/home/mina/notes`라면 `today.txt`가 같은 파일을 가리킨다. 현재 위치가 바뀌면 상대 경로의 기준도 바뀌지만 절대 경로의 출발점은 그대로다. [경로 해석 매뉴얼](https://man7.org/linux/man-pages/man7/path_resolution.7.html)

## 도움말 찾기

| 확인할 내용 | 명령 |
| --- | --- |
| 현재 작업 디렉터리 | `pwd` |
| Bash가 해석하는 명령의 종류 | `type cd` |
| Bash 내장 명령 사용법 | `help cd` |
| 섹션 1의 `ls` 매뉴얼 | `man 1 ls` |

매뉴얼의 섹션 1은 실행 프로그램과 셸 명령을 다룬다. 로컬 매뉴얼이 설치되지 않았다면 `man`이 문서를 찾지 못할 수 있으며, 그것만으로 명령도 없다고 판단할 수는 없다. [Bash 매뉴얼](https://man7.org/linux/man-pages/man1/bash.1.html), [man 매뉴얼](https://man7.org/linux/man-pages/man1/man.1.html)
