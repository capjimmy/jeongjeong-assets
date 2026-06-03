# 적정온도 / 설정온도 동적 자산

KOC 오피스컨설팅이 운영하는 자매 브랜드 두 스마트스토어의 상세페이지에서 사용하는 동적 이미지·매니페스트 호스팅.

## 🎯 목적

스마트스토어 상세페이지는 한 번 등록하면 수정이 번거롭지만, **이미지의 URL이 같으면 이미지 파일만 덮어써도 모든 상품에서 자동으로 새 이미지가 보입니다**. 이 저장소는 그 동적 갱신 메커니즘을 제공합니다.

## 📁 구조

```
recommend/
  ├── jeongjeong/          # 적정온도 추천 상품 이미지
  │   ├── rc610.jpg
  │   ├── rc620-30kf.jpg
  │   ├── rub6200.jpg
  │   └── manifold.jpg
  └── seoljeong/           # 설정온도 추천 상품 이미지
banners/                   # 메가 배너 동적 갱신
manifest.json              # 추천 상품 메타데이터 (모델/가격/별점/URL)
```

## 🌐 URL 패턴

### GitHub Raw (이미지 직접 접근)
```
https://raw.githubusercontent.com/capjimmy/jeongjeong-assets/main/recommend/jeongjeong/rc620-30kf.jpg
```

### GitHub Pages (활성화 후)
```
https://capjimmy.github.io/jeongjeong-assets/recommend/jeongjeong/rc620-30kf.jpg
```

## 🔄 추천 상품 갱신 방법

### 1. 이미지만 교체 (가장 빠름)
1. 새 베스트셀러 이미지를 같은 파일명으로 덮어쓰기
   ```
   recommend/jeongjeong/rc620-30kf.jpg  ← 새 이미지로 교체
   ```
2. Git push
3. 모든 스마트스토어 상품 페이지에 자동 반영 (최대 24시간 캐시)

### 2. manifest.json 업데이트
- 가격·별점·리뷰 수 등 메타데이터 변경
- `version`과 `updated` 날짜 갱신
- 자체 호스팅 페이지(미리보기 등)에서 자동 반영

### 3. 캐시 갱신 트릭
HTML 안 `<img>` URL에 버전 파라미터 추가:
```html
<img src=".../rc620-30kf.jpg?v=20260604">
```
- 새 버전 적용 시 `?v=` 값만 변경
- 브라우저/CDN 캐시 우회

## 🔗 스마트스토어 상세설명에 임베드

```html
<a href="https://smartstore.naver.com/jeongjeong-ondo/products/PRODUCT_ID">
  <img src="https://raw.githubusercontent.com/capjimmy/jeongjeong-assets/main/recommend/jeongjeong/rc620-30kf.jpg"
       alt="RC620-30KF 대형 평수 추천" />
</a>
```

- 클릭 URL은 하드코딩 (실제 스마트스토어 상품 URL)
- 이미지는 GitHub Raw에서 동적 로드

## ⚠️ 제약 사항

- **네이버 스마트스토어 상세설명은 JavaScript 차단** → JSON 동적 로드는 자체 호스팅 페이지에서만 가능
- **iframe 외부 도메인 차단** → GitHub Pages iframe 임베드는 불가
- **이미지 외부 호스팅은 허용** → 이미지 src로 GitHub Raw 사용 가능
- **추천 상품의 클릭 URL은 본문 안에 하드코딩** → URL 변경 시 본문 수정 필요

## 📋 운영 체크리스트

- [ ] 적정온도 12 SPU 스마트스토어 등록 후 실제 URL 12개 수집
- [ ] manifest.json의 `url_placeholder`를 실제 URL로 교체
- [ ] 자체 호스팅 페이지(insta.korea-officeautomation.com)에서 동적 렌더링 테스트
- [ ] 매주 금요일 베스트셀러 분석 → 추천 이미지 교체
- [ ] 시즌별 메가 배너 (가을 점검·겨울 무이자) 교체 일정

## 🛠 운영자

KOC 오피스컨설팅 (박민경 대표)
- 적정온도 (린나이 파주대리점)
- 설정온도 (모든 보일러 전문)
