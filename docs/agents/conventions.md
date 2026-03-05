# Conventions (GitFlow Agentic)

## Path rule
- 모든 파일 경로는 반드시 `repo/`로 시작한다.
- 절대경로(/opt/...) 사용 금지.
- 변경 대상 파일은 `changes/CR-*.md`에 명시된 목록만 수정한다.

## One change = one CR
- 모든 작업은 `changes/CR-YYYYMMDD-###-slug.md` 1개를 기준으로 진행한다.
- CR 문서에 "Done Criteria"가 없으면 작업을 시작하지 않는다.

## Branch & commit
- 기본: feature branch 사용 (ops만 수행)
  - `feat/<slug>` 또는 `fix/<slug>`
- 커밋 메시지: Conventional Commits 권장
  - `feat: ...`, `fix: ...`, `docs: ...`, `refactor: ...`

## Tests
- 테스트/실행은 ops가 담당한다.
- planner는 Test Plan을 정의하고, coder는 필요한 테스트 수정/추가를 구현한다.
