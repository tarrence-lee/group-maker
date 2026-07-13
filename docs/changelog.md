# 변경 이력

## v1 — 초기 QA 및 버그 수정
- `computeGroupSizes(n)`: 참가자 5명일 때 [2명, 3명]으로 나뉘던 버그 수정. 5는 3~4명 단위로 정확히
  나눌 수 없는 유일한 인원수(Frobenius 문제)라서, 5명은 예외적으로 단일 조(5명)로 처리하도록 결정.
- 참가자 수 "＋" 스테퍼 버튼에 100명 상한이 없던 버그 수정.
- "참가자 명단" 붙여넣기 기능 신규 추가: 공백 구분 시 단어 단위 이름, 공백 없을 시 글자 단위 이름으로 자동 인식.

## v2 — UI 구조 변경
- "참가자 명단" 붙여넣기 입력 필드의 라벨을 단순화하고 한 줄 입력창으로 변경, 화면 상단으로 이동.
- "다시하기"(전체 초기화) 버튼 추가 → 이후 헤더의 제목(`h1`) 옆으로 재배치.
- 기존 "참가자 수 조절(－/＋)" 스테퍼와 "이름 지우기" 버튼 줄을 완전히 삭제. 이후 인원 조정은
  참가자 명단 붙여넣기, 개별 행 삭제(✕), 다시하기 버튼으로만 가능.

## v3 — 재검증(QA) 결과 및 후속 수정
QA 재검증에서 발견된 항목 중 아래 2건을 프론트엔드 재설계 시 함께 반영:
- **WCAG 2.5.3 (Label in Name) 위반 수정**: "다시하기" 버튼의 `aria-label`이 화면에 보이는 텍스트를
  포함하지 않던 문제 → `aria-label="다시하기 (전체 초기화)"`로 수정.
- **붙여넣기 파싱 결과 피드백 추가**: 공백 없는 문자열을 붙였을 때 글자 단위로 조용히 쪼개지던 문제 →
  적용 후 항상 "이름 N명으로 인식했습니다" / "글자 단위로 N명 인식했습니다" 상태 메시지를 표시하도록 개선.
- (참고) 360px 화면에서 제목/버튼이 다소 어색하게 줄바꿈되는 화장용 이슈는 발견되었으나 기능에 영향 없어
  이번 라운드에서는 보류.

## v4 — 모바일 재설계 + PWA화 + 배포
- 모바일 한 화면(375×667 기준)에 기본 8명 명단이 스크롤 없이 보이도록 헤더/입력창/탭 여백·폰트 축소.
- `group-maker.html` → `index.html`로 파일명 변경 (GitHub Pages 루트 자동 서빙을 위함).
- PWA 설치 지원 추가: `manifest.json`, `service-worker.js`(오프라인 캐싱), 아이콘 3종
  (`icon-192.png`, `icon-512.png`, `apple-touch-icon.png`) 신규 생성.
- GitHub 공개 저장소 `tarrence-lee/group-maker` 생성 및 최초 커밋 푸시.
- GitHub Pages 배포 활성화 (`master` 브랜치, 루트 경로) → https://tarrence-lee.github.io/group-maker/
- `docs/` 폴더에 프로젝트 문서화(개요/사용법/배포/변경 이력) 추가.

## v5 — Vercel로 호스팅 이전
- `vercel.json` 추가: `service-worker.js`/`manifest.json`은 `no-cache`, 아이콘은 1년 캐시(`immutable`)로 헤더 지정.
- GitHub 저장소를 Vercel에 연동해 `master` push 시 자동 배포되도록 설정 → https://group-maker-five.vercel.app/
- 배포 파일이 로컬 소스와 바이트 단위로 동일함을 확인, 서비스워커 등록(`activated`) 및 조 편성 기능 정상 동작 확인.
- 기존 GitHub Pages 비활성화 (`DELETE /repos/tarrence-lee/group-maker/pages`), `tarrence-lee.github.io/group-maker/`는 404.
- README/docs의 라이브 주소를 Vercel 주소로 갱신.
