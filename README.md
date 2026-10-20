# 🎵 Unity Rhythm Game

Unity 2018.4로 개발한 리듬게임 프로젝트

## 아키텍처 구조

```
Unity Rhythm Game/
├── Assets/
│   ├── Scripts/
│   │   ├── GameManager.cs        ← 싱글턴, 게임 전체 속도 제어
│   │   ├── NoteBehavior.cs       ← 노트 낙하 로직
│   │   └── BackGroundBehavior.cs ← 배경 페이드아웃 효과
│   ├── Scenes/
│   │   └── SampleScene.unity
│   └── Sprites/                  ← 18개 이미지 에셋
└── ProjectSettings/              ← Unity 2018.4.36f1
```

## 핵심 스크립트

### `GameManager.cs`
게임의 중심 컨트롤러. **싱글턴 패턴**으로 전역 접근 가능.

| 멤버 | 타입 | 역할 |
|---|---|---|
| `instance` | `static GameManager` | 전역 싱글턴 인스턴스 |
| `noteSpeed` | `float` | 노트 낙하 속도 |

- `Awake()`에서 싱글턴 초기화
- 다른 스크립트가 `GameManager.instance.noteSpeed`로 속도를 참조

### `NoteBehavior.cs`
노트 오브젝트에 붙는 스크립트. 매 프레임 아래로 이동.

| 멤버 | 타입 | 역할 |
|---|---|---|
| `noteType` | `int` | 노트 종류 (1~4) |
| `speed` | `float` | 개별 속도 (미사용) |

- `Update()`에서 `Vector3.down * GameManager.instance.noteSpeed`로 이동

### `BackGroundBehavior.cs`
배경 이미지를 점점 투명하게 만드는 효과.

- `Start()`에서 `FadeOut` 코루틴 시작
- `SpriteRenderer`의 알파값을 0.005씩 감소

## 에셋 구성

### 노트
`Note 1.png` ~ `Note 4.png` — 4방향 노트 스프라이트

### 판정 표시
| 판정 | 파일 |
|---|---|
| Perfect | `Perfect.png` |
| Good | `Good.png` |
| Bad | `Bad.png` |
| Miss | `Miss.png` |

### 결과 랭크
`Rank S.png` · `Rank A.png` · `Rank B.png` · `Rank C.png`

### 배경
- `Game Background.jpg` — 게임 플레이 배경
- `Game Background (Shine).jpg` — 빛나는 버전
- `Select Background.jpg` — 선택 화면
- `Result Background.jpg` — 결과 화면

## 게임 흐름

```
GameManager (싱글턴)
    │
    ├── noteSpeed ──→ NoteBehavior
    │                    │
    │                    └── 매 프레임 아래로 이동
    │                        (Note 1~4 스프라이트)
    │
    └── 같은 Scene ──→ BackGroundBehavior
                         │
                         └── 시작 시 배경 FadeOut
```

1. `GameManager`가 `Awake()`에서 싱글턴으로 초기화
2. `BackGroundBehavior`가 `Start()`에서 배경 페이드아웃 시작
3. `NoteBehavior`가 `Update()`마다 노트를 아래로 이동
4. 판정 시스템은 스프라이트가 준비되어 있음 (Perfect/Good/Bad/Miss)

## 코드 통계

| 언어 | 파일 수 | 코드 줄 | 비율 |
|---|---|---|---|
| C# | 3 | 36 | 49% |
| JSON | 1 | 37 | 51% |

**총 73줄** · 스프라이트 18장 · Unity 2018.4.36f1

---

*Analyzed by Hermes Agent*
