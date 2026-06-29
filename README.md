# Archive

블로그(Tistory 등)에 임베드할 정적 자산을 호스팅하는 **공개** 저장소입니다.
Tistory 본문에서는 외부 CDN(예: `cdn.plot.ly`) 스크립트가 차단되어 Plotly 차트가
렌더링되지 않습니다. 그래서 차트를 **독립 실행형 HTML**로 만들어 이 저장소에 올리고,
**GitHub Pages**로 서빙한 뒤 본문에서는 `<iframe>`으로 불러옵니다. iframe 내부 문서는
github.io 도메인에서 자체적으로 로드되므로 Tistory의 CDN 차단(CSP)에 영향을 받지 않습니다.

## ⚠️ 공개 저장소 규칙

- 이 저장소는 **공개**입니다. 누구나 모든 파일을 볼 수 있습니다.
- **개인정보, API 키, 토큰, 비밀번호, 환경 변수, 내부 경로, 자격 증명을 절대 올리지 않습니다.**
- 차트에 들어가는 데이터는 **이미 공개 가능한 집계/시각화 결과물**만 포함합니다.
  (원본 raw 데이터·PII는 업로드 금지)
- 업로드 전 점검: `grep -riE 'api[_-]?key|secret|token|password|/home/|os\.environ' <폴더>`

## 📁 폴더 구조 (목적 / 날짜)

자산은 **목적(영문)** → **날짜(YYYY-MM-DD)** 트리로 관리합니다. 같은 주제를 반복 업로드하기
때문에, 목적별로 묶고 그 아래 작업 날짜로 버전을 구분합니다.

```
<purpose-in-english>/
└── <YYYY-MM-DD>/
    ├── chart-01-<slug>.html
    ├── chart-02-<slug>.html
    └── ...
```

- `purpose` : 영문 kebab-case (예: `pension-analysis`, `housing-price`)
- `YYYY-MM-DD` : 자산을 만든 날짜
- `chart-NN-<slug>.html` : 두 자리 순번 + 영문 설명 slug

### 예시

```
pension-analysis/
└── 2026-06-29/
    ├── chart-01-employment-by-company-size.html
    ├── chart-02-monthly-hiring-and-leaving.html
    └── ...
```

## 🌐 GitHub Pages 설정 (최초 1회)

Settings → Pages → Source: `Deploy from a branch` → Branch: `main` / `/ (root)` → Save.
배포 후 서빙 주소:

```
https://seohyunjun.github.io/Archive/<purpose>/<YYYY-MM-DD>/<file>.html
```

## 🧩 새 차트 추가 → 임베드 절차

1. Plotly figure를 **독립 실행형 HTML**로 저장합니다.
   - 헤드에 Plotly CDN(`<script src="https://cdn.plot.ly/plotly-x.y.z.min.js">`)을 포함.
   - 민감 정보 없이 시각화 결과만 포함.
   - 파이썬 예시: `fig.write_html("chart-01-foo.html", include_plotlyjs="cdn", full_html=True)`
2. `<purpose>/<YYYY-MM-DD>/` 폴더에 넣고 `git add` → `commit` → `push`.
3. 블로그 본문(HTML 모드)에서 `<iframe>`으로 불러옵니다.

```html
<iframe
  src="https://seohyunjun.github.io/Archive/pension-analysis/2026-06-29/chart-01-employment-by-company-size.html"
  loading="lazy" scrolling="no"
  style="width:100%;height:484px;border:0;overflow:hidden;display:block;"
  title="Employment by company size"></iframe>
```

- `height`는 차트 높이 + 여백(약 24px)으로 맞춥니다.
- 차단되는 CDN 스크립트는 본문이 아닌 **iframe 내부 문서**에서만 로드됩니다.
