# Agora STT(V2V) FAQ 및 트러블슈팅

최종 업데이트: 2026-07-16

## 시작하기

### STT(V2V) 서비스를 사용하려면 어떻게 해야 하나요?

먼저 Agora Console에서 **Conversational AI Engine**을 활성화해야 합니다. 활성화하지 않으면 API 호출 시 권한 오류가 발생합니다.

활성화 방법: Agora Console -> 프로젝트 선택 -> Conversational AI Engine -> 활성화(Enable)

## 지원 언어

언어 코드는 `source_language`와 `target_language` 설정에서 사용합니다. 표의 코드는 클릭형 HTML 페이지에서도 확인할 수 있습니다.

| Language | Code |
| --- | --- |
| Korean (Korea) | `ko-KR` |
| English (United States) | `en-US` |
| Vietnamese (Vietnam) | `vi-VN` |
| Chinese | `zh-CN` |
| Thai (Thailand) | `th-TH` |
| Japanese (Japan) | `ja-JP` |
| Spanish (Spain) | `es-ES` |
| French (France) | `fr-FR` |
| Filipino (Philippines) | `tl-PH` |
| Russian (Russia) | `ru-RU` |
| Afrikaans (South Africa) | `af-ZA` |
| Arabic | `ar-AE` |
| Norwegian | `nb-NO` |
| Catalan | `ca-ES` |
| Azerbaijani (Latin, Azerbaijan) | `az-AZ` |
| Belarusian | `be-BY` |
| Bulgarian (Bulgaria) | `bg-BG` |
| Bengali (India) | `bn-IN` |
| Bosnian (Bosnia and Herzegovina) | `bs-BA` |
| Czech (Czechia) | `cs-CZ` |
| Welsh (United Kingdom) | `cy-GB` |
| Danish (Denmark) | `da-DK` |
| German (Germany) | `de-DE` |
| Greek (Greece) | `el-GR` |
| Estonian (Estonia) | `et-EE` |
| Basque | `eu-ES` |
| Persian (Iran) | `fa-IR` |
| Finnish (Finland) | `fi-FI` |
| Galician | `gl-ES` |
| Gujarati (India) | `gu-IN` |
| Hebrew (Israel) | `he-IL` |
| Hindi (India) | `hi-IN` |
| Croatian (Croatia) | `hr-HR` |
| Hungarian (Hungary) | `hu-HU` |
| Indonesian (Indonesia) | `id-ID` |
| Italian (Italy) | `it-IT` |
| Kazakh (Kazakhstan) | `kk-KZ` |
| Kannada (India) | `kn-IN` |
| Lithuanian (Lithuania) | `lt-LT` |
| Latvian (Latvia) | `lv-LV` |
| Macedonian (North Macedonia) | `mk-MK` |
| Malayalam (India) | `ml-IN` |
| Marathi (India) | `mr-IN` |
| Malay (Malaysia) | `ms-MY` |
| Dutch (Netherlands) | `nl-NL` |
| Punjabi (India) | `pa-IN` |
| Polish (Poland) | `pl-PL` |
| Portuguese | `pt-PT` |
| Romanian (Romania) | `ro-RO` |
| Slovak (Slovakia) | `sk-SK` |
| Slovenian (Slovenia) | `sl-SI` |
| Albanian (Albania) | `sq-AL` |
| Serbian (Cyrillic, Serbia) | `sr-RS` |
| Swedish (Sweden) | `sv-SE` |
| Kiswahili (Kenya) | `sw-KE` |
| Kiswahili (Tanzania) | `sw-TZ` |
| Tamil (India) | `ta-IN` |
| Telugu (India) | `te-IN` |
| Turkish | `tr-TR` |
| Ukrainian | `uk-UA` |
| Urdu (India) | `ur-IN` |

## 자주 묻는 질문

### 이 API는 여러 언어를 지원하나요?

네, 지원합니다. 다만 각 언어별로 별도의 API(에이전트)를 시작해야 합니다.

### 하나의 채널에서 여러 에이전트를 동시에 사용할 수 있나요?

가능합니다. 각 에이전트에 고유한 agent name을 부여하면 동일 채널에서 병렬 운영됩니다.

### 통화 중 에이전트가 응답하지 않습니다.

아래 트러블슈팅 템플릿을 작성하여 지원팀에 공유해 주세요.

### API 응답은 200(성공)인데 에이전트가 동작하지 않습니다.

RTC 토큰 서버와 Agent 서버는 분리되어 운영됩니다. STT 서버에서 요청을 정상적으로 수신하여 200 응답을 반환하더라도, RTC 토큰에 문제가 있을 경우 에이전트는 채널에 참여하지 못합니다.

확인 사항: 토큰이 `agent_rtc_uid`에 맞게 발급되었는지, 토큰이 만료되지 않았는지 확인해 주세요.

### 하나의 worker에서 여러 언어를 동시에 번역할 수 있나요?

