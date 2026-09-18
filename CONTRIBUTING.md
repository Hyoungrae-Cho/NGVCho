# 기여 가이드 / 브랜치 정책

AI를 활용한 바이브 코딩(vibe coding) + GitHub 워크플로우에서 일반적으로 쓰이는 정책을 따른다: 사람 리뷰 대신 자동화된 CI 게이트로 품질을 보증하고, `main`은 항상 배포 가능한 상태로 유지한다.

## 브랜치 전략

- `main`: 보호 브랜치. 항상 배포 가능한 상태를 유지하며, **직접 push를 금지**한다. 모든 변경은 PR을 통해서만 병합한다.
- 작업 브랜치 명명 규칙: `feature/<설명>`, `fix/<설명>`, `docs/<설명>`, `chore/<설명>` (예: `feature/architecture-review-flow`, `fix/tdd-skill-typo`)
- 작업 브랜치는 짧게 유지한다. 작업이 끝나면 PR을 열어 `main`에 병합하고, 병합 후 브랜치는 삭제한다.

## PR 규칙

- 모든 변경은 PR을 통해 `main`에 병합한다. `main`에 직접 커밋/push하지 않는다.
- PR을 열면 `.github/workflows/ci.yml`의 `CI / quality-gates` 체크가 자동 실행된다. 이 체크가 통과해야 병합할 수 있다.
- 병합 방식은 **Squash merge**를 권장한다(기능 단위로 커밋 이력을 깔끔하게 유지).
- 병합 전 PR 내 리뷰 대화(코멘트)는 모두 해결(resolve) 상태여야 한다.
- 사람 리뷰어의 승인은 필수가 아니다(0명) — AI 페어 코딩 중심의 1인/소규모 워크플로우이므로, 자동화된 CI 게이트가 사실상의 리뷰 역할을 한다. 팀 규모가 커지면 이 정책을 변경한다.

## `main` 브랜치 보호 규칙

GitHub 저장소의 Settings → Branches에서 `main`에 아래 규칙을 적용한다. 이 환경에는 GitHub CLI(`gh`)가 설치되어 있지 않아 자동으로 적용할 수 없었으므로, 저장소 관리자가 직접 웹 UI에서 설정하거나 아래 명령을 로컬에서 실행한다.

- Require a pull request before merging (필요 승인 인원: **0명**)
- Require status checks to pass before merging → `CI / quality-gates` 체크 선택
- Require branches to be up to date before merging
- Require conversation resolution before merging
- Do not allow bypassing the above settings (관리자 포함)
- Restrict deletions (`main` 삭제 금지)
- Do not allow force pushes

### `gh` CLI로 한 번에 적용하기

```bash
gh auth login   # 최초 1회, 저장소에 대한 admin 권한 필요
gh api -X PUT repos/Hyoungrae-Cho/NGVCho/branches/main/protection \
  -H "Accept: application/vnd.github+json" \
  -f required_status_checks.strict=true \
  -f 'required_status_checks.checks[][context]=CI / quality-gates' \
  -f enforce_admins=true \
  -f required_pull_request_reviews.required_approving_review_count=0 \
  -f restrictions=null \
  -f allow_force_pushes=false \
  -f allow_deletions=false \
  -f required_conversation_resolution=true
```

## CI가 검사하는 항목 (`CLAUDE.md` 구현/테스트 지침 근거)

`.github/workflows/ci.yml`은 PR과 `main` push 시 다음을 오픈소스 도구로 검사한다. Python 소스가 아직 없으면(현재 저장소 상태) 해당 게이트는 건너뛰고, `Coding` 서브에이전트가 소스를 추가하는 순간부터 자동으로 적용된다.

| 검사 | 도구 | 기준 |
|---|---|---|
| 정적 분석 | flake8 | 스타일/오류 검사 |
| 함수 순환복잡도 | radon/xenon | 10 이하 |
| 중복 코드 | jscpd | 8라인 이상 중복 금지 |
| 단위 테스트 | unittest + coverage | branch coverage 100%, 테스트 성공률 100% |

다음 항목은 자동화 스크립트의 신뢰도 문제로 아직 CI에는 포함하지 않았다. `Coding` 서브에이전트가 작성 시점에 점검하며(`implementation` 스킬 §2~§5), 필요하면 이후 CI에도 추가할 수 있다:

- Doxygen 주석 비율 20% 이상
- 함수/변수명 camelCase, 3글자 이상
