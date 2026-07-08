# Hermes Agent 초보자 실습 가이드

마지막 확인일: 2026-07-08

대상 독자: 개발을 거의 해본 적 없는 공과대학 학부 신입생
목표: Windows, macOS, Ubuntu 중 자기 환경에 맞는 부분을 그대로 따라 해서 Hermes Agent를 설치하고 첫 실습까지 해보기

이 문서는 "복사해서 붙여넣기만 해도 최대한 그대로 동작"하도록 작성했다. 다만 AI 모델을 실제로 호출하려면 Nous Portal 로그인, 또는 OpenRouter/OpenAI/Anthropic/Google 같은 모델 제공자의 계정이나 API 키가 필요하다. 설치 명령 자체는 복사해서 실행할 수 있지만, 모델 사용 단계에서는 브라우저 로그인이나 본인 API 키 입력이 필요할 수 있다.

## 0. Hermes Agent가 무엇인가

Hermes Agent는 Nous Research가 만든 오픈소스 AI 자율 에이전트다. 일반 챗봇처럼 답변만 하는 것이 아니라, 사용자가 허락한 범위 안에서 파일을 읽고, 터미널 명령을 실행하고, 웹 검색이나 브라우저 자동화 같은 도구를 사용하면서 일을 진행할 수 있다.

처음에는 이렇게 이해하면 된다.

| 단어 | 쉬운 설명 |
| --- | --- |
| 에이전트 | 사용자의 목표를 듣고 여러 단계를 스스로 계획해서 수행하는 AI |
| 터미널 | 컴퓨터에 글자로 명령을 입력하는 창 |
| CLI | 터미널에서 쓰는 프로그램 |
| 모델 | Hermes가 생각하고 답변할 때 사용하는 AI 두뇌 |
| Provider | OpenAI, Anthropic, OpenRouter, Nous Portal처럼 모델을 제공하는 서비스 |
| API 키 | 모델 제공자가 "이 사용자가 돈을 낼 수 있는 계정이다"라고 확인하는 비밀번호 같은 값 |
| Tool | 파일 읽기, 명령 실행, 웹 검색처럼 Hermes가 사용할 수 있는 기능 |
| Context | Hermes에게 미리 알려주는 프로젝트 설명이나 규칙 |

Hermes Agent를 안전하게 쓰는 기본 생각은 간단하다.

1. 처음에는 연습용 폴더에서만 쓴다.
2. 중요한 파일이 있는 폴더에서 바로 실험하지 않는다.
3. AI가 실행하려는 명령을 읽어보고 모르면 먼저 물어본다.
4. API 키나 비밀번호를 채팅창에 함부로 붙여넣지 않는다.
5. 삭제, 초기화, 강제 덮어쓰기 명령은 특히 조심한다.

## 1. 이 문서에서 쓰는 표기법

아래처럼 회색 박스 안에 있는 내용은 터미널에 붙여넣는 명령이다.

```powershell
hermes --version
```

`#`로 시작하는 줄은 설명 주석이다. 보통 같이 붙여넣어도 문제없지만, 초보자는 그대로 붙여넣어도 된다.

이 문서에는 세 가지 운영체제 섹션이 있다. 자기 컴퓨터에 맞는 섹션 하나만 따라 하면 된다.

| 내 컴퓨터 | 따라 할 섹션 |
| --- | --- |
| Windows 10 또는 Windows 11 | 4장 Windows |
| MacBook 또는 Mac mini | 5장 macOS |
| Ubuntu 데스크톱, Ubuntu 서버, WSL2 Ubuntu | 6장 Ubuntu |

Windows에서 WSL2 Ubuntu를 쓰는 경우는 Windows 섹션이 아니라 Ubuntu 섹션을 따라 한다.

## 2. 설치 전에 알아야 하는 것

Hermes Agent 설치는 크게 세 단계다.

1. Hermes 프로그램을 설치한다.
2. 사용할 AI 모델을 연결한다.
3. 터미널에서 첫 대화를 실행한다.

공식 문서 기준으로 가장 쉬운 모델 연결 방법은 `hermes setup --portal`이다. 이 명령은 Nous Portal 로그인 과정을 열고, 모델과 Tool Gateway 설정을 한 번에 연결한다.

Nous Portal을 쓰지 않을 수도 있다. 예를 들어 OpenRouter API 키가 있거나, OpenAI/Anthropic/Google 계정의 API 키를 직접 쓰고 싶다면 `hermes model` 명령으로 provider를 고르면 된다.

