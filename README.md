# MultiAgent Plugin

[MultiAgent 데스크톱 앱](https://github.com/OneThingChanged/Multiagent)에서 돌리는
Claude Code · Codex 세션이 공통으로 쓰는 **관습/가이드 패키지**.

지금은 핵심 자산 하나(`skills/multiagent-conventions`)만 들어 있다 — 터미널이
출력 속 파일 경로를 **클릭 가능한 링크**로 만들 수 있게, 에이전트가 경로를
올바른 형태로 적도록 안내하는 스킬.

## 왜 한 repo로 양쪽을 다루나

Claude Code와 Codex는 **매니페스트 위치만 다르고**(`./.claude-plugin/` vs
`./.codex-plugin/`) **`skills/`(SKILL.md) 포맷은 동일**하다. 그래서 이 repo 루트
하나가 동시에 "Claude 플러그인 루트"이자 "Codex 플러그인 루트"로 동작한다.

```
MultiagentPlugin/
├─ skills/
│  └─ multiagent-conventions/SKILL.md   ← 공유 자산 (양쪽이 그대로 읽음)
├─ .claude-plugin/
│  ├─ plugin.json                       ← Claude 매니페스트
│  └─ marketplace.json                  ← Claude 마켓플레이스 카탈로그
├─ .codex-plugin/
│  └─ plugin.json                       ← Codex 매니페스트 (skills: "./skills/")
└─ README.md
```

> 업로드는 `git push` 한 번. 설치 명령만 도구별로 다르다.

## 설치

### Claude Code

```text
/plugin marketplace add OneThingChanged/MultiagentPlugin
/plugin install multiagent@multiagent
```

`/plugin` UI의 Installed 탭에서 관리·제거할 수 있다.

### Codex

스킬 단위로 GitHub에서 바로 설치(가장 확실한 경로):

```bash
python3 ~/.codex/skills/.system/skill-installer/scripts/install-skill-from-github.py \
  --repo OneThingChanged/MultiagentPlugin \
  --path skills/multiagent-conventions
```

- `$CODEX_HOME/skills/multiagent-conventions`로 복사된다(공개 repo는 직접
  다운로드, 비공개는 git sparse checkout 폴백).
- 설치 후 **Codex 재시작** 필요.

## 업데이트

설치는 "복사본"이라 자동 동기화가 아니다. repo를 갱신한 뒤:

- Claude: `/plugin` 에서 업데이트(또는 marketplace 갱신 후 재설치)
- Codex: 위 설치 명령을 다시 실행(기존 디렉터리는 먼저 제거)

## 범위 / 의도적으로 뺀 것

- **알림·resume hook은 넣지 않는다.** working/done·session-start hook은
  MultiAgent 앱이 프로젝트별 `.claude/settings.local.json` /
  `.codex/config.toml`에 자동 머지하므로 플러그인이 다룰 필요가 없다.
- 상시 강제 규칙(예: 경로 표기)은 스킬(on-demand 로드)만으로는 100% 보장되지
  않으므로, 사용자 글로벌 메모리(`~/.claude/CLAUDE.md`, `~/.codex/AGENTS.md`)와
  **병행**한다. 이 스킬은 더 자세한 참고용 + 향후 자산 확장의 그릇이다.

## 로드맵 (후보)

- SessionStart hook으로 상시 규칙 주입 (Claude `SessionStart` /
  Codex `session_start` — 이벤트명·스키마가 달라 도구별 hooks 정의 필요)
- MultiAgent UX 관습을 더 담은 추가 스킬
- 공용 헬퍼 스크립트(`bin/`)
