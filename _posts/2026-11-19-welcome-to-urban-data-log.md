---
layout: post
title: "Urban Data Log를 시작하며"
subtitle: "도시데이터 공부와 과제를 쌓아 두는 아카이브"
date: 2026-11-19 09:00:00 +0900
categories: [log]
tags: [도시데이터, 공부기록, 시작]
description: "도시데이터 공부와 과제를 흩어지지 않게 쌓아 두기 위해 만든 기록. 블로그의 목적과 글 형식, 그리고 앞으로 다룰 것들."
---

## 왜 아카이브인가

수업 필기는 노트북 여기저기로 흩어지고, 과제 코드는 `final_v3_진짜최종` 같은 이름만 남는다.
한 학기가 지나면 "그때 그 데이터 어떻게 정리했더라"를 다시 처음부터 찾게 된다.
그 반복을 줄이려고 글을 쌓기로 했다.

노트를 예쁘게 남기는 게 목적은 아니다. **다시 꺼내 쓸 수 있게** 남기는 게 목적이다.
그래서 이 블로그는 설명보다 기록에 가깝게 운영한다.

## 여기에 남길 것

- **강의 노트** — 배운 개념을 내 문장으로 다시 쓴 정리. 정의, 예시, 헷갈린 지점까지.
- **과제 기록** — 문제 정의부터 데이터 수집, 분석, 시각화, 그리고 막혔던 부분과 해결.
- **지도와 시각화** — 공간 데이터를 어떻게 읽고 보여 줄지에 대한 짧은 실험들.

## 글의 형식

글마다 아래 네 칸을 채우는 걸 기본으로 한다. 길이는 상관없다.

1. **문제** — 무엇을 알고 싶었는지 한 문단으로
2. **데이터** — 출처, 기간, 단위, 전처리에서 버린 것
3. **방법** — 쓴 도구와 판단의 이유
4. **결과와 회고** — 숫자, 그림, 그리고 다음에 다르게 할 점

이 형식이 지켜지면 나중에 읽어도 맥락이 살아 있다.

---

## 앞으로 쓸 것들 — 렌더링 확인

이 블로그에서 코드, 수식, 표, 그림, 지도가 깨지지 않는지 첫 글에서 직접 확인해 둔다.

### 코드

```python
import geopandas as gpd
import pandas as pd

districts = gpd.read_file("data/seoul_districts.geojson")
districts["area_km2"] = districts.geometry.area / 1_000_000
districts["density"] = districts["population"] / districts["area_km2"]

top5 = (
    districts[["name", "population", "area_km2", "density"]]
    .sort_values("density", ascending=False)
    .head(5)
)
print(top5.to_string(index=False))
```

셸 명령도 같은 방식으로 들어간다.

```bash
# 좌표계를 먼저 확인하고 면적을 계산할 것
ogrinfo -so data/seoul_districts.geojson seoul_districts | grep -i "EPSG"
```

### 수식

지역 $i$의 인구밀도는 인구를 면적으로 나눈 값이다. 단위가 사람/㎢인지 확인하는 걸 잊지 말자.

<div class="math-block" markdown="0">
$$
\rho_i = \frac{P_i}{A_i}
\qquad
\bar{\rho} = \frac{\sum_{i} P_i}{\sum_{i} A_i}
$$
</div>

전체 평균 밀도는 밀도의 평균이 아니라 **인구 합을 면적 합으로 나눈 값**이라는 점이 늘 헷갈린다.
가중평균으로 쓰면 아래와 같다.

<div class="math-block" markdown="0">
$$
\bar{\rho} = \sum_{i} w_i \rho_i,
\qquad
w_i = \frac{A_i}{\sum_{j} A_j}
$$
</div>

### 표

| 자치구 | 인구(명) | 면적(㎢) | 인구밀도(명/㎢) |
| --- | ---: | ---: | ---: |
| 종로구 | 142,000 | 23.9 | 8,700 |
| 중구 | 122,000 | 11.0 | 11,200 |
| 성동구 | 291,000 | 16.9 | 19,400 |
| 동대문구 | 341,000 | 14.2 | 21,300 |
| 강남구 | 528,000 | 39.5 | 14,100 |

*값은 표 형식을 보여 주기 위한 예시이며 실제 통계와 다를 수 있다.*

### 그림

<figure>
  <img src="{{ '/assets/img/example-density.svg' | relative_url }}" alt="서울 5개 자치구의 인구밀도를 비교한 예시 막대그래프">
  <figcaption>자치구별 인구밀도 비교 — 정적 그림은 이렇게 캡션과 함께 넣는다.</figcaption>
</figure>

### 지도

인터랙티브 지도는 iframe으로 그대로 붙는다. 아래는 OpenStreetMap 임베드 예시(서울시청 일대)다.
지도를 조작해 보면 블로그 본문 안에서도 스크롤과 확대가 되는 걸 확인할 수 있다.

<div class="embed">
  <iframe src="https://www.openstreetmap.org/export/embed.html?bbox=126.9600%2C37.5510%2C127.0010%2C37.5790&amp;layer=mapnik&amp;marker=37.5665%2C126.9780" title="서울시청 일대 지도" loading="lazy" referrerpolicy="no-referrer-when-downgrade"></iframe>
</div>
<p class="embed-note">지도 데이터 © OpenStreetMap 기여자</p>

<div class="note">
<p><strong>메모.</strong> kepler.gl나 Leaflet으로 만든 지도도 HTML 파일 하나로 올려 두고 iframe으로 불러오면 그대로 동작한다. 용량이 큰 데이터는 미리 경량화해서 넣을 것.</p>
</div>

---

## 다음 글

서울 행정동 경계 데이터로 인구밀도 격자를 만들고, 같은 값을 두 가지 방식(면적 기준·격자 기준)으로
비교해 본 기록을 먼저 올릴 예정이다. 그다음은 과제로 했던 대중교통 접근성 분석.

읽는 사람이 나 혼자여도 상관없다. 쌓이면 지도가 된다.