초보자에게 추천하는 순서는 다음과 같다.

1. 설치한다.
2. `hermes doctor`로 문제가 없는지 본다.
3. `hermes setup --portal`을 실행한다.
4. 브라우저가 열리면 로그인한다.
5. `hermes`를 실행해서 첫 대화를 한다.

## 3. 꼭 알아둘 안전 규칙

Hermes는 터미널 명령을 실행할 수 있다. 그래서 강력하지만, 동시에 조심해야 한다.

처음에는 아래 규칙을 지킨다.

1. 연습 폴더에서 시작한다.
2. `Downloads`, `Desktop`, `Documents` 전체를 마음대로 정리하라고 시키지 않는다.
3. `삭제해`, `전부 정리해`, `초기화해` 같은 요청은 초보 단계에서 피한다.
4. Hermes가 명령 실행 승인을 요청하면, 모르는 명령은 "이 명령이 무슨 뜻인지 먼저 설명해줘"라고 묻는다.
5. `--yolo` 같은 자동 승인 모드는 쓰지 않는다.

안전한 첫 프롬프트 예시는 다음과 같다.

```text
너는 지금부터 초보자를 도와주는 조교야.
명령을 실행하기 전에 그 명령이 무엇을 하는지 쉬운 말로 설명해줘.
파일을 삭제하거나 덮어쓰기 전에 반드시 나에게 먼저 확인해줘.
```

## 4. Windows 10/11에서 설치하고 사용하기

이 섹션은 Windows PowerShell을 사용한다. `cmd.exe`가 아니라 PowerShell 또는 Windows Terminal을 여는 것이 좋다.

### 4.1 PowerShell 열기

1. 키보드에서 Windows 키를 누른다.
2. `PowerShell`이라고 입력한다.
3. `Windows PowerShell` 또는 `Terminal`을 연다.
4. 관리자 권한으로 열 필요는 없다.

열린 창에 아래 명령을 붙여넣고 Enter를 누른다.

```powershell
$PSVersionTable.PSVersion
```

숫자가 출력되면 PowerShell이 정상이다.

### 4.2 Hermes Agent 설치

아래 한 줄을 PowerShell에 그대로 붙여넣고 Enter를 누른다.

```powershell
iex (irm https://hermes-agent.nousresearch.com/install.ps1)
```

기다린다. 설치 중에 여러 줄의 로그가 나온다. 중간에 질문이 나오면 기본값을 선택하거나 안내에 따라 진행한다.

설치가 끝나면 PowerShell 창을 닫고 새 PowerShell 창을 연다. 이 과정이 중요하다. 새 창을 열어야 Windows가 새로 등록된 `hermes` 명령을 알아본다.

### 4.3 설치 확인

새 PowerShell 창에서 아래 명령을 차례대로 붙여넣는다.

```powershell
Get-Command hermes
hermes --version
hermes doctor
```

성공하면 대략 이런 느낌의 결과가 나온다.

```text
CommandType     Name       Version    Source
-----------     ----       -------    ------
Application     hermes.exe ...

Hermes Agent ...
```

`hermes doctor`는 문제가 있으면 무엇이 부족한지 알려준다. 초보자는 메시지를 그대로 복사해서 Hermes Agent 이슈나 프로젝트 대화에 가져오면 된다.

### 4.4 모델 연결

가장 쉬운 방법은 Nous Portal 연결이다.

```powershell
hermes setup --portal
```

브라우저가 열리면 로그인한다. 모델 선택 화면이 나오면 처음에는 추천 모델이나 기본값을 선택한다.

Nous Portal을 쓰지 않고 다른 provider를 쓰고 싶다면 아래 명령을 쓴다.

```powershell
hermes model
```

방향키로 provider를 고르고 Enter를 누른다. API 키를 요구하면 본인 계정의 키를 넣는다. API 키는 채팅창이 아니라 설정 과정에 넣어야 한다.

### 4.5 첫 대화 실행

아래 명령을 실행한다.

```powershell
hermes
```

Hermes가 열리면 아래 문장을 그대로 붙여넣고 Enter를 누른다.

```text
안녕. 나는 개발을 거의 모르는 공과대학 신입생이야. 지금 내 컴퓨터에서 Hermes Agent가 정상 작동하는지 확인해줘. 먼저 현재 운영체제와 현재 폴더를 확인하고, 네가 사용할 수 있는 도구를 쉬운 말로 설명해줘.
```

