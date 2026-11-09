# set-orchestration

Claude Code 스킬입니다. 지금 쓰고 있는 세션을 **오케스트레이션 세션**으로 바꿉니다. 그 뒤로는 모든 작업을 하위 세션에 맡기고, 이 세션은 일을 나눠 주고, 상태를 확인하고, 결과물을 검증하고, 보고하는 일만 합니다.

- 할 일은 하위 세션을 새로 띄워서 맡깁니다.
- 이어지는 일이나 후속 작업은 맥락을 이미 가진 하위 세션에 다시 맡깁니다.
- 결과물을 확인했고 더 맡길 일이 없는 세션은 보관(archive)해서 메모리를 비웁니다.
- 하위 세션이 "완료"라고 보고해도 실제 파일을 읽어 본 뒤에 사용자에게 보고합니다.

## 요구 환경

- **Claude 데스크탑 앱(Windows/macOS)의 Code 탭**에서만 동작합니다.
- 터미널 CLI(`claude`), VS Code·JetBrains 확장, claude.ai 웹에서는 하위 세션을 관리하는 도구가 없어서 쓸 수 없습니다. 이런 환경에서 부르면 쓸 수 없다고 알리고 끝납니다.
- 모든 작업을 하위 세션으로 띄우기 때문에 세션 하나로 일할 때보다 사용량이 많이 듭니다.

## 설치

개인 스킬 폴더(`~/.claude/skills/`)에 이 저장소를 그대로 받으면 설치가 끝납니다.

**macOS / Linux / Git Bash**

```bash
git clone https://github.com/gupilleveldesigner/set-orchestration.git ~/.claude/skills/set-orchestration
```

**Windows PowerShell**

```powershell
git clone https://github.com/gupilleveldesigner/set-orchestration.git "$HOME\.claude\skills\set-orchestration"
```

**git이 없을 때**

1. 이 페이지 위쪽의 **Code → Download ZIP**을 눌러 받습니다.
2. 압축을 풀고 폴더 이름을 `set-orchestration-main`에서 `set-orchestration`으로 바꿉니다.
3. 그 폴더를 `~/.claude/skills/` 안에 넣습니다. Windows라면 `C:\Users\<사용자이름>\.claude\skills\`입니다. 이 폴더가 없으면 새로 만듭니다.

설치가 끝나면 `~/.claude/skills/set-orchestration/SKILL.md` 파일이 있어야 합니다. 새 세션을 열면 스킬이 목록에 나타납니다.

## 사용법

데스크탑 앱 Code 탭의 세션에서 이렇게 입력합니다.

```
/set-orchestration
```

"오케스트레이션 모드로 해줘"처럼 이 모드를 직접 지목해서 말해도 켜집니다. 그냥 "나눠서 해줘", "병렬로 돌려줘"라고만 하면 켜지지 않습니다.

끌 때는 "오케스트레이션 해제" 또는 "직접 해줘"라고 말하면 됩니다.

## 업데이트·삭제

```bash
git -C ~/.claude/skills/set-orchestration pull
```

삭제하려면 `~/.claude/skills/set-orchestration` 폴더를 지우면 됩니다.

## 라이선스

MIT
