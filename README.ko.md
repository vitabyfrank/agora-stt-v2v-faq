# Agora STT(V2V) — FAQ 및 트러블슈팅 가이드

> **[English README](./README.md)**

고객 대면용 **Agora STT(V2V)** FAQ 및 트러블슈팅 가이드입니다.

## 개요

Agora STT V2V API의 자주 묻는 질문, 설정 시 흔한 실수, 에러 해결 방법을 정리한 단일 HTML 페이지입니다. 한국어/영어/일본어/중국어 간체 전환과 AI 친화적인 언어별 Markdown FAQ 보기 및 다운로드를 지원합니다.

## 구성

| 섹션 | 설명 |
|------|------|
| 시작하기 | Agora Console에서 Conversational AI Engine 활성화 |
| 자주 묻는 질문 | 다국어 지원, 멀티에이전트, UID/토큰 설정, 409 충돌, 200 성공인데 실패 |
| 트러블슈팅 템플릿 | 이슈 리포트 양식 |
| 주요 에러 코드 | 200, 401, 403, 404, 409, 500 및 조치 방법 |
| 리소스 | 데모 및 공식 문서 링크 |

## 주요 기능

- **다국어 전환** — 한국어 / English / 日本語 / 简体中文 토글
- **AI 친화 Markdown 보기/다운로드** — 현재 언어의 Markdown FAQ를 새 탭으로 열거나 명확한 파일명으로 다운로드
- **사이드바 네비게이션** — 스크롤 인식 active 상태 + 클릭 시 화면 중앙 이동
- **하이라이트** — 클릭한 항목에 시각적 강조 박스 표시
- **반응형** — 모바일 대응 (접이식 사이드바)
- **인쇄 지원** — 인쇄 시 사이드바/토글 자동 숨김
- **무의존성** — 순수 HTML/CSS/JS, 빌드 불필요

## 사용법

브라우저에서 `index.html`을 열거나, 정적 파일 서버로 실행하세요.

```bash
# Python
python3 -m http.server 5500

# Node.js
npx serve .
```

## 리소스

- [STT(V2V) 데모](https://dl.agoralab.co/v2v/)
- [STT(V2V) 공식 문서](https://dl.agoralab.co/v2v/docs/#/ko/)
- [Agora Console](https://console.agora.io/)

## Markdown FAQ 파일

- [한국어](./faq.kr.md)
- [English](./faq.en.md)
- [日本語](./faq.ja.md)
- [简体中文](./faq.zh.md)
