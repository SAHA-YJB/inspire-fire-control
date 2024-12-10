# 프로젝트 초기 세팅

- Next.js
- Tailwind CSS
- TypeScript
- Shadcn UI

# 네이밍 컨벤션

###### 카멜케이스 / 상수 => 스네이크케이스

###### page.tsx, layout.tsx

- export default function Home() {}
- export default function OOOPage() {}

###### page.tsx, layout.tsx 외 components

- export const ComponentName: FC = () => {}

---

# 데이터 흐름도

```mermaid
graph TB
    subgraph Orin
        O[Orin AI Detection]
        DS[DeepStream]
    end

    subgraph Cameras-NVR
        C1[Camera 1 RTSP]
        C2[Camera 2 RTSP]
        C3[Camera n RTSP]
    end
    subgraph Computer
        subgraph Backend
            BS[Backend Server]
            WS[WebSocket Server]
            DB[(Database)]
        end

        subgraph Frontend
            FC[Frontend Client]
            VS[Video Streaming]
            UI[UI Display]
        end
    end

    C1 & C2 & C3 --> |RTSP Port| BS
    BS -->|RTSP Port| FC
    FC -->|Display Stream| VS

    C1 & C2 & C3 --> O
    O --> DS
    DS -->|객체 검출 데이터, 온도값| WS
    WS -->|데이터 저장| DB
    WS -->|실시간 데이터| FC
    FC -->|데이터 표시| UI

   style O fill:#f96,stroke:#333
    style DS fill:#f96,stroke:#333
    style C1 fill:#69f,stroke:#333
    style C2 fill:#69f,stroke:#333
    style C3 fill:#69f,stroke:#333
    style BS fill:#7fb77e,stroke:#333
    style WS fill:#7fb77e,stroke:#333
    style DB fill:#7fb77e,stroke:#333
    style FC fill:#b784a7,stroke:#333
    style VS fill:#b784a7,stroke:#333
    style UI fill:#b784a7,stroke:#333
    style Backend fill:#f0fff0,stroke:#333
    style Frontend fill:#fff0f5,stroke:#333
     style Computer fill:#f5f5f5,stroke:#333,font-weight:bold,font-size:16px

```

# process

```mermaid
graph TB
    subgraph Detection[감지 프로세스]
        AI[AI 판단]
        TEMP[온도 감지]

        subgraph Conditions[조건 판단]
            ACC[설정 판단률 < AI 판단률]
            TEMP_COND[설정 온도 < 감지 온도]
            DANGER[위험 조건 = 판단률 && 온도]
        end
    end

    subgraph Actions[감지 후 동작]
        ZONE[주차구역 판별]
        LOC[카메라 위치 확인]
        SEND[서버로 데이터 전송]
        RELAY[릴레이 작동]
        RESET[복구 버튼]
    end

    subgraph Alerts[경고 상태]
        ACC_WARN[판단률 경고]
        TEMP_WARN[온도 경고]
        DANGER_WARN[화재 발생]
    end

    AI --> ACC
    TEMP --> TEMP_COND

    ACC --> |"true"| ACC_WARN
    TEMP_COND --> |"true"| TEMP_WARN

    ACC & TEMP_COND --> DANGER
    DANGER --> |"true"| DANGER_WARN

    DANGER_WARN --> ZONE
    ZONE --> LOC
    LOC --> SEND
    SEND --> RELAY

    RESET --> |"초기화"| RELAY

    style Detection fill:#f0f0f0,stroke:#333
    style Actions fill:#e6f3ff,stroke:#333
    style Alerts fill:#fff0f0,stroke:#333
    style DANGER_WARN fill:#ff6b6b,stroke:#333,color:#fff
    style RESET fill:#4CAF50,stroke:#333,color:#fff
    style RELAY fill:#64b5f6,stroke:#333
    style ACC_WARN fill:#ffd54f,stroke:#333
    style TEMP_WARN fill:#ffd54f,stroke:#333
```

---

### This project

###### 프로젝트 소개

- 리조트 관리 시스템을 위한 웹 애플리케이션.

- 사용자에게 시각적으로 정보를 제공하여 리조트의 안전과 효율적인 운영을 지원
- 리조트의 안전과 운영 효율성을 높이기 위해 설계
- 사용자에게 직관적이고 실시간으로 정보를 제공하는 것을 목표

---

### 주요 기능 및 핵심 기술 구현사항

1. 실시간 CCTV 모니터링

   - WebSocket을 통한 실시간 CCTV 영상 스트리밍
   - 카메라의 경고 상태에 따라 UI에 경고 표시
   - 사용자 설정에 따라 카메라의 위치와 이름을 직접 편집 가능

2. 알람 시스템

   - Web Audio API를 활용한 커스텀 알람 소리 재생
   - 온도 및 AI 판단 결과에 따라 경고 발생
   - 사용자 설정 기반 알람 동작 제어 및 다중 알람 상태 관리

3. 데이터 시각화 및 로그 관리

   - Recharts를 활용한 실시간 데이터 차트
   - 카메라에서 수집된 로그 데이터를 테이블 형식으로 시각화
   - 화재 및 연기 감지 데이터를 시각화하여 차트로 제공
   - 로그 데이터를 엑셀 파일로 다운로드 가능

4. 사용자 설정 및 보안 기능
   - 카메라 위치와 이름, 알람 소리 및 알람 바 설정
   - 판단률 및 온도 설정
   - 토큰 기반 인증 시스템을 통한 보안 강화
   - 세션 관리 및 자동 로그아웃 기능

###### 기술 스택

- 프론트엔드: Next.js, TypeScript, Tailwind CSS, Shadcn UI
- 기타: Socket.io를 사용하여 실시간 데이터 전송 및 WebSocket 연결을 관리

###### 코드 구조

1. src/app/components
   - UI 컴포넌트들이 위치한 디렉토리로, 각 페이지에 필요한 다양한 컴포넌트들이 포함
2. src/utils
   - 데이터 처리 및 API 호출을 위한 유틸리티 함수들이 포함
3. src/types
   - TypeScript 타입 정의 파일들이 포함
