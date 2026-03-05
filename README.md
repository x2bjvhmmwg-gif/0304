

## 1️⃣ 환경 및 모듈 준비

### 가상환경
```bash
python -m venv .venv
.venv\Scripts\activate   # Windows
````

### 필수 패키지 설치

```bash
pip install streamlit pandas numpy beautifulsoup4 requests trafilatura altair vega_datasets ollama
pip install --upgrade pip
```

### Streamlit 실행

```bash
streamlit run app1.py
```

---

## 2️⃣ Streamlit 기본 구조

```python
import streamlit as st

st.set_page_config(page_title="페이지제목", page_icon="💗", layout="wide")

st.title("앱 제목")
st.text("설명문구")

# 상태 관리
if 'slider_value' not in st.session_state:
    st.session_state.slider_value = (2017,2022)
```

### 주요 위젯

| 위젯                                                          | 용도        |
| ----------------------------------------------------------- | --------- |
| `st.selectbox()`                                            | 선택 박스     |
| `st.slider()`                                               | 범위 선택     |
| `st.button()`                                               | 버튼 클릭     |
| `st.tabs()`                                                 | 탭 구조      |
| `st.dataframe()`                                            | 표 출력      |
| `st.bar_chart()` / `st.line_chart()` / `st.scatter_chart()` | 간단 차트     |
| `st.altair_chart()`                                         | Altair 차트 |
| `st.markdown()`                                             | 마크다운 출력   |
| `st.image()`                                                | 이미지 출력    |
| `st.download_button()`                                      | 파일 다운로드   |

---

## 3️⃣ 데이터 수집

### A. 멜론 음원 정보

* 사용 모듈: `requests`, `BeautifulSoup`, `pandas`
* 순서:

  1. URL 요청 → HTML 수신
  2. 태그 선택 → CSS 선택자 활용 (`select()`, `.text`)
  3. DataFrame 생성
  4. Streamlit으로 출력
  5. DB 저장 가능

### B. 위키백과 시즌별 에피소드

* 목표: 시즌별 에피소드 + 요약 수집
* 사용 모듈: `BeautifulSoup`, `pandas`, `json`
* 순서:

  1. URL 요청 → HTML 수신
  2. 테이블 선택 → `select_one("table.wikitable.plainrowheaders.wikiepisodetable")`
  3. 에피소드별 요약 추출
  4. JSON / DataFrame / HTML 탭으로 출력
  5. 다운로드 버튼 제공

### C. 뉴스 기사 수집 및 번역

* 사용 모듈: `trafilatura`, `ollama`
* 순서:

  1. URL 입력 → `trafilatura.fetch_url()`
  2. 기사 본문과 이미지 추출 → `tra.extract()`, `tra.extract_metadata()`
  3. 한국어 여부 검사 → 정규식 `[ㄱ-ㅎㅏ-ㅣ가-힣]`
  4. 영어 기사 → Ollama AI 번역

```python
prompt = f"다음 영어 기사를 한글로 번역해주세요:\n{text}"
stream = ollama.chat(model="gemma3:4b", messages=[{"role":"user","content":prompt}], stream=True)
```

5. 스트리밍으로 화면 출력
6. 필요시 다운로드

---

## 4️⃣ 데이터 시각화

### Streamlit 기본 차트

```python
st.bar_chart(df, x="컬럼", y="컬럼")
st.line_chart(df)
st.scatter_chart(df)
```

### Altair 차트

```python
import altair as alt
chart = alt.Chart(source).mark_circle().encode(
    x='Horsepower',
    y='Miles_per_Gallon',
    color='Origin'
).interactive()
st.altair_chart(chart, use_container_width=True)
```

### 슬라이더 범위 선택

```python
sl = st.slider("년도 범위", min_value=1989, max_value=2023, value=st.session_state.slider_value, step=1)
filtered_df = df.filter(items=[str(i) for i in range(sl[0], sl[1]+1)])
```

---

## 5️⃣ 공식문서 링크

| 항목          | 링크                                                                                                               |
| ----------- | ---------------------------------------------------------------------------------------------------------------- |
| Streamlit   | [https://docs.streamlit.io](https://docs.streamlit.io)                                                           |
| Charts      | [https://docs.streamlit.io/develop/api-reference/charts](https://docs.streamlit.io/develop/api-reference/charts) |
| Altair      | [https://altair-viz.github.io](https://altair-viz.github.io)                                                     |
| Trafilatura | [https://trafilatura.readthedocs.io](https://trafilatura.readthedocs.io)                                         |
| Ollama      | [https://ollama.com/docs](https://ollama.com/docs)                                                               |

---

## 6️⃣ 사용 기술 요약

| 기술        | 목적           | 모듈                                                 |
| --------- | ------------ | -------------------------------------------------- |
| 웹 스크래핑    | HTML 데이터 수집  | `requests`, `BeautifulSoup`                        |
| JSON 파싱   | 데이터 구조화      | `json`                                             |
| DataFrame | 테이블 처리 및 시각화 | `pandas`                                           |
| Streamlit | 웹 앱 UI       | `streamlit`                                        |
| 슬라이더/버튼   | 인터랙티브 제어     | `st.slider`, `st.button`                           |
| 차트        | 시각화          | `st.bar_chart`, `st.line_chart`, `st.altair_chart` |
| 뉴스 기사 추출  | 텍스트/이미지 수집   | `trafilatura`                                      |
| AI 번역     | 영어 → 한국어     | `ollama`                                           |

---

## 7️⃣ 사용 순서 (실무 기준)

1. 가상환경 구성 → 모듈 설치
2. Streamlit 기본 구조 세팅
3. 데이터 수집
4. DataFrame 전처리
5. 시각화 (Streamlit 기본/Altair)
6. AI 처리 (뉴스 번역)
7. 다운로드/파일 관리

---


