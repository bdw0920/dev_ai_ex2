# PROTOCOL.md

이 문서는 사용자가 요청하는 다양한 서비스 개발 작업을 처리하기 위한
범용 OpenClaw multi-agent 표준 프로토콜이다.

모든 agent는 이 문서를 source of truth 로 간주한다.

## 1. 목적
이 프로토콜의 목적은 다음과 같다.

- 사용자의 요구사항을 작은 작업 단위로 분해한다
- 특정 도메인에 종속되지 않는 개발 절차를 표준화한다
- 역할별 agent 가 자기 책임만 수행하게 한다
- 단계별 산출물을 파일로 남긴다
- 실패 시 다음 단계로 진행하지 않는다
- 과도한 개발을 방지한다
- 기존 기능을 유지하면서 점진적으로 기능을 확장한다

## 2. 적용 범위
이 프로토콜은 특정 서비스 도메인에 한정되지 않는다.

예:
- NMS
- 웹 서비스
- 백엔드 API
- 내부 도구
- 데이터 처리 서비스
- 자동화 서비스
- 관리 콘솔
- 단일 기능 유틸리티

서비스의 도메인 요구사항은 task 별 입력으로 주어지고,
이 프로토콜은 그 요구사항을 처리하는 절차만 정의한다.

## 3. 기본 원칙
1. 모든 작업은 task 단위로 처리한다
2. 한 번에 하나의 task만 활성 상태로 처리한다
3. 각 단계의 성공 기준은 "좋은 답변"이 아니라 "지정 파일의 실제 생성 또는 갱신"이다
4. 채팅 응답보다 파일 산출물이 우선이다
5. out_of_scope 는 절대 구현하지 않는다
6. 실패 시 즉시 중단하고 다음 단계로 가지 않는다
7. 기존 기능을 깨뜨리지 않는다
8. 미래 대비 과도한 추상화나 구조화는 기본적으로 금지한다
9. 사용자의 요청 범위를 벗어나는 도메인 확장은 금지한다

## 4. 표준 단계
표준 순서는 아래와 같다.

1. planner
2. coder
3. reviewer
4. tester
5. gitops

## 5. 단계별 역할

### planner
- 사용자 요구사항을 작은 task 하나로 분해한다
- 목표, 범위, 제외 범위, 완료 조건, 테스트 요구사항을 정리한다
- handoff_to 를 coder 로 지정한다
- plan 문서를 생성한다

### coder
- planner 문서의 in_scope 만 구현한다
- 실제 코드 파일, 설정 파일, 테스트 파일을 생성 또는 수정한다
- impl 문서를 생성한다

### reviewer
- coder 결과를 검토한다
- 먼저 범위 초과 여부를 확인한다
- 그 다음 요구사항 일치 여부, 구조 적절성, 테스트 충분성을 검토한다
- decision 을 명시한다
- 코드 수정은 하지 않는다

### tester
- 검증만 수행한다
- 실행 명령, 기대 결과, 실제 결과, pass/fail, 재현 절차를 남긴다
- 코드를 수정하지 않는다

### gitops
- release note 성격의 문서를 작성한다
- git status 결과를 기록한다
- 별도 지시가 없는 한 commit/push/deploy 는 수행하지 않는다

## 6. 파일 경로 규칙

### 계획 문서
- docs/agent-flow/plans/task-XXX.md

### 구현 문서
- docs/agent-flow/implementation/task-XXX-impl.md

### 리뷰 문서
- docs/agent-flow/reviews/task-XXX-review.md

### 테스트 문서
- docs/agent-flow/tests/task-XXX-test.md

### 릴리즈 문서
- docs/agent-flow/releases/task-XXX-release.md

## 7. task id 규칙
- TASK-001
- TASK-002
- TASK-003
- ...

파일명 형식:
- task-001.md
- task-001-impl.md
- task-001-review.md
- task-001-test.md
- task-001-release.md

## 8. 단계별 성공 조건

### planner 성공 조건
- plan 문서가 실제로 존재한다
- handoff_to 가 coder 이다
- 범위가 과도하게 확장되지 않았다
- 특정 도메인 요구사항이 명확히 구조화되었다

### coder 성공 조건
- impl 문서가 실제로 존재한다
- 필요한 실제 파일이 생성 또는 갱신되었다
- planner 의 in_scope 를 넘지 않았다

### reviewer 성공 조건
- review 문서가 실제로 존재한다
- decision 이 명시되었다
- 범위 초과 여부가 명시되었다

### tester 성공 조건
- test 문서가 실제로 존재한다
- pass_fail 이 명시되었다
- PASS 일 때만 다음 단계로 진행할 수 있다

### gitops 성공 조건
- release 문서가 실제로 존재한다
- git status 결과가 기록되었다
- 별도 지시가 없는 한 commit/push/deploy 를 수행하지 않았다

## 9. 다음 단계 진입 조건
- planner 성공 시에만 coder 진행
- coder 성공 시에만 reviewer 진행
- reviewer decision 이 approved 일 때만 tester 진행
- tester 결과가 PASS 일 때만 gitops 진행

## 10. 금지 사항
1. streamTo 사용 금지
2. 단계 건너뛰기 금지
3. 실패 후 다음 단계 진행 금지
4. planner 의 과개발 금지
5. coder 의 out_of_scope 구현 금지
6. reviewer 의 코드 수정 금지
7. tester 의 코드 수정 금지
8. gitops 의 무단 commit/push/deploy 금지
9. 파일이 없는데 성공이라고 보고하는 행위 금지

## 11. 도구 사용 원칙

### planner
- write/edit 사용 가능
- apply_patch 사용 금지

### coder
- read/write/edit/apply_patch/exec 사용 가능

### reviewer
- read/write/edit/exec 사용 가능
- apply_patch 사용 금지

### tester
- read/write/exec 사용 가능
- edit/apply_patch 사용 금지

### gitops
- read/write/exec 사용 가능
- edit/apply_patch 사용 금지

## 12. 메인 지시 해석 규칙
메인 지시문이 짧더라도 다음을 기본으로 해석한다.

- "표준 프로토콜대로 처리하라" = 이 문서를 따르라
- "새 task 시작" = planner 부터 시작하라
- "기존 기능 유지 + 기능 추가" = reviewer/tester 가 회귀 여부를 확인하라
- "실패 시 중단" = 이 문서의 단계 게이트를 따르라
- 도메인 요구사항은 planner 가 task 로 구조화하라

## 13. 최종 응답 형식
작업 종료 후 main 은 아래 형식으로 요약한다.

- planner_status:
- planner_file:
- coder_status:
- coder_files:
- coder_impl_file:
- reviewer_status:
- reviewer_file:
- tester_status:
- tester_file:
- gitops_status:
- gitops_file:
- overall_status:
- stopped_at_stage:
- next_action:
- summary:
