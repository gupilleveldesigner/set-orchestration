# set-orchestration

Claude Code 스킬입니다. 지금 쓰고 있는 세션을 **오케스트레이션 세션**으로 바꿉니다. 그 뒤로는 모든 작업을 하위 세션에 맡기고, 이 세션은 일을 나눠 주고, 상태를 확인하고, 결과물을 검증하고, 보고하는 일만 합니다.

- 할 일은 하위 세션을 새로 띄워서 맡깁니다.
- 이어지는 일이나 후속 작업은 맥락을 이미 가진 하위 세션에 다시 맡깁니다.
- 결과물을 확인했고 더 맡길 일이 없는 세션은 보관(archive)해서 메모리를 비웁니다.
- 하위 세션은 기본적으로 메인 세션과 같은 모델로 띄웁니다. 범위가 좁고 검증하기 쉬운 일만 더 저렴한 모델에 맡기고, 상태 표에 어떤 모델을 썼는지 표시합니다.
- 하위 세션이 "완료"라고 보고해도 실제 파일을 읽어 본 뒤에 사용자에게 보고합니다.

## 요구 환경

- **Claude 데스크탑 앱(Windows/macOS)의 Code 탭**과 **클라우드 세션(claude.ai/code)**에서 동작합니다. 하위 세션을 만들고, 이어서 지시하고, 보관하는 도구가 이 두 환경에 있습니다.
- 터미널 CLI(`claude`)와 VS Code·JetBrains 확장에는 그 도구가 없어서 쓸 수 없습니다. 이런 환경에서 부르면 쓸 수 없다고 알리고 끝납니다.
- 클라우드에서는 하위 세션이 별도 컨테이너에서 돌기 때문에, 결과물은 작업 브랜치에 푸시된 것을 가져와 확인합니다. 보관하면 컨테이너가 반납되므로 푸시하지 않은 작업은 남지 않습니다.
- 사용량은 작업 규모에 따라 다릅니다.
  - 작은 일이 많을 때는 세션 하나로 할 때보다 더 듭니다. 새 세션마다 시스템 프롬프트와 도구 정의를 다시 읽고, 검증하느라 같은 파일을 여러 번 읽기 때문입니다.
  - 길고 큰 작업은 세션마다 맥락이 작게 유지되므로 비슷하거나 덜 들 수 있습니다.
  - 여러 세션이 동시에 돌면 시간당 쓰는 양이 늘어 사용 한도에 더 빨리 닿습니다.

## 설치

개인 스킬 폴더(`~/.claude/skills/`)에 이 저장소를 그대로 받으면 설치가 끝납니다. 클라우드 세션에서 쓰려면 작업하는 저장소의 `.claude/skills/set-orchestration/`에 넣어 커밋해 둡니다. 클라우드 컨테이너는 내 컴퓨터의 `~/.claude`를 보지 못합니다.

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

세션에서 이렇게 입력합니다. 맡길 일을 같이 적으면 되묻지 않고 바로 위임합니다.

```
/set-orchestration
/set-orchestration 로그인 버그 원인 찾아서 고쳐줘
```

"오케스트레이션 모드로 해줘"처럼 이 모드를 직접 지목해서 말해도 켜집니다. 그냥 "나눠서 해줘", "병렬로 돌려줘"라고만 하면 켜지지 않습니다.

켜진 뒤에는 짧게 말하면 됩니다.

| 말 | 하는 일 |
|---|---|
| "상태" | 하위 세션의 실제 상태를 다시 읽어 표로 보여줍니다 |
| "정리" | 결과물을 확인했고 더 맡길 일이 없는 세션을 보관합니다 |
| "○○한테 이어서 …" | 맥락을 가진 기존 세션에 후속 작업을 보냅니다 |
| "오케스트레이션 해제", "직접 해줘" | 모드를 끕니다 |

## 업데이트·삭제

```bash
git -C ~/.claude/skills/set-orchestration pull
```

삭제하려면 `~/.claude/skills/set-orchestration` 폴더를 지우면 됩니다.

## 라이선스

MIT