대화가 정상적으로 이어지면 설치 성공이다.

종료하려면 `Ctrl+D`를 누르거나 아래처럼 입력한다.

```text
/exit
```

만약 `/exit`가 동작하지 않으면 `Ctrl+C`를 두 번 누르거나 창을 닫아도 된다.

### 4.6 연습용 폴더 만들기

중요한 파일을 건드리지 않도록 연습 폴더를 만든다. PowerShell에 아래 전체를 붙여넣는다.

```powershell
New-Item -ItemType Directory -Force "$env:USERPROFILE\hermes-practice" | Out-Null
Set-Location "$env:USERPROFILE\hermes-practice"
@'
# Hermes Practice Project

You are helping a first-year engineering student.
Always explain terminal commands before suggesting them.
Never delete files unless the user explicitly asks.
If you create or edit a file, summarize what changed afterward.
'@ | Set-Content -Encoding UTF8 AGENTS.md
Get-ChildItem
hermes
```

Hermes가 열리면 아래 문장을 붙여넣는다.

```text
이 폴더의 AGENTS.md를 읽고, beginner_notes.md 파일을 만들어줘.
내용은 "Hermes Agent란 무엇인가", "처음 쓸 때 조심할 점 3가지", "오늘 해볼 수 있는 연습 2가지"로 구성해줘.
파일을 만들기 전에 어떤 일을 할지 먼저 설명하고, 만든 뒤에는 파일 내용을 요약해줘.
```

성공하면 `beginner_notes.md` 파일이 만들어진다. 확인하려면 Hermes를 종료한 뒤 PowerShell에서 아래 명령을 실행한다.

```powershell
Get-Content .\beginner_notes.md
```

### 4.7 Windows에서 자주 막히는 문제

#### 문제: `hermes` 명령을 찾을 수 없다고 나온다

해결:

```powershell
# PowerShell 창을 닫고 새로 연 뒤 다시 실행
Get-Command hermes
```

그래도 안 되면 임시로 아래 명령을 실행한다.

```powershell
& "$env:LOCALAPPDATA\hermes\hermes-agent\venv\Scripts\hermes.exe" --version
```

#### 문제: 글자가 깨진다

Windows Terminal을 사용한다. 아주 오래된 `cmd.exe`에서는 유니코드가 깨질 수 있다.

확인 명령:

```powershell
Get-ChildItem env:HERMES_DISABLE_WINDOWS_UTF8
```

아무것도 나오지 않는 것이 보통 정상이다.

#### 문제: 브라우저 도구가 실패한다

아래 명령으로 진단한다.

```powershell
hermes doctor
```

Hermes가 Playwright Chromium 설치 명령을 알려주면 그 명령을 그대로 실행한다.

#### 문제: 완전히 삭제하고 다시 설치하고 싶다

먼저 일반 삭제:

```powershell
hermes uninstall
```

설정, 세션, 로그까지 모두 지우고 정말 처음부터 다시 시작하려면:

```powershell
hermes uninstall
Remove-Item -Recurse -Force "$env:LOCALAPPDATA\hermes"
Remove-Item -Recurse -Force "$env:USERPROFILE\.hermes"
```

위 명령은 Hermes 관련 데이터를 지운다. 다른 중요한 폴더에 붙여넣지 말고 그대로만 실행한다.

## 5. macOS에서 설치하고 사용하기

공식 지원의 중심은 Apple Silicon Mac이다. 즉 M1, M2, M3, M4 계열 Mac이면 가장 무난하다. Intel Mac은 공식 지원 매트릭스에서 제한이 있을 수 있다.

### 5.1 Terminal 열기

1. `Command + Space`를 누른다.
2. `Terminal`을 입력한다.
3. Terminal 앱을 연다.

아래 명령을 붙여넣고 Enter를 누른다.

```bash
uname -m
```

`arm64`가 나오면 Apple Silicon Mac이다.

### 5.2 Git 확인

아래 명령을 실행한다.

```bash
git --version
```

Git 버전이 출력되면 다음 단계로 간다.

만약 개발자 도구 설치 창이 뜨면 설치를 진행한다. 창이 뜨지 않고 오류가 나면 아래 명령을 실행한다.

```bash
xcode-select --install
```

설치가 끝난 뒤 Terminal을 새로 열고 다시 확인한다.

```bash
git --version
```

### 5.3 Hermes Agent 설치

아래 명령을 그대로 붙여넣는다.

```bash
curl -fsSL https://hermes-agent.nousresearch.com/install.sh | bash
```

