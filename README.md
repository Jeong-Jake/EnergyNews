# Energy News Briefing

한국 에너지/전력 관련 뉴스를 자동 수집하여 **카드뉴스 웹 UI**로 보여주고, **이메일 브리핑**까지 발송하는 서비스입니다.

## 주요 기능

| 기능 | 설명 |
|------|------|
| 자동 뉴스 수집 | Google News RSS 기반, 매일 08:00 / 14:00 KST 자동 실행 |
| 카드뉴스 웹 UI | GitHub Pages로 호스팅, PC 3열 / 태블릿 2열 / 모바일 1열 반응형 |
| VPP 우선 표시 | VPP, 가상발전소, 통합발전소 관련 기사 상단 배치 |
| 카테고리 필터 | 카테고리별 버튼 클릭으로 필터링 |
| 중복 기사 제거 | 링크 + 제목 유사도(70%) 기반으로 다른 신문사 중복 기사 자동 제거 |
| 이메일 브리핑 | 뉴스 수집 후 HTML 이메일 자동 발송 (복수 수신자 지원) |

## 검색 카테고리

VPP | 전력시장 | ESS | 태양광/RPS | 차세대전력망 | 전기요금 | 제도개선 | AI

## 프로젝트 구조

```
EnergyNews/
├── index.html                          # 카드뉴스 웹 UI
├── data/
│   └── news.json                       # 수집된 뉴스 데이터 (자동 갱신)
├── scripts/
│   ├── fetch_news.py                   # 뉴스 수집 스크립트
│   └── send_email.py                   # 이메일 발송 스크립트
└── .github/
    └── workflows/
        └── fetch-news.yml              # GitHub Actions 워크플로우
```

## 설정 방법

### 1. GitHub 레포지토리 생성 & 푸시

```bash
cd energy-news
git init
git add .
git commit -m "초기 설정: 에너지 뉴스 브리핑"
git remote add origin https://github.com/본인아이디/EnergyNews.git
git branch -M main
git push -u origin main
```

### 2. GitHub Pages 활성화

1. 레포지토리 → **Settings** → **Pages**
2. Source: **Deploy from a branch**
3. Branch: **main** / **/ (root)** 선택
4. **Save**

### 3. 이메일 발송 설정 (선택사항)

레포지토리 → **Settings** → **Secrets and variables** → **Actions** 에서 아래 3개 시크릿 추가:

| Secret Name | 값 |
|-------------|-----|
| `GMAIL_ADDRESS` | 발신 Gmail 주소 |
| `GMAIL_APP_PASSWORD` | Gmail 앱 비밀번호 (16자리) |
| `RECIPIENT_EMAIL` | 수신자 이메일 (복수일 경우 쉼표로 구분) |

> Gmail 앱 비밀번호는 [Google 계정 > 보안 > 2단계 인증 활성화 > 앱 비밀번호](https://myaccount.google.com/apppasswords)에서 생성합니다.

### 4. 첫 실행 (수동)

1. 레포지토리 → **Actions** 탭
2. **"에너지 뉴스 수집"** 워크플로우 선택
3. **"Run workflow"** 클릭

이후 매일 08:00, 14:00 KST에 자동 실행됩니다.

## 접속

```
https://본인아이디.github.io/EnergyNews
```

## 커스터마이징

### 검색 키워드 수정
`scripts/fetch_news.py`의 `SEARCH_QUERIES` 리스트를 수정하세요:

```python
SEARCH_QUERIES = [
    ("VPP 가상발전소 통합발전소", "VPP"),
    ("전력시장 전력거래", "전력시장"),
    # 원하는 키워드 추가...
]
```

### VPP 우선 키워드 수정
`scripts/fetch_news.py`의 `PRIORITY_KEYWORDS` 리스트를 수정하세요:

```python
PRIORITY_KEYWORDS = ["VPP", "vpp", "가상발전소", "통합발전소", "분산에너지"]
```

### 수집 시간 변경
`.github/workflows/fetch-news.yml`의 cron 설정을 수정하세요 (UTC 기준):

```yaml
schedule:
  - cron: '0 23 * * *'   # 08:00 KST
  - cron: '0 5 * * *'    # 14:00 KST
```
