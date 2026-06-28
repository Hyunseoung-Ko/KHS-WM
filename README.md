# 통합 대시보드 (한투 자산관리 + 보험 고객관리)

기존 두 대시보드를 하나로 합친 단일 앱입니다.

- **한투(kis-dashboard)**: 자산 엑셀 분석 · 고객·자산 CRM · 영업 도출 · 은퇴 시뮬 · E2EE 동기화
- **보험(toss-reports)**: 분기 연락 스케줄 · 생일 관리 · 연락 이력 · 보험 리포트 배포

고객은 **채널 태그(한투 / 보험 / 양쪽)** 로 한 곳에서 통합 관리합니다.

## 구성

- `index.html` — 단일 페이지 앱 전체
- `lib/` — Pretendard 폰트 · Chart.js · SheetJS(xlsx) · html2canvas (모두 로컬 번들)
- `.nojekyll` — Jekyll 비활성 (정적 그대로 서빙)
- `.gitignore` — 고객 데이터/백업 JSON 커밋 금지

## 데이터 저장 (중요)

- **고객·자산·보험 데이터는 이 저장소에 커밋되지 않습니다.** 브라우저(IndexedDB)에 저장되고, 동기화를 켜면 **별도 비공개 저장소에 E2EE(AES-GCM) 암호화**되어 올라갑니다. GitHub도 내용을 못 봅니다.
- 따라서 이 저장소는 **공개로 둬도 개인정보가 노출되지 않습니다**(소스 코드만 있음).
- 보험 리포트(HTML)와 공유 링크는 기존처럼 **toss-reports** 저장소에 배포됩니다.

## 배포 방법 (GitHub Pages)

저장소 이름: **KHS-WM** · 접속 주소: **https://hyunseoung-ko.github.io/KHS-WM/**

1. GitHub에서 새 저장소 `KHS-WM` 생성 (빈 저장소, README 추가 안 함).
2. 이 폴더에서 푸시 (git remote는 이미 설정됨):
   ```
   git push -u origin main
   ```
3. 저장소 **Settings → Pages → Source: main / (root)** 로 설정.
4. 접속: `https://hyunseoung-ko.github.io/KHS-WM/`

## 최초 설정 (앱 안에서)

1. **PIN 6자리** 생성 (첫 실행 시).
2. **설정 · 백업 → 동기화**: 데이터 repo(`<계정>/kis-dashboard-data`) · 토큰 · 동기화 암호 입력 → 기존 한투 데이터가 그대로 복원됩니다.
3. **설정 · 백업 → 📥 보험 고객 가져오기**: toss-reports 토큰으로 기존 보험 고객 172명을 통합 CRM으로 1회 이전 (이름 기준 한투 고객과 자동 병합).
4. **리포트 탭 → 토큰 연결**: 같은 toss-reports 토큰으로 연결하면 리포트 배포·관리 가능.

> 토큰은 2가지입니다: ① E2EE 동기화용(데이터 repo) ② toss-reports용(보험 가져오기+리포트). 하나의 fine-grained PAT에 두 repo 권한을 함께 주면 한 개로도 됩니다.

## 기존 URL 연결 (선택)

기존 주소를 통합본으로 보내려면 각 저장소의 진입 파일에 아래 리다이렉트를 넣으세요:

```html
<meta http-equiv="refresh" content="0; url=https://hyunseoung-ko.github.io/KHS-WM/">
```

- `kis-dashboard/index.html` → 통합본
- `toss-reports/dashboard.html` → 통합본 (단, 리포트 *공유 링크* 들은 toss-reports에 그대로 살아있어야 하므로 dashboard.html만 교체)