설치가 끝나면 shell을 새로고침한다.

```bash
exec zsh -l
```

만약 zsh가 아니라 bash를 쓰고 있다면 아래를 실행한다.

```bash
exec bash -l
```

### 5.4 설치 확인

아래 명령을 실행한다.

```bash
which hermes
hermes --version
hermes doctor
```

`which hermes`는 보통 `~/.local/bin/hermes` 같은 경로를 출력한다.

### 5.5 모델 연결

가장 쉬운 방법:

```bash
hermes setup --portal
```

브라우저가 열리면 로그인한다. 모델 선택 화면에서는 처음에는 추천값이나 기본값을 고른다.

다른 provider를 쓰려면:

```bash
hermes model
```

### 5.6 첫 대화 실행

```bash
hermes
```

Hermes가 열리면 아래 문장을 붙여넣는다.

```text
안녕. 나는 개발을 거의 모르는 공과대학 신입생이야. 지금 macOS에서 Hermes Agent가 정상 작동하는지 확인해줘. 현재 폴더와 사용할 수 있는 도구를 확인하고, 내가 다음에 해볼 만한 쉬운 실습을 3개 추천해줘.
```

### 5.7 연습용 폴더 만들기

Terminal에 아래 전체를 붙여넣는다.

```bash
mkdir -p "$HOME/hermes-practice"
cd "$HOME/hermes-practice"
cat > AGENTS.md <<'EOF'
# Hermes Practice Project

You are helping a first-year engineering student.
Always explain terminal commands before suggesting them.
Never delete files unless the user explicitly asks.
If you create or edit a file, summarize what changed afterward.
EOF
ls -la
hermes
```

Hermes가 열리면 아래 문장을 붙여넣는다.

```text
이 폴더의 AGENTS.md를 읽고, beginner_notes.md 파일을 만들어줘.
내용은 "Hermes Agent란 무엇인가", "처음 쓸 때 조심할 점 3가지", "오늘 해볼 수 있는 연습 2가지"로 구성해줘.
파일을 만들기 전에 어떤 일을 할지 먼저 설명하고, 만든 뒤에는 파일 내용을 요약해줘.
```

Hermes를 종료한 뒤 파일을 확인한다.

```bash
cat beginner_notes.md
```

### 5.8 macOS에서 자주 막히는 문제

#### 문제: `curl: command not found`

macOS에서는 보통 curl이 기본으로 있다. 그래도 없다면 Command Line Tools 설치부터 다시 확인한다.

```bash
xcode-select --install
```

#### 문제: `hermes: command not found`

새 터미널을 열거나 아래를 실행한다.

```bash
exec zsh -l
```

그래도 안 되면 PATH를 확인한다.

```bash
echo "$PATH"
ls -la "$HOME/.local/bin"
```

#### 문제: 모델 연결이 안 된다

아래 순서대로 실행한다.

```bash
hermes doctor
hermes model
hermes setup --portal
```

#### 문제: 삭제하고 다시 설치하고 싶다

일반 삭제:

```bash
hermes uninstall
```

설정까지 모두 지우고 다시 시작:

```bash
hermes uninstall
rm -rf "$HOME/.hermes"
rm -f "$HOME/.local/bin/hermes"
```

`rm -rf`는 삭제 명령이다. 위 명령을 수정해서 다른 폴더에 쓰지 않는다.

## 6. Ubuntu에서 설치하고 사용하기

이 섹션은 Ubuntu Desktop, Ubuntu Server, Windows의 WSL2 Ubuntu에 모두 쓸 수 있다.

### 6.1 Terminal 열기

Ubuntu Desktop에서는 `Ctrl + Alt + T`를 누른다.
WSL2 Ubuntu에서는 Windows Terminal에서 Ubuntu 탭을 연다.

아래 명령을 실행해 Ubuntu 정보를 확인한다.

```bash
lsb_release -a
```

### 6.2 필요한 기본 패키지 설치

아래 명령을 그대로 붙여넣는다.

```bash
sudo apt update
sudo apt install -y git curl xz-utils build-essential
git --version
curl --version
```

`sudo` 비밀번호를 물어보면 Ubuntu 로그인 비밀번호를 입력한다. 입력해도 화면에 별표가 보이지 않는 것이 정상이다. 입력 후 Enter를 누른다.

### 6.3 Hermes Agent 설치

```bash
curl -fsSL https://hermes-agent.nousresearch.com/install.sh | bash
```

