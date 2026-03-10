# TASK-001: 장비 목록 조회 API 설계

## 목적
NMS 서비스에 등록된 장비 목록을 조회할 수 있는 RESTful API를 제공한다.

## 범위 (In Scope)
- GET /devices 엔드포인트 제공
- 반환 형식: JSON 리스트
- 응답 예시: `[{"device_id": "d-1234567890", "device_name": "Router-01", "ip_address": "192.168.1.100", "vendor": "Cisco"}]`
- 임시 저장소 기반 (메모리 기반 Map 기반)

## 제외 (Out of Scope)
- 검색 기능 (device_name, vendor 등 필드 기준)
- 페이지네이션 (`page`, `limit` 파라미터)
- 정렬 (`sort`, `order` 파라미터)
- 장비 수정/삭제 API
- 인증/권한 처리
- 프론트엔드 UI
- DB 마이그레이션 또는 실제 데이터베이스 연동

## 완료 조건 (Done Criteria)
- `/devices` 엔드포인트가 동작하며 정상 응답을 반환
- 응답은 JSON 형식이며, 장비 정보 목록을 포함
- 요청이 잘못된 경우 400 응답 반환
- 기술적 오류 발생 시 500 응답 반환
- 오류 메시지가 명확하고 유의미함

## 예시 요청/응답

**요청:**
```http
GET /devices HTTP/1.1
Host: nms.example.com
```

**응답 (성공):**
```http
HTTP/1.1 200 OK
Content-Type: application/json

[
  {
    "device_id": "d-1234567890",
    "device_name": "Router-01",
    "ip_address": "192.168.1.100",
    "vendor": "Cisco"
  },
  {
    "device_id": "d-0987654321",
    "device_name": "Switch-01",
    "ip_address": "192.168.1.200",
    "vendor": "HP"
  }
]
```

**응답 (실패 - 오류):**
```http
HTTP/1.1 500 Internal Server Error
Content-Type: application/json

{
  "error": "Internal server error",
  "details": "Database query failed"
}
```

## 추가 참고
- 임시 저장소: 메모리 기반 (예: Map<String, Object>)
- 장비 ID는 역순으로 반환 (최신 생성 순서)
- 응답은 비동기적 처리 없이 동기적으로 이루어짐

## 다음 단계
- 현재 작업이 승인된 후 해당 기능을 너비 구현할 수 있도록 하기 위해 coder에게 수신할 수 있다.