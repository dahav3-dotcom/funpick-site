# funpick-site

이지랩스(EZ Labs) 회사 소개 정적 사이트. GitHub Pages로 배포됩니다.

- 사이트 파일: `site/` (`index.html`, `assets/style.css`)
- 배포: `.github/workflows/pages.yml` — `main`에 `site/` 변경이 push되면 자동 배포
- 주소: https://funpick.kr (GitHub 기본 주소: https://dahav3-dotcom.github.io/funpick-site/)
- 도메인 점검: Actions의 `Domain check` 워크플로를 수동 실행하면 DNS·HTTPS 상태를 확인

## 내용 수정 위치 (`site/index.html`)

- 대학교 업무협약 실적: `mou-list` 안의 `<li>` 한 줄이 1건
- 문의 이메일: `contact-mail` 링크
- 하단 사업자 정보: `biz` 안의 각 `<dd></dd>`

## 메모

- AI 쇼츠 팩토리 프로 카드의 와디즈 링크(https://www.wadiz.io/web/campaign/detail/414137)는 10월 19일 펀딩 종료 후 교체 예정

## 커스텀 도메인

funpick.kr은 GitHub Pages에 연결합니다. 도메인 등록은 저장소 Settings → Pages → Custom domain에서 합니다.
호스팅.kr DNS에서 GitHub Pages로 향하는 줄은 아래뿐이며, tool.funpick.kr 등 다른 하위 주소와
메일(MX/TXT) 줄은 건드리지 않습니다.

- `@` A: 185.199.108.153, 185.199.109.153, 185.199.110.153, 185.199.111.153
- `www` CNAME: dahav3-dotcom.github.io
