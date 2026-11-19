# Urban Data Log

도시데이터 공부와 분석 아카이브 — GitHub Pages(Jekyll)로 돌아가는 개인 블로그.

- 주소: <https://yoontap-bit.github.io>
- 저장소: <https://github.com/yoontap-bit/yoontap-bit.github.io>

## 구조

```
_config.yml                    사이트 제목·설명·주소 등 기본 설정
index.html                     홈 (최근 글 목록)
posts.html                     /posts/ 전체 글 아카이브
about.md                       /about/ 소개
404.html                       없는 주소 안내
feed.xml                       RSS
_layouts/default.html          공통 뼈대 (헤더·푸터·수식·복사 버튼)
_layouts/post.html             글 한 편
_layouts/page.html             고정 페이지
assets/css/style.css           전체 스타일
assets/img/                    이미지
_posts/YYYY-MM-DD-slug.md      글
```

## 글 쓰는 법

`_posts/` 안에 `2026-11-19-글-주소.md` 형식으로 파일을 만든다. 날짜와 파일명이 곧 주소가 된다
(`/posts/2026/11/19/글-주소/`). 한글 파일명은 피하는 편이 안전하다.

```yaml
---
layout: post
title: "글 제목"
subtitle: "한 줄 부제 (선택)"
date: 2026-11-19 09:00:00 +0900
categories: [note]
tags: [도시데이터, 공간분석]
description: "검색·공유 미리보기에 쓰이는 한 줄 설명"
---
```

## 본문에서 쓰는 것들

**코드** — 백틱 세 개로 언어를 지정하면 자동으로 강조 표시되고, 블록 오른쪽 위에 복사 버튼이 뜬다.

**수식** — 문단 안에서는 `$ ... $`, 한 줄 전체는 `$$ ... $$`.
복잡한 TeX는 아래처럼 감싸면 마크다운이 건드리지 않는다.

```html
<div class="math-block" markdown="0">
$$
\rho_i = \frac{P_i}{A_i}
$$
</div>
```

**표** — 마크다운 표를 그대로 쓰면 되고, 폭이 넘치면 가로로 스크롤된다.

**그림** — `assets/img/`에 파일을 넣고 캡션과 함께:

```html
<figure>
  <img src="{{ '/assets/img/파일명.png' | relative_url }}" alt="설명">
  <figcaption>캡션</figcaption>
</figure>
```

**인터랙티브 지도** — iframe을 `.embed`로 감싸면 반응형으로 꽉 찬다.

```html
<div class="embed">
  <iframe src="https://..." title="지도" loading="lazy"></iframe>
</div>
```

**강조 박스** — `<div class="note"> … </div>`

## 제목·설명 바꾸기

`_config.yml`의 `title`, `tagline`, `description`만 고치면 헤더·푸터·탭 제목에 모두 반영된다.

## 로컬 미리보기 (선택)

```bash
bundle exec jekyll serve
```

Ruby가 없다면 GitHub에 푸시한 뒤 Actions/Pages 빌드 결과를 보면 된다.
Pages 설정에서 Source를 **Deploy from a branch → main → / (root)** 로 두면 별도 워크플로 없이 빌드된다.
