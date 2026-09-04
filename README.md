# Bricks_portfolio

**2:2 팀 대항으로 진행되는 4인 멀티플레이 장애물 경주 게임**에서
제가 담당한 Gameplay Framework, 접속 플로우, 실시간 진행률 UI 소스입니다.

> **참고** — 포트폴리오 목적으로 본인이 작성한 소스만 발췌했습니다.
> 빌드 가능한 전체 프로젝트가 아니며, 에셋과 바이너리는 포함되어 있지 않습니다.

<br>

## 프로젝트 개요

| | |
| --- | --- |
| **기간** | 2025.04 ~ 2025.05 (1개월) |
| **팀 규모** | 5인 |
| **엔진** | Unreal Engine 5 |
| **네트워크** | Listen Server |

**기술 스택** &nbsp;C++ · Unreal Engine 5 · Blueprint · Listen Server · Replication · RPC

<br>

## 담당 업무

- GameInstance, GameMode, GameState, PlayerState 기반 멀티플레이 상태 구조 구현
- **네 명 전원의 실시간 진행률 UI 제안 및 단독 구현**
  두 팀이 떨어진 트랙에서 동시에 출발하는 구조라 상대 상황을 파악하기 어렵던 문제를
  팀 회의에서 제기해 채택된 뒤 직접 구현했습니다.
- 로비에서 선택한 팀·ID가 레벨 전환 시 초기화되던 문제를 PlayerState의 `CopyProperties` 오버라이드로 해결
- 호스트에서는 정상이나 클라이언트에 반영되지 않던 문제를 속성 리플리케이션 등록으로 해결
- 호스트·클라이언트를 에디터에서 동시 실행해 전 화면을 대조하는 검증 절차 수립

<br>



## 주요 파일

###GameplayFramework

| 파일 | 역할 |
| --- | --- |
| `BrickGameInstance` | 레벨 전환에도 유지되어야 하는 세션·팀 데이터 보관 |
| `BrickLobbyGameMode` / `BrickLobbyGameState` | 로비 접속·팀 선택·준비 상태 관리 |
| `BrickInGameMode` / `BrickInGameState` | 경주 진행 규칙 처리, 각 PlayerState의 진행률을 집계해 순위 산출 |
| `BrickGamePlayerState` | 팀·ID·진행률 등 플레이어별 데이터 보관, `CopyProperties` 오버라이드로 레벨 전환 간 값 유지 |
| `BrickGamePlayerController` | 입력 처리 및 UI 연결 |
| `BrickPlayerStart` | 팀별 스폰 위치 지정 |
| `EGameTeam` | 팀 구분 열거형 |

### UI

| 파일 | 역할 |
| --- | --- |
| `ProgressBoard` | 네 명 전원의 실시간 진행률 표시. 팀 색상 구분 및 자기 위치 표시 |
| `LobbyUserWidget` | 로비 화면. 접속 플레이어 목록 표시 |
| `LobbyUserEntry` | 플레이어별 로비 항목. 소속 팀 선택 처리 |
| `CountdownUserWidget` | 경주 시작 카운트다운 |

### Props

| 파일 | 역할 |
| --- | --- |
| `CheckPoint` | 구간별 리스폰 지점. 낙사해도 처음이 아닌 가까운 지점에서 재시작 |
| `KillVolume` | 낙사 판정. 통과한 체크포인트로 리스폰 처리 |
| `FinishGoalVolume` | 결승 지점 통과 판정 |
