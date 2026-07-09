# 뉴슐랭 맵 운영 가이드

## 파일 위치
`/Users/ahnsoeun/Desktop/ai workspace/newchelin-map/index.html`

---

## 기술 스택
- React 18 (CDN) + Tailwind CSS
- 지도: **Leaflet + OpenStreetMap** (무료, API 키 없음, 해외 접속 가능)
- 폰트: Noto Sans KR + Poppins (헤더 타이틀)
- 파비콘: `favicon.png` (루트에 위치, 교체 시 같은 파일명으로 덮어쓰기)
- 단일 `index.html` 파일로 구성

---

## 현재 데이터 (Vol.01~03, 총 10곳)

| Vol | 호수 | 가게명 | 카테고리 |
|-----|------|--------|---------|
| Vol.01 | 8호 | 토가라시 | 맛집 |
| Vol.01 | 8호 | 류몽민 | 맛집 |
| Vol.01 | 8호 | 홍명 | 맛집 |
| Vol.01 | 8호 | 용삼계탕 | 맛집 |
| Vol.02 | 9호 | 커피휘엘 | 카페 |
| Vol.02 | 9호 | 카페 수목금토 | 카페 |
| Vol.02 | 9호 | 아임뮤트로스터리 | 카페 |
| Vol.03 | 10호 | 현지식당 | 맛집 |
| Vol.03 | 10호 | 사랑채 | 맛집 |
| Vol.03 | 10호 | 박성희가요리하는집 | 맛집 |

---

## Vol별 핀·UI 색상
- Vol.01 → 빨강 `#E74C3C`
- Vol.02 → 파랑 `#3B82F6`
- Vol.03 → 노랑 `#F59E0B`
- 새 Vol 추가 시 `VOL_COLORS` 객체에 색상 추가 필요

---

## Vol별 뉴스레터 링크
카드의 "관련 뉴스레터 보기" 버튼은 `ISSUE_LINKS` 객체에서 호수(issue) 기준으로 매핑됩니다.
```js
const ISSUE_LINKS = {
  8: 'https://stibee.com/api/v1.0/emails/share/oWNEWkGCBhXlOqQtuAOKTH-3bZ0kELk',   // Vol.01
  9: 'https://stibee.com/api/v1.0/emails/share/do_AFKVMdD0oa6Tx38YA7naFmnXl4n8',   // Vol.02
  10: 'https://stibee.com/api/v1.0/emails/share/D-_JkInAShqb8GMj5QZji4agQ1tCLpA',  // Vol.03
};
```
새 Vol 추가 시 해당 호수의 뉴스레터 공유 링크를 `ISSUE_LINKS`에 추가해야 버튼이 노출됩니다.

---

## 새 식당 추가 방법

### 1. 좌표 찾기
1. Google 지도에서 가게 검색
2. 핀 위치 우클릭
3. 뜨는 좌표 복사 (예: `37.514, 127.026`)

### 2. Claude에게 전달할 정보
새 세션에서 아래 형식으로 요청:

```
/Users/ahnsoeun/Desktop/ai workspace/newchelin-map/index.html
여기에 새 식당 추가해줘:
- 이름: 
- 카테고리: 맛집 or 카페
- Vol: Vol.04
- 호수: 11
- 한 줄 설명: 
- 상세 설명: 
- 추천 메뉴: 
- 주소: 
- 영업시간: 
- 전화번호: (없으면 생략 가능)
- 네이버 링크: 
- 좌표: lat: , lng: 
- 해당 호수 뉴스레터 공유 링크: (ISSUE_LINKS에 새로 추가 필요)
```

### 3. index.html 내부 PLACES 배열 구조 참고
```js
{
  id: 'unique-id',           // 영문 하이픈 (고유값)
  name: '식당명',
  category: 'restaurant',   // 맛집: restaurant / 카페: cafe
  vol: 'Vol.04',
  issue: 11,                 // 뉴스레터 호수 (ISSUE_LINKS 매핑 키와 일치해야 함)
  oneLiner: '한 줄 설명',
  description: '2~3문장 상세 설명',  // 카드에는 미노출, 데이터로만 보관
  recommendedMenu: ['메뉴1', '메뉴2'],
  address: '도로명 주소',
  hours: '영업시간',          // 없으면 필드 생략 (카드에 🕒 라인 자체가 안 보임)
  phone: '02-000-0000',      // 없으면 빈 문자열 '' 가능
  lat: 37.000000,
  lng: 127.000000,
  naverUrl: 'https://naver.me/xxxxx',
}
```

---

## 풋터 링크
- 뉴스레터 구독: https://page.stibee.com/subscriptions/437650
- 홈페이지: https://www.its-newid.com/
- 데스크톱(사이드바)·모바일 풋터 모두 동일하게 반영됨

---

## 배포 정보
- **라이브 URL**: https://newchelin-map.vercel.app
- **GitHub**: https://github.com/ahnsoeun/newchelin-map
- **호스팅**: Vercel (GitHub 연동, 자동 배포)

## 업데이트 배포 방법
index.html 수정 후 터미널에서:
```
cd "/Users/ahnsoeun/Desktop/ai workspace/newchelin-map" && git add . && git commit -m "식당 추가" && git push
```
push 하면 Vercel이 자동으로 반영됩니다.
