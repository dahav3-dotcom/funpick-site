# funpick-site

이지랩스(EZ Labs) 회사 소개 정적 사이트. GitHub Pages로 배포됩니다.

- 사이트 파일: `site/` (`index.html`, `assets/style.css`)
- 배포: `.github/workflows/pages.yml` — `site/`가 바뀐 채로 push되면 자동 배포
- 미리보기 주소: https://dahav3-dotcom.github.io/funpick-site/

## 내용 수정 위치 (`site/index.html`)

- 대학교 업무협약 실적: `mou-list` 안의 `<li>` 한 줄이 1건
- 문의 이메일: `contact-mail` 링크
- 하단 사업자 정보: `biz` 안의 각 `<dd></dd>`

## 커스텀 도메인 (아직 연결 안 함)

funpick.kr 연결은 미리보기 확인 후 진행합니다. DNS는 funpick.kr 본 주소와 www만 바꾸고,
tool.funpick.kr 등 다른 하위 주소와 메일(MX/TXT) 줄은 건드리지 않습니다.
