# 배포 및 PWA 설치

## 저장소 / 라이브 주소
- GitHub 저장소: https://github.com/tarrence-lee/group-maker (public)
- GitHub Pages 라이브 주소: https://tarrence-lee.github.io/group-maker/
- 배포 브랜치/경로: `master` 브랜치, 저장소 루트(`/`)를 GitHub Pages 소스로 사용

## 배포 구조
GitHub Pages는 프로젝트 사이트(`https://<계정>.github.io/group-maker/`) 형태로 서빙되며, 커스텀 도메인이
아닌 하위 경로이기 때문에 `manifest.json`/`service-worker.js`/아이콘 경로가 전부 **상대 경로**로 작성되어
있습니다 (`start_url: "./index.html"`, `scope: "./"` 등). 다른 계정/경로로 옮겨 배포해도 그대로 동작합니다.

## 파일 구조
```
group-maker/
├── index.html          # 앱 본체 (HTML/CSS/JS 전부 단일 파일)
├── manifest.json        # PWA 매니페스트 (이름, 아이콘, 시작 URL 등)
├── service-worker.js    # 오프라인 캐싱용 서비스 워커 (cache-first)
├── icons/
│   ├── icon-192.png
│   ├── icon-512.png
│   └── apple-touch-icon.png   # iOS 홈 화면 아이콘 (불투명 배경)
└── docs/                # 이 문서 폴더
```

## 재배포 방법
1. `index.html`/`manifest.json`/`service-worker.js`/`icons/`를 수정합니다.
2. `service-worker.js`의 `CACHE_NAME` 값을 변경하면(예: `v1` → `v2`) 기존 방문자의 캐시가 무효화되고
   새 버전이 강제로 다시 받아집니다. 콘텐츠만 바꾸고 캐시 이름을 그대로 두면 이미 설치한 사용자에게는
   한동안 이전 버전이 보일 수 있습니다.
3. 변경 사항을 커밋하고 `master` 브랜치에 푸시하면 GitHub Pages가 자동으로 다시 빌드/배포합니다.
   ```
   git add -A
   git commit -m "설명"
   git push
   ```
4. 배포는 보통 1분 이내에 반영됩니다. 진행 상황은 저장소의 Actions 탭 또는
   `gh api repos/tarrence-lee/group-maker/pages/builds/latest` 로 확인할 수 있습니다.

## 로컬에서 미리 보기
서비스 워커는 `https` 또는 `localhost`에서만 동작하므로, 단순히 파일을 더블클릭해서 여는 것(`file://`)으로는
PWA 설치/오프라인 캐싱이 동작하지 않습니다. 로컬에서 테스트하려면 간단한 로컬 서버가 필요합니다. 예:
```
python -m http.server 8000
```
그 후 `http://localhost:8000` 으로 접속하면 서비스 워커와 설치 프롬프트가 정상 동작합니다.

## 보안/키 관련 참고
이 앱은 외부 API를 호출하지 않고 서버도 없는 순수 클라이언트 앱이므로, API 키나 `.env` 파일이 필요 없습니다.
저장소에도 그런 파일이 없고, 앞으로도 추가할 계획이 없다면 그대로 유지하면 됩니다.
