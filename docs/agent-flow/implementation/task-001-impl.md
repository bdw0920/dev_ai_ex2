# TASK-001: 장비 목록 조회 API 구현

## Objective
GET /devices 엔드포인트를 구현하여 등록된 장비 목록을 JSON 형식으로 반환한다.

## Implementation Plan

### 1. API 엔드포인트 정의
- `GET /devices` 엔드포인트 제공
- 응답 형식: JSON 리스트
- 응답 예: `[{"device_id": "d-1234567890", "device_name": "Router-01", "ip_address": "192.168.1.100", "vendor": "Cisco"}]`

### 2. 임시 저장소 구현
- 대체 저장소: 메모리 기반 Map 객체 (`Map<String, Object>`) 사용
- 장비 데이터는 `device_id`를 키로 저장
- `device_id`는 생성 시 랜덤ID 생성 및 저장

### 3. 응답 로직
- 성공 (200 OK): 장비 목록 반환
- 400 Bad Request: 요청이 잘못된 경우
- 500 Internal Server Error: 내부 오류 발생 시

### 4. Endpoint Implementation
```js
// Memory store
const registeredDevices = new Map();

// GET /devices
app.get('/devices', (req, res) => {
  try {
    // Retrieve all devices
    const devices = Array.from(registeredDevices.entries())
      .map(([id, data]) => ({
        device_id: id,
        ...data
      }))
      .sort((a, b) => b.device_id.localeCompare(a.device_id)); // 역순 정렬

    // Return list
    res.status(200).json(devices);
  } catch (error) {
    // Server error
    console.error('Error fetching devices:', error);
    res.status(500).json({
      error: 'Internal server error',
      details: 'Unable to retrieve device list'
    });
  }
});
```

## 도전 과제
- 가정: 실제로는 DB 연결 필요
- 여러 장비 동시 등록 처리 시 동시성 문제 예외
- 코드 작성이 완료된 후 OpenAPI 스펙 작성 필요

## 다음 단계
- reviewer에게 handoff
- 검토 요청 ("task_id": "TASK-001", "objective": "장비 목록 조회 API 구현")