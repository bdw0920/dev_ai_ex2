Agentic GitFlow 문서 구조:
1) docs/agents/conventions.md: 공통 규칙
2) docs/agents/git_planner.md: 기획 역할 규칙
3) docs/agents/git_coder.md: 구현 역할 규칙
4) docs/agents/git_reviewer.md: 리뷰 역할 규칙
5) docs/agents/git_ops.md: 운영/배포 역할 규칙
6) changes/CR-*.md: 작업 단위(Change Request)
7) CR은 계획→구현→리뷰→ops 로그를 한 문서에 기록
8) 파일 변경은 CR의 Files to change 범위만 허용
9) ops는 APPROVE 없으면 commit/push 금지
10) push는 feature branch 기준으로 진행
