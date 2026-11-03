# A3-2 - 영상 속 움직이는 대상 따라가기 (object-tracking)

> 🚧 **미착수** — 디렉터리·레포 준비만 완료

배경 차분으로 영상 속 움직이는 객체를 감지하고 고유 ID 를 붙여 추적하며, 미리 등록한 물체를
특징점 매칭(ORB/SIFT)으로 다시 찾아낸다. 겹침·정지·조명 변화에서 ID Switch 와 추적 실패를 측정해
"위치 정보만으로는 객체를 구분할 수 없다"는 한계를 보고서로 정리한다.

- 과제 원문: 로컬 `../a3-2-object-tracking.md`
- AI 심화 학습 · 컴퓨터 비전 · 80시간 · 필수 · 개인
- Python 3.8 이상 (권장 3.10+, OpenCV 4.5+ · SIFT 는 `opencv-contrib-python`, ORB 는 기본 패키지)

## 설치 및 실행

```bash
uv venv --python 3.12 .venv && source .venv/bin/activate
uv pip install -r requirements.txt   # uv 가 없으면 python -m venv .venv 후 pip install -r requirements.txt
```

진입점은 착수 때 정한다.

## 테스트 영상 (과제 §7)

- 필수: "두 객체 겹침" 시나리오 20~30초
- 권장: "조명 급변", "정지/재등장" 시나리오 각 1개
- 웹캠 직접 촬영 또는 공개 데이터셋(PETS·MOT Challenge) — 보고서에 영상명·조건 라벨·측정 방법 기입

## 라이브러리 제약 (과제 §7)

| 구분 | 내용 |
|---|---|
| 금지 | YOLO·SSD·R-CNN 등 딥러닝 탐지 모델, OpenCV `dnn` 모듈, MediaPipe 등 고수준 AI 라이브러리 |
| 구조 | `Tracker` 클래스는 별도 파일 `tracker.py` 로 분리 |

## 제출물 체크리스트

- [ ] 영상 입출력 — `cv2.VideoCapture`(웹캠/파일), FPS 표시, 키 `q` 종료·`s` 저장·`p` 일시정지
- [ ] 배경 차분 — MOG2 또는 `absdiff`, 모폴로지(열림·닫힘), 학습률 3가지 이상 비교
- [ ] 객체 감지 — 윤곽선, 면적 필터, 바운딩 박스·중심점
- [ ] `tracker.py` — 거리 기반 매칭, 일정 프레임 미등장 시 ID 삭제, 새 ID 부여, 궤적 저장·표시
- [ ] 특징점 매칭 — ORB 또는 SIFT, 매칭 성공 시 "TARGET DETECTED"
- [ ] 정량 측정 — ID Switch·추적 실패 수동 측정, 조건별 실패율 (기록 템플릿: 영상명-조건-프레임 범위-ID Switch-실패-메모)
- [ ] 분석 보고서 — 추적 성능표, 특징점 매칭률(회전·가림), 배경 모델링·거리 기반·Hand-crafted Feature 의 한계, 딥러닝이 필요한 이유
- [ ] GitHub 리포지토리 제출

## 보너스

- [ ] Hungarian Algorithm (scipy 없이 NumPy) vs Greedy
- [ ] 침입 감지 구역 (마우스 지정 + 경고·로그)
- [ ] IoU 기반 매칭 vs 중심점 거리
- [ ] 칼만 필터 예측 (`cv2.KalmanFilter`) 전후 ID Switch 비교
