# Abyss — DDGI Light Diver · 최종 과제 리포트

> 컴퓨터 그래픽스 최종 과제
> 게임 링크: `https://<내아이디>.github.io/abyss/`
> 기술 스택: **Three.js r182 (WebGPU)** · GI 기법: **DDGI (Dynamic Diffuse Global Illumination)**

> ⚠️ **작성 규칙(채점 필수)**: 아래 *모든 항목*은 본인이 구현한 게임에서 직접 캡처한 이미지로 설명해야 함.
> 캡처가 없는 항목은 0점 처리됨. 이미지는 `./screenshots/` 폴더에 넣고 `![설명](./screenshots/파일명.png)` 형식으로 삽입.
> 작성하면서 `TODO` / `<여기에...>` 표시는 모두 본인 내용으로 교체할 것.

---

## 1. 게임 개요 (기획)

### 1.1 콘셉트
빛이 닿지 않는 심해 동굴을 탐험하는 발광 다이버. **플레이어 자신이 유일한 동적 광원**이며, 내가 움직일 때마다 주변 컬러 산호·암벽에 빛이 실시간으로 번진다(간접광). 그 빛을 단서로 어둠 속 비콘을 점등해 길을 연다.

`<여기에 본인이 추가/변경한 콘셉트 설명>`

![게임 타이틀/시작 화면](./screenshots/01_title.png)

### 1.2 코어 게임플레이 루프
`<여기에 루프 설명: 어둠 진입 → 내 빛으로 공간 인지 → 간접광으로 단서 발견 → 비콘 점등 → 다음 구역>`

![플레이 화면 전경](./screenshots/02_overview.png)

### 1.3 조작
- 이동: `WASD` / 상승·하강: `Space` / `C` / 시점: 마우스
- `<추가한 조작이 있으면 기재>`

---

## 2. 강의 내용 ↔ 구현 매핑

> ⚠️ 아래 "강의 주제"는 예시. **실제 수업에서 배운 항목명으로 교체**하고, 각 행마다 본인 게임 캡처를 붙일 것.

| 강의 주제(예시) | 구현에서의 대응 | 캡처 |
|---|---|---|
| Global Illumination 개념 | DDGI 간접광 항 적용 | `03_gi_concept.png` |
| Irradiance probe / light field | probe 배치 및 캡처 | `04_probe.png` |
| 렌더링 방정식 / diffuse irradiance | probe로부터 diffuse 간접광 계산 | `05_irradiance.png` |
| Octahedral mapping | irradiance/depth 인코딩 | `06_octahedral.png` |
| 가시성 / light leak 방지 | probe depth(Chebyshev) 테스트 | `07_visibility.png` |
| PBR / 직접광 라이팅 | 다이버 PointLight + 머티리얼 | `08_direct.png` |
| 톤매핑 / 포스트프로세싱 | ACES 톤매핑 적용 | `09_tonemap.png` |

`<각 행을 본인 강의 목차에 맞게 수정하고, 표 아래에 항목별로 캡처+설명을 풀어쓸 것>`

### 2.x (예시) Octahedral mapping
`<강의에서 배운 내용 한두 줄 요약 → 내 코드에서 어떻게 구현했는지 → 캡처로 증명>`

![octahedral 인코딩 결과](./screenshots/06_octahedral.png)

---

## 3. GI 기술 적용 상세 (DDGI)

### 3.1 적용한 DDGI 구조
- probe 그리드 배치
- 각 probe에서 환경 캡처(큐브맵) → irradiance 인코딩
- 표면 셰이딩 시 인접 probe 가중 샘플링
- 가시성 테스트로 light leak 차단

`<실제 구현한 항목 체크 및 설명>`

### 3.2 핵심 증명: GI ON / OFF 비교
간접광(DDGI)을 켰을 때와 껐을 때(`G` 키)의 차이. 산호 색이 주변 표면으로 번지는 color bleeding과 그늘의 간접 채움이 사라지는 것을 비교.

| GI ON | GI OFF |
|---|---|
| ![GI 켜짐](./screenshots/10_gi_on.png) | ![GI 꺼짐](./screenshots/11_gi_off.png) |

`<두 컷의 차이를 글로 설명>`

### 3.3 동적 GI (플레이어 = 광원)
같은 위치에서 다이버만 이동시켰을 때 간접광이 따라 움직이는 모습.

![동적 GI 전](./screenshots/12_dynamic_a.png)
![동적 GI 후](./screenshots/13_dynamic_b.png)

---

## 4. 개발 상세 (완성도)

### 4.1 기술 스택 / 구조
- Three.js r182 `three/webgpu`, WebGPURenderer (미지원 시 WebGL2 폴백)
- `<씬 구성, 주요 클래스/함수 구조 간단히>`

### 4.2 주요 구현 요소
`<동굴 챔버 / 다이버 / 카메라(스프링암) / probe / 입력 등 각각 한 단락 + 캡처>`

![3인칭 스프링암 카메라](./screenshots/14_camera.png)

### 4.3 겪은 문제와 해결
`<예: CubeRenderTarget 클래스명 문제 → WebGLCubeRenderTarget로 해결 등 실제 트러블슈팅 기록>`

---

## 5. 빌드 / 실행 방법
- 게임 링크(웹): `https://<내아이디>.github.io/abyss/`
- 로컬 실행: 폴더에서 `python3 -m http.server 8000` → `http://localhost:8000`
- 권장 브라우저: 최신 Chrome / Edge (WebGPU)

---

## 6. 회고
`<배운 점, 한계, 더 해보고 싶은 것(예: 트레이스드 DDGI로 확장)>`
