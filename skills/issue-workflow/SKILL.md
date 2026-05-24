# issue-workflow 스킬

## 트리거

다음 명령어 또는 요청이 오면 이 스킬을 사용한다:
- `/issue [타입] [제목 또는 설명]`
- "이슈 만들어줘", "GitHub 이슈 생성", "이슈 열어줘"

인자 없이 실행하면 현재 대화 컨텍스트에서 작업 내용을 자동으로 추론한다.

## 이슈 타입

| 타입 | 이슈 제목 prefix | 설명 | 템플릿 |
|------|-----------------|------|--------|
| `feat` | `[FEAT]` | 새로운 기능 구현 | feature_request |
| `fix` | `[FIX]` | 버그 수정 | bug_report |
| `refactor` | `[REFACTOR]` | 코드 리팩토링 | feature_request |
| `design` | `[DESIGN]` | UI/UX 수정 | feature_request |
| `docs` | `[DOCS]` | 문서 작업 | feature_request |
| `test` | `[TEST]` | 테스트 코드 작업 | feature_request |
| `chore` | `[CHORE]` | 설정/빌드 작업 | feature_request |
| `ci` | `[CI]` | CI/CD 작업 | feature_request |
| `perf` | `[PERF]` | 성능 개선 | feature_request |

## 이슈 템플릿

### Feature Request (feat / refactor / design / docs / test / chore / ci / perf)

```
### 🛠️ 만들고자 한 기능 설명

{작업 목적과 내용을 2-3문장으로 서술}

### ✅ TODO LIST

- [ ] {세부 작업 1}
- [ ] {세부 작업 2}
- [ ] {세부 작업 3}

### ⏰ 예상 작업 기간

{컨텍스트 기반 예상 기간, 모르면 "-" 로 표기}

### 📝 참고 링크(선택)

### 🗣️ ETC(선택)

### 📸 피그마 스크린샷
```

### Bug Report (fix)

```
## 어떤 버그인가요?

{버그를 간결하게 설명}

## 어떤 상황에서 발생한 버그인가요?

- **Given**: {사전 조건}
- **When**: {어떤 행동을 했을 때}
- **Then**: {어떤 문제가 발생했는지}

## 예상 결과

{정상적으로 동작했어야 할 결과}

## 참고자료

{관련 스크린샷, 에러 로그 등 — 없으면 생략}
```

## 브랜치 네이밍 컨벤션

```
{type}/{이슈번호}-{kebab-case-설명}
```

- `설명`: 이슈 제목을 **영어**로 의미 번역 후 kebab-case로 변환 (한국어 그대로 쓰지 않는다)
- 최대 50자를 넘지 않도록 축약

```
feat/5-user-login
fix/7-main-layout-broken
refactor/12-auth-hook-cleanup
```

## 워크플로우

### Phase 1 — 분석 및 계획 수립

1. 인자에서 타입과 설명 파싱. 없으면 현재 대화 컨텍스트에서 추론.
2. 타입에 맞는 템플릿 선택 후 내용 채우기.
3. `git config user.name`으로 현재 작성자 확인.
4. 계획표를 출력하고 **사용자 승인 대기**. 수정 요청 시 내용 조정.

계획표 출력 형식:
```
## 이슈 생성 계획

**타입**: feat
**제목**: [FEAT] 사용자 로그인 구현
**Assignee**: @me

### 이슈 본문 미리보기
---
(본문 내용)
---

**생성될 브랜치**: `feat/{번호}-user-login`
**Base 브랜치**: `develop`

계속 진행할까요?
```

### Phase 2 — 이슈 생성

5. 승인 후 `gh issue create` 실행:
```bash
gh issue create \
  --title "[TYPE] 제목" \
  --body "$(cat <<'EOF'
...
EOF
)" \
  --assignee "@me"
```
6. 출력된 이슈 URL에서 번호 파싱.

### Phase 3 — 브랜치 생성 및 체크아웃

7. `origin/develop`을 base로 브랜치 생성:
```bash
git checkout -b {type}/{번호}-{description} origin/develop
```
8. 결과 출력:
```
이슈 생성 완료: #{번호} — {제목}
   URL: https://github.com/.../issues/{번호}
브랜치 생성 완료: {type}/{번호}-{description}
   이제 작업을 시작할 수 있습니다.
```

## 주의사항

- assignee는 항상 `@me` (현재 인증된 gh CLI 사용자).
- `gh` CLI가 인증되어 있지 않으면 `gh auth login`을 먼저 실행하도록 안내한다.
- 같은 이름의 브랜치가 이미 있으면 사용자에게 알리고 다른 이름을 제안한다.
- `origin/develop`이 없는 프로젝트에서는 사용자에게 base 브랜치를 물어본다.

## 금지

- 사용자 승인 없이 이슈를 생성하지 않는다
- 브랜치 설명에 한국어를 그대로 쓰지 않는다 (반드시 영어 kebab-case로 변환)
- `gh` 인증 상태를 확인하지 않고 실행하지 않는다
