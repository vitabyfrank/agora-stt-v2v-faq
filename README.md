# Agora STT(V2V) — FAQ & Troubleshooting Guide

고객 대면용 Agora STT(V2V) FAQ 및 트러블슈팅 가이드입니다.

A bilingual (Korean / English) FAQ and troubleshooting reference for the **Agora Speech-to-Speech Translation (V2V)** service.

---

## 개요 | Overview

Agora STT V2V API의 자주 묻는 질문, 설정 시 흔한 실수, 에러 해결 방법을 정리한 단일 HTML 페이지입니다.

Single-page HTML document covering common questions, configuration pitfalls, and error resolution for the Agora STT V2V API.

## 구성 | Sections

| 섹션 | Section | 설명 / Description |
|------|---------|-------------------|
| 시작하기 | Getting Started | Agora Console에서 서비스 활성화 |
| 사용량 | Usage | STT(V2V) 사용량 확인 방법 |
| 자주 묻는 질문 | FAQ | 다국어, 멀티에이전트, UID/토큰, 409 충돌, 200 성공인데 실패 |
| 트러블슈팅 템플릿 | Troubleshooting | 이슈 리포트 양식 |
| 주요 에러 코드 | Error Codes | 200, 401, 403, 404, 409, 500 |
| 리소스 | Resources | 데모 및 공식 문서 링크 |

## 주요 기능 | Features

- **한/영 전환** — 한국어 ↔ English 토글
- **사이드바 네비게이션** — 스크롤 인식 active 상태 + 클릭 시 화면 중앙 이동
- **하이라이트** — 클릭한 항목에 시각적 강조 박스 표시
- **반응형** — 모바일 대응 (접이식 사이드바)
- **인쇄 지원** — 인쇄 시 사이드바/토글 자동 숨김
- **무의존성** — 순수 HTML/CSS/JS, 빌드 불필요

## 사용법 | Usage

브라우저에서 `agora-qna.html`을 열거나, 정적 파일 서버로 실행하세요.

Open `agora-qna.html` in any browser, or serve via any static file server:

```bash
# Python
python3 -m http.server 5500

# Node.js
npx serve .
```

## 리소스 | Resources

- [STT(V2V) 데모 / Demo](https://dl.agoralab.co/v2v/)
- [STT(V2V) 공식 문서 / Documentation](https://dl.agoralab.co/v2v/docs/#/ko/)
- [Agora Console](https://console.agora.io/)