설치가 끝나면 shell을 새로고침한다.

```bash
source "$HOME/.bashrc"
```

만약 위 명령 후에도 `hermes`를 못 찾으면 터미널을 닫고 새로 연다.

### 6.4 설치 확인

```bash
which hermes
hermes --version
hermes doctor
```

### 6.5 모델 연결

추천:

```bash
hermes setup --portal
```

다른 provider:

```bash
hermes model
```

### 6.6 첫 대화 실행

```bash
hermes
```

Hermes가 열리면 아래 문장을 붙여넣는다.

```text
안녕. 나는 개발을 거의 모르는 공과대학 신입생이야. 지금 Ubuntu에서 Hermes Agent가 정상 작동하는지 확인해줘. 현재 폴더와 운영체제를 확인하고, 내가 안전하게 연습할 수 있는 첫 작업을 추천해줘.
```

### 6.7 연습용 폴더 만들기

아래 전체를 붙여넣는다.

```bash
mkdir -p "$HOME/hermes-practice"
cd "$HOME/hermes-practice"
cat > AGENTS.md <<'EOF'
# Hermes Practice Project

You are helping a first-year engineering student.
Always explain terminal commands before suggesting them.
Never delete files unless the user explicitly asks.
If you create or edit a file, summarize what changed afterward.
EOF
ls -la
hermes
```

Hermes가 열리면 아래 문장을 붙여넣는다.

```text
이 폴더의 AGENTS.md를 읽고, beginner_notes.md 파일을 만들어줘.
내용은 "Hermes Agent란 무엇인가", "처음 쓸 때 조심할 점 3가지", "오늘 해볼 수 있는 연습 2가지"로 구성해줘.
파일을 만들기 전에 어떤 일을 할지 먼저 설명하고, 만든 뒤에는 파일 내용을 요약해줘.
```

Hermes 종료 후 확인:

```bash
cat beginner_notes.md
```

### 6.8 Ubuntu에서 자주 막히는 문제

#### 문제: `sudo` 비밀번호가 입력되지 않는 것처럼 보인다

정상이다. Linux 터미널은 비밀번호 입력 중 아무 글자도 표시하지 않는다. 그냥 비밀번호를 입력하고 Enter를 누른다.

#### 문제: `curl: command not found`

```bash
sudo apt update
sudo apt install -y curl
```

#### 문제: `xz` 관련 오류가 난다

```bash
sudo apt update
sudo apt install -y xz-utils
```

#### 문제: Desktop 앱이나 브라우저 자동화 관련 오류가 난다

아래 명령을 실행한다.

```bash
sudo apt update
sudo apt install -y build-essential
hermes doctor
```

#### 문제: 삭제하고 다시 설치하고 싶다

일반 삭제:

```bash
hermes uninstall
```

완전 삭제:

```bash
hermes uninstall
rm -rf "$HOME/.hermes"
rm -f "$HOME/.local/bin/hermes"
```

`rm -rf`는 강력한 삭제 명령이다. 이 줄을 수정해서 다른 폴더에 쓰지 않는다.

## 7. 모든 OS 공통: Hermes 기본 사용법

이 장은 Windows, macOS, Ubuntu 모두에 해당한다. 명령은 각 OS 터미널에 그대로 입력하면 된다.

### 7.1 대화 시작

```bash
hermes
```

Windows PowerShell에서도 같은 명령을 쓴다.

```powershell
hermes
```

### 7.2 더 현대적인 TUI로 시작

TUI는 터미널 안에서 조금 더 앱처럼 보이는 화면이다.

```bash
hermes --tui
```

Windows:

```powershell
hermes --tui
```

### 7.3 한 번만 질문하고 끝내기

짧은 확인에는 `chat -q`가 편하다.

macOS/Ubuntu:

```bash
hermes chat -q "현재 폴더에 어떤 파일이 있는지 설명해줘."
```

Windows:

```powershell
hermes chat -q "현재 폴더에 어떤 파일이 있는지 설명해줘."
```

### 7.4 이전 대화 이어서 하기

```bash
hermes --continue
```

짧은 형태:

```bash
hermes -c
```

Windows PowerShell에서도 똑같이 쓴다.

```powershell
hermes --continue
```

### 7.5 모델 바꾸기

대화 밖에서 바꾸기:

```bash
hermes model
```

대화 안에서 바꾸기:

```text
/model
```

특정 모델명을 알고 있을 때:

```text
/model anthropic/claude-sonnet-4.6
```