아니요. STT(V2V) 서비스는 품질과 안정성을 위해 **worker 1개당 1개 번역 언어**만 지원하도록 설계되어 있습니다. 다중 언어 자막이 필요한 경우, 언어별로 worker를 분리하여 각각 구성해야 합니다.

예시:

```text
"name": "translation_en" -> 한국어 -> 영어
"name": "translation_zh" -> 한국어 -> 중국어
```

### `agent_rtc_uid`와 `remote_rtc_uids`의 차이점은 무엇인가요?

`agent_rtc_uid`는 에이전트(번역 봇)의 UID입니다. 채널에 참여할 에이전트의 고유 ID를 지정합니다.

`remote_rtc_uids`는 번역 대상 유저의 UID 목록입니다. 에이전트가 음성을 수신할 실제 사용자의 UID를 지정합니다.

흔한 실수: `remote_rtc_uids`에 존재하지 않는 UID를 넣거나, 에이전트 자신의 UID를 넣는 경우가 있습니다. 반드시 채널에 참여 중인 유저의 UID를 지정해 주세요.

### 에이전트 토큰은 어떻게 발급해야 하나요?

`token`은 반드시 `agent_rtc_uid`에 해당하는 에이전트의 토큰으로 발급받아야 합니다.

예: `agent_rtc_uid`가 `"1002"`이면 UID 1002에 대한 토큰을 생성합니다.

주의: 유저의 토큰을 에이전트 토큰 자리에 사용하면 오류가 발생합니다.

### 동일 채널에서 여러 언어로 번역 에이전트를 시작하면 오류가 발생합니다.

각 에이전트의 `name` 필드에 고유한 값을 사용해야 합니다. 동일한 `name`으로 두 번째 에이전트를 시작하면 기존 에이전트와 충돌하여 `409 TaskConflict` 오류가 발생합니다.

해결 방법: 언어별로 고유한 name을 부여하세요.

```text
"name": "agora_translation_test_id_en" -> 영어 Agent
"name": "agora_translation_test_id_zh" -> 중국어 Agent
```

## 트러블슈팅 템플릿

문제 발생 시 아래 정보를 함께 공유해 주시면 빠른 해결이 가능합니다.

### 기본 정보

- 채널명: 예: `test_channel_01`
- 에이전트 ID: 예: `agent-abc123`
- App ID: 예: `aab236bhelshyg27`
- 발생 시각: 예: `2026-03-17 14:30 KST`

### RESTful API 정보

- API Endpoint: 예: `POST /v1/projects/{appid}/join`
- Request Body: JSON body
- Response Status Code: 예: `401`, `500`, `200`
- Response Body: 에러 메시지 전문 붙여넣기

### 추가 정보

- 환경: 예: Web SDK v4.23.0
- 문제 설명: 예: 에이전트가 채널에 참여한 후 음성 입력을 받지만 STT 변환 결과가 클라이언트에 전달되지 않음
- 에러 메시지: 예: `{"code": "AGENT_TIMEOUT", "message": "Agent failed to respond within 10s"}`
- 재현 절차:
  1. Console에서 App ID `aab236b***`로 프로젝트 생성
  2. POST `/v1/projects/{appid}/join` 호출하여 에이전트를 채널 `test_channel_01`에 참여시킴
  3. 클라이언트(Web SDK v4.23.0)에서 동일 채널에 입장
  4. 클라이언트가 음성을 전송하면 에이전트가 약 5초간 STT 변환 후 응답 없음
  5. 에러 확인
- 시도한 해결 방법: 예: 토큰 재발급, 다른 채널에서 테스트, `agent_rtc_uid` 변경 등
- 핵심 발견 사항: 예: 동일 설정으로 다른 App ID에서는 정상 동작, 특정 App ID에서만 문제 발생
- 관련 로그: 콘솔 로그, 네트워크 요청/응답, SDK 로그 등을 붙여넣기

로그 파일이나 스크린샷이 있으면 함께 첨부해 주세요. 정보가 상세할수록 빠른 해결이 가능합니다.

## 주요 에러 코드

| Code | 의미 | 조치 |
| --- | --- | --- |
| 200 | 성공 (주의) | 200이어도 토큰 문제 시 에이전트 미동작 가능. 토큰이 `agent_rtc_uid`에 맞게 발급되었고 만료되지 않았는지 확인하세요. |
| 401 | 인증 실패 | Token / Client ID / Client Secret을 재확인하세요. |
| 403 | 권한 없음 | 프로젝트 권한 및 IP 허용 목록을 확인하세요. |
| 409 | 작업 충돌 | 에이전트 `name`을 고유하게 설정하세요. |
| 404 | 리소스 없음 | Endpoint URL, App ID, RESTful API Parameters를 확인하세요. |
| 500 | 서버 오류 | 위 트러블슈팅 템플릿을 작성하여 지원팀에 문의하세요. |

## 리소스

- [STT(V2V) 데모](https://dl.agoralab.co/v2v/)
- [STT(V2V) 공식 문서](https://dl.agoralab.co/v2v/docs/#/ko/)
- [Agora Console](https://console.agora.io/)
