## robotx.txt 외부 검색 노출 막기

작업 페이지가 네이버 검색에 노출 되었을때 수집 막기
- disallow 부분만 기재하면 아무것도 수집하지않음 
- allow 부분도 함께 추가해줘야한다 
- robots.txt는 루트디렉토리에 위치  `도메인/robots.txt`

```txt
User-agent: Googlebot
Allow: /
User-agent: Googlebot-News
Allow: /
User-agent: Googlebot-Image
Allow: /
User-agent: Googlebot-Mobile
Allow: /
User-agent: Mediapartners-Google
Allow: /
User-agent: Facebot
Allow: /
User-agent: Bingbot
Allow: /
User-agent: facebookexternalhit
Allow: /
User-agent: kakaotalk-scrap
Allow: /
User-agent: Naverbot
Allow: /
User-agent: Yeti
Allow: /
User-agent: *
Disallow: [허용하지 않는 url]

```

[네이버 레퍼런스](https://searchadvisor.naver.com/guide/seo-basic-robots)