사용 가능한 모델명은 provider와 구독 상태에 따라 달라질 수 있다. 처음에는 `hermes model`의 선택 화면을 쓰는 것이 안전하다.

### 7.6 도구 설정 보기

```bash
hermes tools
```

대화 안에서는:

```text
/tools
```

초보자는 처음에 너무 많은 도구를 켜기보다, 기본값으로 첫 대화가 되는지 확인한 뒤 필요한 것만 켠다.

### 7.7 진단하기

문제가 있으면 제일 먼저:

```bash
hermes doctor
```

그다음:

```bash
hermes model
hermes setup
hermes --continue
```

### 7.8 업데이트

```bash
hermes update
```

기존 설치를 최신 버전으로 업데이트한다.

### 7.9 기본 slash command

Hermes 대화창 안에서 `/`를 입력하면 명령 목록이 뜬다.

자주 쓰는 명령:

| 명령 | 뜻 |
| --- | --- |
| `/help` | 도움말 보기 |
| `/tools` | 사용 가능한 도구 보기 |
| `/model` | 모델 변경 |
| `/save` | 대화 저장 |
| `/usage` | 사용량 보기 |

### 7.10 줄바꿈 입력

긴 요청을 여러 줄로 쓰고 싶을 때:

| OS | 추천 키 |
| --- | --- |
| Windows Terminal | `Ctrl+Enter` 또는 `Ctrl+J` |
| macOS Terminal | `Option+Enter`가 안 되면 `Ctrl+J` |
| Ubuntu Terminal | `Alt+Enter` 또는 `Ctrl+J` |

키가 안 먹으면 한 줄로 써도 된다.

### 7.11 중간에 멈추기

Hermes가 너무 오래 생각하거나 원하지 않는 방향으로 가면 `Ctrl+C`를 누른다. 그다음 이렇게 말한다.

```text
방금 작업은 멈춰줘. 지금까지 한 일을 요약하고, 다음에는 파일을 수정하기 전에 나에게 먼저 물어봐줘.
```

## 8. 첫 실습 3개

아래 실습은 초보자가 Hermes를 안전하게 익히기 좋다.

### 실습 1: 폴더 설명시키기

연습 폴더에서 Hermes를 실행한다.

Windows:

```powershell
Set-Location "$env:USERPROFILE\hermes-practice"
hermes
```

macOS/Ubuntu:

```bash
cd "$HOME/hermes-practice"
hermes
```

Hermes에 붙여넣기:

```text
현재 폴더에 어떤 파일이 있는지 확인하고, 각 파일이 무슨 역할을 하는지 초보자에게 설명하듯이 알려줘. 파일을 수정하지는 마.
```

### 실습 2: 간단한 Python 파일 만들기

Hermes에 붙여넣기:

```text
hello.py라는 아주 간단한 Python 파일을 만들어줘.
실행하면 "Hello from Hermes practice"라고 출력되게 해줘.
파일을 만들기 전에 어떤 내용을 쓸지 먼저 보여주고, 만든 뒤에는 실행해서 결과를 확인해줘.
```

Hermes가 명령 실행을 요청하면 내용을 읽고 승인한다.

종료 후 직접 확인:

Windows:

```powershell
python .\hello.py
```

macOS/Ubuntu:

```bash
python3 hello.py
```

Python이 설치되어 있지 않으면 Hermes에게 이렇게 묻는다.

```text
내 컴퓨터에 Python이 설치되어 있는지 확인하고, 없으면 설치 방법을 운영체제에 맞게 알려줘. 바로 설치 명령을 실행하지 말고 먼저 설명해줘.
```

### 실습 3: 수업 노트 요약시키기

연습용 텍스트 파일을 만든다.

Windows:

```powershell
Set-Location "$env:USERPROFILE\hermes-practice"
@'
오늘 배운 내용:
1. AI 에이전트는 목표를 여러 단계로 나누어 해결한다.
2. 터미널 명령은 컴퓨터에게 직접 지시하는 문장이다.
3. 안전하게 쓰려면 삭제 명령과 비밀번호 노출을 조심해야 한다.
'@ | Set-Content -Encoding UTF8 lecture_note.txt
hermes
```

macOS/Ubuntu:

```bash
cd "$HOME/hermes-practice"
cat > lecture_note.txt <<'EOF'
오늘 배운 내용:
1. AI 에이전트는 목표를 여러 단계로 나누어 해결한다.
2. 터미널 명령은 컴퓨터에게 직접 지시하는 문장이다.
3. 안전하게 쓰려면 삭제 명령과 비밀번호 노출을 조심해야 한다.
EOF
hermes
```

