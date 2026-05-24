# minna-agent-skils

내가 자주 쓰는 AI 에이전트용 스킬 카탈로그.

특정 에이전트 전용 저장소가 아니라, 자주 하는 작업 방식을 SKILL.md로 정리해두고
각 프로젝트의 instruction 파일(CLAUDE.md / AGENTS.md)에서 필요한 것만 연결해 쓴다.

## 현재 스킬

| 스킬 | 설명 | 트리거 |
|------|------|--------|
| [atomic-commit](skills/atomic-commit/SKILL.md) | secret guard + atomic 단위 커밋 작성 | "커밋해줘", "commit" |
| [pull-request](skills/pull-request/SKILL.md) | WHY 중심 PR 본문 작성 | "PR 만들어줘", "pull request" |
| [code-review](skills/code-review/SKILL.md) | findings-first 심각도 기반 코드 리뷰 | "코드 리뷰해줘", "review" |
| [doc-writer](skills/doc-writer/SKILL.md) | README/설계문서/ADR 작성 | "문서 만들어줘", "README" |

## 프로젝트에 연결하는 법

### Claude Code (CLAUDE.md)

```markdown
## Project Skills

내 스킬 레포 경로: /path/to/minna-agent-skils

- 커밋 요청 → skills/atomic-commit/SKILL.md 지시 따름
- PR 작성 요청 → skills/pull-request/SKILL.md 지시 따름
- 코드 리뷰 요청 → skills/code-review/SKILL.md 지시 따름
- 문서 작성 요청 → skills/doc-writer/SKILL.md 지시 따름
```

### Codex (AGENTS.md)

```markdown
## Project Skills

- Use $atomic-commit at <SKILLS_ROOT>/skills/atomic-commit/SKILL.md when asked to commit changes.
- Use $pull-request at <SKILLS_ROOT>/skills/pull-request/SKILL.md when asked to create a PR.
- Use $code-review at <SKILLS_ROOT>/skills/code-review/SKILL.md when asked to review code.
- Use $doc-writer at <SKILLS_ROOT>/skills/doc-writer/SKILL.md when asked to write documentation.
```

`<SKILLS_ROOT>`를 이 레포를 clone한 실제 경로로 바꾼다.

snippets/ 폴더에 각 스킬별 연결 조각이 있으니 복사해서 쓰면 된다.

## 구조

```
minna-agent-skils/
├── skills/
│   ├── atomic-commit/SKILL.md
│   ├── pull-request/SKILL.md
│   ├── code-review/SKILL.md
│   └── doc-writer/SKILL.md
└── snippets/           ← CLAUDE.md / AGENTS.md에 붙여 쓸 연결 조각
    ├── atomic-commit.md
    ├── pull-request.md
    ├── code-review.md
    └── doc-writer.md
```

## 스킬 추가하는 법

1. `skills/<새-스킬-이름>/SKILL.md` 파일 생성
2. 트리거, 워크플로우, 금지 사항을 한국어로 작성
3. `snippets/<새-스킬-이름>.md`에 프로젝트 연결 조각 추가
4. 이 README 표에 한 줄 추가
