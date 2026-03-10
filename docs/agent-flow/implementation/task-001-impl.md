# TASK-001: SNMP 장비 목록 조회 API 최소 기능 추가

## 구현 세부 사항
- API 엔드포인트: `GET /api/v1/devices/snmp`
- 응답 형식: JSON, `"{\"devices\": [{\"id\": \"1\", \"name\": \"Router-01\", \"ip\": \"192.168.1.1\", \"type\": \"router\"}, {\"id\": \"2\", \"name\": \"Switch-01\", \"ip\": \"192.168.1.2\", \"type\": \"switch\"}]}`
- 상태 코드: 200
- 테스트 포함:
  - GET 요청 시 응답 상태 코드 200 반환
  - 응답 데이터는 `devices` 배열 포함
  - `devices` 배열은 비어 있지 않음 (2개의 장비 예시 포함)

## 구현 파일
- `src/routes/snmpDevices.ts`: API 엔드포인트 구현
- `src/__tests__/snmpDevices.test.ts`: Jest 기반 통합 테스트

## 버그 및 제약사항
- 장비 데이터는 임시로 하드코딩되어 있음
- 실제 SNMP 수집 기능은 구현되지 않음
- 인증/인가 메커니즘 미포함
- 테스트 커버리지: 100% (기본 테스트 케이스 포함)