Hermes에 붙여넣기:

```text
lecture_note.txt를 읽고, 1학년 학생이 시험 전에 볼 수 있는 요약본 study_summary.md를 만들어줘.
요약본에는 핵심 개념, 쉬운 비유, 확인 문제 3개를 넣어줘.
```

## 9. 프로젝트 폴더에서 Hermes를 잘 쓰는 방법

Hermes는 프로젝트 안의 안내 파일을 읽고 그 규칙을 따를 수 있다. 대표적으로 `AGENTS.md`가 있다.

예를 들어 어떤 프로젝트 폴더에 아래 파일을 만들어두면 Hermes가 시작할 때 참고한다.

```markdown
# Agent Instructions

이 프로젝트에서는 초보자를 돕는 방식으로 설명한다.
파일을 삭제하기 전에 반드시 사용자에게 확인한다.
큰 변경을 하기 전에 먼저 계획을 말한다.
```

연습 폴더에서는 이미 `AGENTS.md`를 만들었다. 그래서 Hermes에게 이렇게 요청할 수 있다.

```text
이 프로젝트의 AGENTS.md 규칙을 따르면서 작업해줘.
먼저 현재 폴더 구조를 설명하고, 그 다음 내가 할 수 있는 안전한 실습을 제안해줘.
```

중요한 점:

1. Hermes를 프로젝트 폴더 안에서 실행해야 그 프로젝트의 규칙을 잘 읽는다.
2. `AGENTS.md`는 프로젝트마다 다르게 만들 수 있다.
3. 규칙이 길어질수록 중요한 내용을 위쪽에 둔다.

## 10. 모델과 비용을 이해하기

Hermes Agent 자체는 오픈소스지만, AI 모델을 호출하는 비용은 provider에 따라 발생할 수 있다.

초보자가 기억할 것:

1. 설치는 무료일 수 있지만, 모델 사용은 유료일 수 있다.
2. Nous Portal, OpenRouter, OpenAI, Anthropic, Google 등은 각각 과금 방식이 다르다.
3. 긴 파일을 많이 읽히거나, 긴 대화를 오래 이어가면 비용이 늘 수 있다.
4. `/usage` 명령으로 사용량을 확인한다.
5. 처음에는 작은 실습만 한다.

대화 안에서:

```text
/usage
```

비용이 걱정되면 Hermes에게 이렇게 말한다.

```text
앞으로 비용이 많이 들 수 있는 작업을 하기 전에는 먼저 예상 이유를 설명하고 나에게 확인을 받아줘.
```

## 11. API 키와 비밀정보 다루기

API 키는 비밀번호처럼 다룬다.

하지 말아야 할 것:

1. API 키를 GitHub 공개 저장소에 올리기
2. API 키를 친구에게 보내기
3. API 키를 Hermes 대화창에 아무 설명 없이 붙여넣기
4. `.env` 파일을 공개 repo에 커밋하기

Hermes 공식 설정 흐름을 쓰면 보통 비밀값은 설정 파일이나 `.env`에 저장된다. 직접 파일을 만져야 한다면 먼저 Hermes에게 이렇게 묻는다.

```text
API 키를 안전하게 설정하고 싶어. 내 운영체제 기준으로 Hermes가 권장하는 방법을 알려줘. 키 값을 채팅에 직접 쓰지 않는 방식으로 안내해줘.
```

## 12. 더 안전하게 쓰고 싶을 때

Hermes는 터미널 backend를 바꿀 수 있다. 초보자에게 중요한 개념만 말하면:

| backend | 뜻 | 초보자 추천 |
| --- | --- | --- |
| local | 내 컴퓨터에서 바로 명령 실행 | 연습 폴더에서만 사용 |
| docker | Docker 컨테이너 안에서 격리 실행 | Docker를 배운 뒤 추천 |
| ssh | 원격 서버에서 실행 | 서버를 아는 사람에게 추천 |

Docker가 이미 설치되어 있고 격리 실행을 해보고 싶다면:

```bash
hermes config set terminal.backend docker
```

Windows PowerShell에서도 같은 명령이다.

```powershell
hermes config set terminal.backend docker
```

Docker를 모르면 이 단계는 건너뛴다.

## 13. 문제 해결 순서

뭔가 안 될 때는 감으로 이것저것 바꾸지 말고 아래 순서대로 한다.

```bash
hermes doctor
hermes model
hermes setup
hermes --continue
```

Windows에서도 같은 순서다.

```powershell
hermes doctor
hermes model
hermes setup
hermes --continue
```

그래도 안 되면 아래 정보를 모아 질문한다.

Windows:

```powershell
hermes --version
hermes doctor
Get-Command hermes
$PSVersionTable.PSVersion
```

macOS:

```bash
hermes --version
hermes doctor
which hermes
uname -a
```

Ubuntu:

```bash
hermes --version
hermes doctor
which hermes
lsb_release -a
```

질문 예시:

```text
Hermes Agent 설치 중 문제가 생겼어.
운영체제는 Windows 11이야.
아래는 hermes doctor 결과야.

[여기에 결과 붙여넣기]

초보자도 이해할 수 있게 원인과 해결 순서를 알려줘.
```

## 14. 자주 쓰는 명령 요약표

| 하고 싶은 일 | 명령 |
| --- | --- |
| Hermes 시작 | `hermes` |
| TUI로 시작 | `hermes --tui` |
| 한 번만 질문 | `hermes chat -q "질문"` |
| 이전 대화 이어가기 | `hermes --continue` |
| 모델 설정 | `hermes model` |
| 빠른 Portal 설정 | `hermes setup --portal` |
| 전체 설정 마법사 | `hermes setup` |
| 도구 설정 | `hermes tools` |
| 문제 진단 | `hermes doctor` |
| 업데이트 | `hermes update` |
| 삭제 | `hermes uninstall` |

## 15. 처음 일주일 추천 학습 계획

### 1일차

설치하고 첫 대화를 한다.

```text
내가 Hermes Agent로 무엇을 할 수 있는지 초보자 눈높이로 알려줘.
```

### 2일차

연습 폴더에서 파일 하나를 만들게 한다.

```text
오늘 배운 내용을 정리할 daily_note.md 파일을 만들어줘. 제목, 오늘 한 일, 어려웠던 점, 내일 할 일을 포함해줘.
```

### 3일차

파일을 읽고 요약하게 한다.

```text
daily_note.md를 읽고, 내가 이해하지 못한 개념이 무엇인지 추측해서 질문 3개를 만들어줘.
```

### 4일차

작은 Python 예제를 만든다.

```text
초보자용 Python 계산기 예제를 만들어줘. 덧셈 함수 하나만 만들고, 실행 결과를 확인해줘.
```

### 5일차

명령 설명을 시킨다.

```text
ls, cd, pwd 명령이 무엇인지 Windows와 Linux/macOS 차이까지 포함해서 쉽게 설명해줘.
```

### 6일차

프로젝트 규칙을 다듬는다.

```text
AGENTS.md를 더 안전한 초보자용 규칙으로 개선해줘. 삭제 금지, 명령 설명, 작업 후 요약 규칙을 넣어줘.
```

### 7일차

한 주를 정리한다.

```text
이번 주에 만든 파일들을 확인하고, 내가 Hermes Agent 사용법에서 익힌 내용을 learning_log.md로 정리해줘.
```

## 16. 공식 자료와 이 문서의 기준

이 문서는 아래 공식 자료를 기준으로 작성했다.

- Hermes Agent 공식 문서: https://hermes-agent.nousresearch.com/docs/
- 설치 가이드: https://hermes-agent.nousresearch.com/docs/getting-started/installation
- Quickstart: https://hermes-agent.nousresearch.com/docs/getting-started/quickstart
- Platform Support: https://hermes-agent.nousresearch.com/docs/getting-started/platform-support
- Windows Native Guide: https://hermes-agent.nousresearch.com/docs/user-guide/windows-native
- CLI Interface: https://hermes-agent.nousresearch.com/docs/user-guide/cli
- Tools & Toolsets: https://hermes-agent.nousresearch.com/docs/user-guide/features/tools
- Context Files: https://hermes-agent.nousresearch.com/docs/user-guide/features/context-files
- Security: https://hermes-agent.nousresearch.com/docs/user-guide/security
- GitHub 저장소: https://github.com/NousResearch/hermes-agent
- 최신 릴리스 확인 기준: Hermes Agent v0.18.2, tag `v2026.7.7.2`, published 2026-07-08 UTC

Hermes Agent는 활발하게 바뀌는 프로젝트다. 설치 명령이나 모델 이름이 달라질 수 있으므로, 문서의 명령이 실패하면 먼저 공식 Installation 문서와 GitHub Releases를 확인한다.
